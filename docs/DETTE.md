# Dette — ce qui peut casser plus tard

Journal tenu par la compétence `cloture-de-session` : une entrée par session, la plus récente en
tête, au gabarit de `.claude/skills/cloture-de-session/references/gabarits.md`. Un point réglé est
barré sur place, avec ce qui l'a réglé. Les trous de la spécification n'y vont pas : ils deviennent
des ADR « proposés » dans `docs/decisions/`.

---

## 2026-11-09 — Révision de `critique-specification`, création de `hypotheses-structurantes`, première passe du registre (`.claude/skills/`, `docs/fondations/`, `docs/decisions/0004` à `0007`)

### 1. Le seuil « environ 3 500 habitants » du registre est une estimation, pas une mesure — *moyenne*

`docs/fondations/registre.md` (H3) et l'ADR 0005 proposent une tête de pont « communes ≥ 3 500
habitants » à partir d'un calcul à publication parfaite (`c1_estimation.py`), faute de réseau pour
exécuter `c1_detectabilite.py` sur le Parquet consolidé.

**Pourquoi ça casse.** Si l'ADR 0005 est accepté et que le seuil est recopié dans `docs/01`,
`config/strates.fr.yaml` ou la section « Limites » du playbook avant le test T1, le rapport déclarera
dans « Périmètre et limites » une limite non mesurée ; `verify` et `lint` ne voient pas un texte fixe
faux, et l'erreur n'apparaît que le jour où quelqu'un exécute T1 (sens probable de l'erreur : prudent,
mais non prouvé).

**Ce qu'il faut faire.** Exécuter T1 (une journée agent avec réseau, scripts en annexe de
`docs/fondations/2026-11-09.md`) avant d'écrire un chiffre de strate dans la spécification ou la
configuration ; l'auteur décide de l'ordre (T1 avant ou après la décision sur l'ADR 0005).

### 2. Les deux compétences de passe supposent un réglage d'effort et un réseau que l'environnement de la première passe n'avait pas — *basse*

`critique-specification` et `hypotheses-structurantes` demandent des sous-agents à `effort: "max"`
et, pour les calculs, des téléchargements depuis les portails de `docs/11`. Dans l'application Claude,
l'effort n'est pas réglable et le réseau du shell est coupé ; dans Claude Code, le paramètre `effort`
à l'appel exige la v2.1.292 au moins.

**Pourquoi ça casse.** Sur une version antérieure de Claude Code, l'étape 0 s'arrête (par
construction) ; dans un environnement sans réseau, le sous-agent « calculs » retombe sur des agrégats
publiés sans que la compétence l'ait prévu — la première passe l'a écrit dans son en-tête, la
compétence ne le dit pas. On s'en aperçoit à la première commande ; coût : un tour.

**Ce qu'il faut faire.** Ajouter à l'étape 0 des deux compétences la vérification du réseau
(`curl -sS -m 10 -o /dev/null -w '%{http_code}' https://www.data.gouv.fr/`) et la conduite à tenir
sans (agrégats publiés étiquetés « rapporté », scripts prêts en annexe, statut « à tester ») ; petite
PR de la prochaine session.

## 2026-11-08 — Contexte agentique importé du Banc d'essai 2027 (`.claude/`, `CLAUDE.md`, `HANDOFF.md`)

### 1. Les compétences citent des commandes et une arborescence qui n'existent pas encore — *basse*

`revue-de-code`, `conformite-specification`, les agents et le gabarit citent `uv run pytest`,
`uv run mypy --strict vigie/`, `uv run vigie lint`, `tests/golden/` et les modules de `docs/07` §3.
Rien de cela n'existe en phase 0.

**Pourquoi ça casse.** Si la structure réelle s'écarte de `docs/07` §3 (un module renommé, une
commande différente), une session ou un sous-agent suit une consigne fausse ; on le voit à la
première commande qui échoue, d'où la gravité basse.

**Ce qu'il faut faire.** À la fin de la phase 0, lancer `revue-contexte-agentique` : son axe 1 relève
mécaniquement les chemins et commandes absents.

### 2. Le partage des tâches entre `codeur-opus` et `codeur-sonnet` place les parseurs des connecteurs du côté d'Opus — *basse*

Le parsing qui produit des preuves est confié à `codeur-opus`, le téléchargement à `codeur-sonnet` :
un connecteur passe donc par deux agents, ou par Opus seul.

**Pourquoi ça casse.** Un brief qui confie un connecteur entier à Sonnet le fait s'arrêter à mi-chemin
(sa définition le lui demande). Coût : un aller-retour, visible.

**Ce qu'il faut faire.** Garder ce partage tant que les preuves déterministes des parseurs ne sont pas
couvertes par `verify` et des tests ; le revoir ensuite (décision de l'auteur).
