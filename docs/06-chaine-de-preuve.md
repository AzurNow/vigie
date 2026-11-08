# 06 — Chaîne de preuve (protocole, v0.1)

La chaîne de preuve est ce qui distingue Vigie d'un générateur de soupçons. Elle garantit que **toute affirmation d'un rapport peut être retracée jusqu'à un document public, par n'importe qui, sans faire confiance ni à Vigie ni à l'auteur du rapport**. Elle est conçue pour résister à deux menaces : l'erreur (de donnée, de règle, de modèle) et l'attaque (procès en diffamation, contestation de bonne ou mauvaise foi).

Principe : *rien n'est affirmé qui ne soit cité ; rien n'est cité qui ne soit archivé ; rien n'est archivé qui ne soit empreint.*

## 1. Objets

### Document
```json
{
  "id": "doc_<sha256 tronqué 16>",
  "sha256": "…",
  "taille": 123456,
  "mime": "application/json",
  "source": "fr/decp",
  "connecteur_version": "0.1.0",
  "provenance": {
    "url_demandee": "…",
    "url_finale": "…",
    "date_heure": "2026-11-07T16:02:11Z",
    "etag": "…", "last_modified": "…",
    "user_agent": "vigie/0.1.0 (+…)"
  },
  "derives": [ {"id": "doc_…", "methode": "ocr:tesseract:5.3", "parent": "doc_<pdf>"} ]
}
```
Un document est immuable. Un dérivé (OCR, conversion, extraction texte d'un PDF) est un document à part entière qui référence son parent et la méthode. Le store est **adressé par contenu** : `.vigie/store/<2 premiers hex>/<sha256>`.

### Preuve (Evidence)
```json
{
  "id": "ev_000123",
  "document": "doc_…",
  "localisation": {
    "type": "json_path | page_offset | ligne | cellule | regex_match",
    "valeur": "$.marches[17].attributaires[0].siret",
    "page": 12, "offset_debut": 3041, "offset_fin": 3120
  },
  "extrait": "texte ou valeur exacte copiée du document",
  "extrait_sha256": "…",
  "methode": "deterministe:decp.parse | llm:extraction | humain:confirmation",
  "llm": null
}
```
L'`extrait` est **copié tel quel** ; son empreinte permet de vérifier qu'il n'a pas été altéré. Si la méthode est `llm:*`, le champ `llm` est renseigné (voir §4).

### Signal
```json
{
  "id": "SIG-2026-000017",
  "regle": {"code": "B2", "version": "0.1.0", "catalogue": "0.1.0"},
  "severite": "forte",
  "confiance": "haute",
  "sujet": {"type": "institution", "siren": "…"},
  "objets": [{"type": "contract", "ocid": "…"}, {"type": "contract", "ocid": "…"}],
  "constat": {
    "valeurs": {"somme": 187400, "seuil": 221000, "fenetre_mois": 12, "attributaire_siret": "…"},
    "texte": "4 marchés notifiés entre … et … au même attributaire, d'un montant cumulé de 187 400 € HT, chacun inférieur au seuil de …"
  },
  "parametres_effectifs": {"B2.fenetre_mois": 12, "seuils.ue_fournitures_services_collectivites": 221000},
  "preuves": ["ev_000123", "ev_000124", "ev_000125", "ev_000126"],
  "contexte_strate": {"strate": "C5", "n": 412, "percentile": 93},
  "combinaisons": ["C2"],
  "fiche": {"pourquoi": "…", "explications_ordinaires": ["…"], "verifier_ensuite": ["…"]}
}
```
Invariants vérifiés à la création : `preuves` non vide ; chaque valeur de `constat.valeurs` provient d'une preuve ou d'un calcul déclaré sur des preuves ; `texte` ne contient aucun mot interdit ; `fiche` est copiée du catalogue, pas rédigée.

### Rapport
```json
{
  "id": "aud_…",
  "protocol": "0.1",
  "vigie_version": "0.1.0",
  "playbook": {"id": "fr/audit-marches-publics-commune", "version": "0.1.0", "sha256": "…"},
  "catalogue_version": "0.1.0",
  "config_sha256": {"seuils.fr.yaml": "…", "regles.fr.yaml": "…", "strates.fr.yaml": "…"},
  "institution": {...},
  "periode": {"debut": "…", "fin": "…"},
  "date_audit": "…",
  "llm": {"fournisseur": "ollama", "modele": "…", "version": "…"} ,
  "sources": [{"id": "fr/decp", "statut": "ok | indisponible | vide | lacunaire", "documents": ["doc_…"]}],
  "qualite_donnees": {...},
  "signaux": [...],
  "textes": {
    "synthese": [{"texte": "…", "signaux": ["SIG-…"]}],
    "constats": {"SIG-…": {"texte": "…", "preuves": ["ev_…"]}}
  },
  "textes_fixes": {"avertissement": "…", "version": "1"},
  "manifeste": [{"id": "doc_…", "sha256": "…", "url_finale": "…", "date_heure": "…", "taille": 0}],
  "journal": ".vigie/audits/aud_…/journal.jsonl",
  "rapport_sha256": "…"
}
```

## 2. Règle d'ancrage des textes

Tout texte produit par un LLM dans un rapport est une liste de **phrases ancrées** : `{texte, signaux[] ou preuves[]}`. Le linter rejette :
- une phrase factuelle sans référence ;
- un nombre, une date, un montant, un nom d'organisation présent dans le texte et absent des `constat.valeurs` ou des `extrait` des preuves référencées (comparaison après normalisation) ;
- un mot de la liste interdite ;
- toute phrase dans les sections fixes (avertissement, fiches de règles) qui diffère du texte de référence.

Les phrases de liaison non factuelles (« Trois éléments ressortent. ») sont autorisées sans référence si elles ne contiennent ni nombre, ni nom, ni qualificatif.

## 3. Reproductibilité : `vigie verify`

Entrée : un rapport JSON (et, optionnellement, le store). Sortie : `OK` ou liste d'écarts. Étapes :

1. Vérifier l'empreinte du rapport.
2. Pour chaque document du manifeste : présent dans le store et empreinte conforme ; sinon, tenter de re-télécharger depuis `url_finale` et comparer (un écart ici n'invalide pas le rapport — la source a pu changer — mais est signalé : « source modifiée depuis l'audit »).
3. Pour chaque preuve : relocaliser l'extrait dans le document et comparer son empreinte.
4. Recharger la config aux empreintes indiquées (ou signaler qu'elle a changé), rejouer les règles sur les mêmes documents, comparer l'ensemble des signaux (identifiants, valeurs, sévérités). Un écart = `ÉCHEC` avec diff.
5. Vérifier l'ancrage de tous les textes (linter).
6. Vérifier que les textes fixes correspondent à leur version.

Un rapport qui ne passe pas `verify` ne peut pas être exporté en HTML partageable ; l'outil le dit.

## 4. Journalisation des appels de modèle

Chaque appel LLM est journalisé dans `journal.jsonl` :
```json
{"etape": 13, "fournisseur": "anthropic", "modele": "…", "prompt_sha256": "…", "prompt_gabarit": "redaction.v1", "temperature": 0, "schema_sortie": "Rapport.redaction", "sortie_sha256": "…", "duree_ms": 0, "tokens": {...}}
```
Le prompt complet n'est pas stocké dans le rapport (il contient les données) mais est reconstructible depuis le gabarit versionné + les entrées, et son empreinte est vérifiable. L'objectif n'est pas une reproductibilité bit à bit des sorties LLM (impossible), mais : (a) savoir exactement ce qui a été demandé, (b) garantir que le texte final est ancré, ce qui rend la variabilité du modèle sans conséquence sur les faits.

## 5. Ce que la chaîne de preuve permet de dire publiquement

Un rapport vérifié autorise cette formulation, et seulement celle-ci : *« Selon les données publiées par [source] le [date], [constat]. Les documents sont joints. Ce constat peut avoir une explication ordinaire ; voici ce qu'il faudrait vérifier. »* Toute formulation qui va au-delà engage son auteur, pas le projet, et le texte d'avertissement fixe le rappelle.

## 6. Export FollowTheMoney

Les entités (institution, attributaires, contrats, délibérations) et les signaux (comme `Note` ou schéma dédié rattaché aux entités) sont exportables en `.ftm.json` pour chargement dans OpenAleph. Les preuves y sont référencées par empreinte et URL ; les documents eux-mêmes peuvent être exportés dans un dossier compagnon.

## 7. Versionnage du protocole

`protocol: 0.1`. Toute modification des objets ci-dessus incrémente la version. Un rapport déclare sa version ; `verify` sait lire toutes les versions passées (pas de rupture de vérifiabilité des anciens rapports — c'est une promesse publique du projet).
