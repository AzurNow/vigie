# Dette — ce qui peut casser plus tard

Journal tenu par la compétence `cloture-de-session` : une entrée par session, la plus récente en
tête, au gabarit de `.claude/skills/cloture-de-session/references/gabarits.md`. Un point réglé est
barré sur place, avec ce qui l'a réglé. Les trous de la spécification n'y vont pas : ils deviennent
des ADR « proposés » dans `docs/decisions/`.

---

## 2026-11-13 — Test T1 et pilote A exécutés par l'agent, ADR 0008 proposé (`docs/fondations/`, `docs/decisions/0008`)

### 1. Les PR #6 et #7 ont pour base la branche d'une autre PR — *moyenne*

#6 a pour base `docs/fondations-2026-11-09` (#4) et #7 a pour base `docs/t1-detectabilite` (#6),
parce que chaque travail s'appuyait sur le registre de la précédente. `LESSONS.md` (« Ne pas fusionner
une PR dont la base est la branche d'une autre PR ») décrit déjà ce piège.

**Pourquoi ça casse.** Si #6 est fusionnée avant #4, elle atterrit dans la branche de #4 et pas dans
`main` ; si #4 est fusionnée sans supprimer sa branche, GitHub ne rebase pas #6 sur `main`. On le voit
à la lecture de `main`, après coup.

**Ce qu'il faut faire.** Fusionner #4, supprimer sa branche (GitHub rebase alors #6 sur `main`),
vérifier que la base de #6 affiche `main`, fusionner, puis de même pour #7. Ou, au choix de l'auteur,
fermer #6 et faire de #7 une seule PR sur `main` depuis le sommet.

### ~~2. Les deux premiers commits de la PR #7 rendent identifiables les deux communes du pilote A~~ — réglé le 2026-11-13 par la réécriture de la branche en un seul commit propre (poussée en force sur décision de l'auteur) ; les anciens commits ne restent visibles que par le lien « force-pushed » de la PR, hors de `main`

Le compte rendu du pilote citait des montants exacts au centime, les populations exactes, le nom d'un
profil d'acheteur départemental et la graine du tirage. Un seul de ces éléments suffit à retrouver la
commune dans le consolidé public. Le troisième commit les retire du fichier, et le corps de la PR a été
réécrit, mais le dépôt est public et l'historique de la branche les garde.

**Pourquoi ça casse.** Une fois la PR fusionnée, n'importe qui peut lire dans l'historique du projet
des signaux attachés à une commune réelle et identifiable ; c'est ce que `docs/10` §3 exclut (« on ne
discute pas d'institutions nommées dans les espaces du projet »). Aucun contrôle ne le voit : ni
`lint`, qui ne lit pas `docs/fondations/`, ni la relecture, qui lit le diff final.

**Ce qu'il faut faire.** Décision de l'auteur, sur une action difficile à défaire : réécrire la
branche en un seul commit propre et la pousser en force avant fusion (recommandé), ou fermer #7 et
ouvrir une PR neuve depuis une branche propre ; dans les deux cas, les anciens commits restent
visibles un temps par le lien de la PR. Ensuite, inscrire la contrainte dans la procédure du pilote
du registre (« montants en tranches, dates à l'année, aucune graine »).

### 3. Le seuil de dispense travaux de 100 000 € est relevé sans ses bornes antérieures à 2026 — *haute*

Le pilote et l'ADR 0008 rapportent ce seuil « pérennisé au 2026-01-01 ; auparavant temporaire et
plusieurs fois prorogé (bornes antérieures non relevées) », d'après une source secondaire
(marche-public.fr) ; le script du pilote l'a appliqué depuis 2020 sans distinction.

**Pourquoi ça casse.** Si ce seuil entre dans `config/seuils.fr.yaml` avec une date de début mal
choisie, B1 cherche la bande 95 000–100 000 € sur des marchés de travaux notifiés pendant une période
où le seuil applicable était de 40 000 €, et inversement ignore la bande 38 000–40 000 € : des signaux
manqués ou fabriqués, à confiance haute, que ni `verify` (qui rejoue à l'identique) ni un test (écrit
sur la même valeur) ne verront. Les résultats du pilote n'en dépendent pas : aucun marché de travaux
de 2024-2025 des deux communes n'est dans l'une ou l'autre bande.

**Ce qu'il faut faire.** L'auteur relève sur la source officielle (Légifrance ou DAJ) les décrets
successifs et leurs dates avant toute écriture dans `config/` ; la PR des seuils de l'ADR 0008 cite
chaque borne.

## 2026-11-09 — Révision de `critique-specification`, création de `hypotheses-structurantes`, première passe du registre (`.claude/skills/`, `docs/fondations/`, `docs/decisions/0004` à `0007`)

### ~~1. Le seuil « environ 3 500 habitants » du registre est une estimation, pas une mesure~~ — réglé le 2026-11-13 par le test T1 (PR #6, `docs/fondations/2026-11-13-t1.md`) : mesuré, la bascule est vers 4 000 à 5 000 habitants

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
