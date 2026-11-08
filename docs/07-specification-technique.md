# 07 — Spécification technique

Implémentation de référence du protocole. Tout ce qui est ici peut être remplacé par une autre implémentation respectant les docs 04, 05 et 06.

## 1. Principes d'architecture

- **Local-first** : tout dans `.vigie/` du dossier courant (workspace). Aucun service distant du projet.
- **Déterministe d'abord** : pipeline de données classique (fetch → parse → normalize → rules → aggregate) ; le LLM n'intervient que dans des étapes nommées, avec schéma de sortie.
- **Content-addressed** : tout document archivé par empreinte ; les rapports référencent des empreintes.
- **Petit cœur, gros commons** : le paquet `vigie` doit rester lisible en une journée. La valeur est dans `playbooks/`, `connectors/`, `catalogue`.
- **Remplaçable** : runner, store, renderer et provider LLM sont derrière des interfaces.

## 2. Stack

| Besoin | Choix | Pourquoi |
|--------|-------|----------|
| Langage | Python ≥ 3.12 | écosystème data, `followthemoney` natif, accessible aux contributeurs journalistes-data |
| Packaging | `uv`, `pyproject.toml`, publication PyPI | installation en une commande |
| CLI | `typer` + `rich` | UX soignée sans effort |
| Modèles | `pydantic` v2 | validation, schémas JSON exportables pour le protocole |
| Stockage | `duckdb` (requêtes analytiques sur DECP national) + fichiers | pas de serveur, rapide sur des Go |
| Entités | `followthemoney` | interop OpenAleph / OpenSanctions |
| Marchés | modèle interne OCDS 1.1 (sous-ensemble) | interop internationale |
| HTTP | `httpx` avec cache conditionnel | ETag / Last-Modified |
| PDF/OCR | `pypdf`/`pdfplumber` ; OCR via `ocrmypdf`/`tesseract` si installés (optionnel) | PV de conseil |
| Rapports | `jinja2` → HTML autonome (CSS inline, pas de JS externe) ; Markdown ; CSV | lisible sans Vigie, archivable |
| LLM | `vigie.llm.Provider` : implémentations `anthropic`, `openai_compatible` (couvre Ollama, vLLM, etc.) | agnostique |
| Tests | `pytest`, `hypothesis` pour les règles, snapshots pour les rapports | |
| i18n | `gettext` (`.po`), français par défaut | |
| Qualité | `ruff`, `mypy --strict` sur `vigie/` | |

Pas de framework d'agents. Pas de base vectorielle en v1 (la recherche sémantique n'est pas nécessaire aux règles ; elle pourra servir aux PV en phase 2, en local).

## 3. Arborescence du dépôt

```
vigie/                      # paquet Python (cœur)
  cli/                      # commandes typer
  protocol/                 # version du protocole, schémas pydantic des objets (Document, Evidence, Signal, Report), schémas de sortie LLM
  store/                    # content-addressed store, manifestes, provenance
  connectors/               # base + implémentations par juridiction (fr/, eu/, ocds/)
  models/                   # Institution, Contract (OCDS), entités FtM, Deliberation
  normalize/                # DECP→OCDS, SIRENE→FtM, dédoublonnage, résolution d'entités
  rules/                    # une règle par fichier : A/a1.py … ; registre ; interface Rule
  aggregate/                # strates, percentiles, combinaisons, deltas
  playbook/                 # parseur Markdown+YAML, runner, points de contrôle
  llm/                      # Provider, gabarits de prompts versionnés, journalisation, ancrage
  report/                   # assemblage JSON canonique, rendu HTML/MD/CSV/FtM
  lint/                     # vocabulaire, ancrage
  verify/                   # rejeu et vérification
  i18n/
config/                     # seuils, paramètres de règles, strates, calendriers, tables (CPV, NAF↔CPV)
connectors/                 # YAML des connecteurs + fiches d'institutions (commons)
playbooks/                  # playbooks (commons)
catalogue/                  # fiches de règles au format machine (générées depuis docs/03 ou source de vérité ? → voir §6)
templates/                  # modèles CADA, questions, HTML du rapport
docs/
tests/
  fixtures/                 # données synthétiques (JAMAIS de données réelles nominatives)
  rules/ connectors/ playbooks/ golden/
```

## 4. Modèle de données (résumé)

- `Institution` : siren, nom, type (`commune`, `epci`, `departement`, `region`, `etablissement`, `entreprise_publique`, `association`, `etat`), code_insee, population, strate, sites[], satellites[] (liens institution→institution typés).
- `Contract` (OCDS sous-ensemble) : ocid, buyer, suppliers[], title, cpv, procurementMethod, procurementMethodDetails (procédure nationale), nature, value {amount, currency, is_maximum}, period, dateSigned/notification, numberOfTenderers (nullable), amendments[], source_ids[].
- `Organization` (FtM `Company`/`PublicBody`) : siren, siret, name, incorporationDate, status, naf, headcount_band, address (normalisée), source_ids[].
- `Person` (FtM `Person`) : **uniquement** avec `role` ∈ {`elu`, `dirigeant_inscrit`} et `institution`/`organization` de rattachement, et seulement produite par les connecteurs autorisés (doc 09). Jamais d'autres attributs que nom et rôle.
- `Deliberation` (phase 2) : séance (date, institution), numéro, objet, montant, organisation(s) citée(s), votes {pour, contre, abstention}, déports mentionnés (booléen + extrait), preuve.
- `Document`, `Evidence`, `Signal`, `Report` : doc 06.

Tous les objets portent `source_ids` et sont reliés à des `Document` via des `Evidence` dès leur création (les parseurs produisent des preuves `deterministe:*`).

## 5. Interfaces clés

```python
class Connector(Protocol):
    id: str
    def fetch(self, params: FetchParams, store: Store) -> list[DocumentRef]: ...
    def parse(self, doc: Document) -> ParsedBatch: ...   # entités + preuves déterministes

class Rule(Protocol):
    code: str; family: str; version: str; default_severity: Severity
    requires: set[str]                                  # champs/entités nécessaires
    def apply(self, ctx: RuleContext) -> list[Signal]: ...  # fonction pure sur ctx (entités, config, strate)

class Provider(Protocol):
    def complete(self, template: PromptTemplate, inputs: dict, schema: type[BaseModel]) -> tuple[BaseModel, LLMCall]: ...

class Renderer(Protocol):
    def render(self, report: Report, fmt: Literal["html","md","csv","ftm"]) -> bytes: ...
```

`RuleContext` expose : contrats, organisations, institution, strate (statistiques nationales pré-calculées), config effective, et une fabrique de preuves `ctx.evidence(document, locator, extract)`. Une règle ne fait pas d'I/O.

## 6. Catalogue : source de vérité

`docs/03-…md` est la source de vérité **humaine**. Un script `scripts/catalogue_sync.py` en extrait `catalogue/<code>.yaml` (sévérité, phase, fiche : pourquoi / explications / vérifier ensuite) et échoue en CI si une règle implémentée n'a pas de fiche ou si une fiche a changé sans bump de version. Le rapport copie les fiches depuis `catalogue/`, jamais depuis un prompt.

## 7. Store

- `.vigie/store/<aa>/<sha256>` : contenu brut. `.vigie/store/index.duckdb` : table `documents` (id, sha256, mime, source, provenance JSON, parent, methode).
- `.vigie/data/<source>.duckdb` ou Parquet : données parsées, requêtables (DECP national = plusieurs millions de lignes ; prévoir un chargement filtré par acheteur + un mode « national » pour les strates, avec cache).
- `.vigie/audits/<id>/` : `report.json`, `journal.jsonl`, exports.
- `.vigie/marks.duckdb` : marques utilisateur (confirmé / expliqué / faux positif), jamais exportées sans action explicite.

## 8. Strates et statistiques nationales

Les percentiles exigent des données nationales. Stratégie : `vigie strata build fr --periode …` télécharge les DECP complètes, calcule par strate et par période les distributions des indicateurs des règles (part d'offre unique, part sans publicité, HHI, part d'avenants…), et stocke un **résumé** (`.vigie/strata/fr-<periode>.parquet`, quelques Mo). Ce résumé peut être **publié dans le dépôt** (données agrégées non nominatives) pour que les utilisateurs n'aient pas à télécharger les DECP complètes. Mise à jour trimestrielle par CI.

## 9. LLM : gabarits et ancrage

- Gabarits versionnés dans `vigie/llm/templates/<nom>.v<n>.md` ; empreinte dans le journal.
- Température 0 quand le fournisseur le permet ; sorties en JSON validé par schéma pydantic ; en cas d'échec de validation, une relance avec l'erreur, puis abandon de l'étape (le rapport sort sans texte rédigé, avec mention).
- Ancrage : la sortie `Rapport.redaction` est `list[PhraseAncree{texte, signaux[], preuves[]}]` ; le linter (§10) tourne avant assemblage.
- Extraction (phase 2) : `Seance` avec, pour chaque champ, `preuve: {page, extrait}` obligatoire ; relocalisation de l'extrait dans le texte du document par recherche exacte puis floue (seuil configurable) ; un champ dont l'extrait n'est pas retrouvé est rejeté.
- Résolution d'entités : le LLM propose, un algorithme déterministe (normalisation + distance) score, l'humain confirme au point de contrôle si le signal est fort.

## 10. Linter

`vigie lint` : (a) vocabulaire interdit (liste dans `vigie/lint/vocabulaire.fr.yaml`, avec variantes et accords), (b) ancrage (doc 06 §2), (c) textes fixes conformes, (d) absence de données de personnes hors règles. Retourne des erreurs localisées. Intégré à `audit`, `report` et au CI sur les rapports golden.

## 11. Commandes (contrat)

| Commande | Effet |
|----------|-------|
| `vigie init` | crée `.vigie/`, `config` locale, détecte le fournisseur LLM (env `VIGIE_LLM_PROVIDER`, `VIGIE_LLM_MODEL`, clés) |
| `vigie sources list` / `vigie connector validate` | |
| `vigie fetch <connector> [filtres]` | télécharge + archive, idempotent |
| `vigie strata build <pays>` | statistiques nationales par strate |
| `vigie audit --playbook <id> --commune/--siren … [--periode] [--batch]` | exécute le playbook ; interactif par défaut |
| `vigie report <audit> --format html\|md\|csv\|ftm` | rend ; passe par `lint` et `verify` |
| `vigie verify <report.json>` | doc 06 §3 |
| `vigie lint <report.json>` | |
| `vigie diff <audit1> <audit2>` | delta |
| `vigie mark <signal> confirme\|explique\|faux-positif [-m]` | marque locale |
| `vigie feedback export` | export anonymisé des marques (code règle, strate, marque) |
| `vigie playbook validate` / `vigie playbook list` | |

Codes de sortie stables (0 ok, 2 validation, 3 vérification, 4 source indisponible) pour l'usage par lot.

## 12. Tests et CI

- Règles : par règle, vrais positifs + faux positifs connus sur fixtures synthétiques ; `hypothesis` pour les invariants (ex. B2 ne se déclenche jamais si un marché individuel dépasse le seuil).
- Connecteurs : parsing sur extraits ; provenance.
- Playbooks : scénario de bout en bout sur un workspace de test avec sources locales (`acces.type: fichier_local`).
- Golden : 3 rapports synthétiques complets ; snapshot JSON (hors horodatages) ; `verify` et `lint` doivent passer.
- CI : ruff, mypy, pytest, `catalogue_sync --check`, `playbook validate` et `connector validate` sur tout le commons, scan « données personnelles » sur `tests/fixtures/` (regex SIREN/SIRET réels interdits hors liste blanche d'institutions publiques).
- Jamais d'appel réseau en CI sauf job nocturne optionnel `--online` sur les connecteurs.

## 13. Distribution

- `pipx install vigie` / `uv tool install vigie`.
- Binaire autonome (PyInstaller) pour Windows/macOS en v1.
- Objectif v1.1 : installateur graphique minimal (double-clic, choix du modèle, champ « votre commune »), parce que le citoyen curieux n'ouvre pas de terminal.
- Images Docker non prioritaires (local-first, pas serveur).

## 14. Sécurité et vie privée

- Aucune télémétrie. Aucun appel réseau hors connecteurs et fournisseur LLM choisi.
- Les clés d'API ne sont jamais écrites dans un rapport ni un journal.
- Les rapports n'incluent pas le chemin local ni l'identité de l'utilisateur.
- Avertissement explicite dans `init` : utiliser un fournisseur LLM commercial envoie les données publiques traitées à ce fournisseur ; un modèle local n'envoie rien.
- Supply chain : dépendances épinglées, `uv.lock` commité, vérification des empreintes.

## 15. Performances cibles

Audit communal (strates pré-calculées, DECP filtrées) : collecte < 2 min, règles < 10 s, rédaction < 2 min avec modèle commercial, < 10 min avec un modèle local 8B sur CPU. Lot de 40 communes en `--batch` : < 30 min hors rédaction.

## 16. Décisions reportées

- Recherche sémantique locale pour les PV (phase 2) — choix de l'embedding local à faire.
- Interface web locale (`vigie serve` sur localhost) pour la lecture et les marques — phase 3, seulement si la demande est forte ; le HTML autonome suffit en v1.
- Signature cryptographique des rapports (Sigstore/minisign) pour attester l'auteur sans serveur — phase 3.
