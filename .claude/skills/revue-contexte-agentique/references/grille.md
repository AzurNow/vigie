# Grille de la revue du contexte agentique

Sept axes. Chaque question appelle une réponse vérifiable ; un « non » n'est un constat que s'il
coûte quelque chose (étape 4 de la compétence). La veille du jour peut ajouter ou contredire une
question : c'est elle qui fait foi sur les pratiques, cette grille fixe seulement ce qu'on regarde.

## 1. Exactitude

- Chaque chemin, commande, variable d'environnement, compétence et agent cité existe. Mécanique, pas
  de mémoire :

  ```bash
  # chemins cités entre accents graves dans le contexte versionné, absents du dépôt
  grep -ohE '`[A-Za-z0-9_.-]+/[A-Za-z0-9_./<>*-]*`' CLAUDE.md HANDOFF.md .claude/skills/*/SKILL.md \
    .claude/skills/*/references/*.md .claude/agents/*.md .claude/briefs/GABARIT.md | tr -d '`' | sort -u \
    | while read -r p; do case "$p" in *'<'*|*'*'*|*AAAA*|*NNNN*) ;; *) [ -e "$p" ] || echo "absent : $p";; esac; done
  # sous-commandes vigie citées, absentes de la CLI (quand elle existe)
  grep -rohE 'vigie [a-z]+( [a-z]+)?' CLAUDE.md HANDOFF.md .claude/skills .claude/agents .claude/briefs/GABARIT.md \
    | sort -u
  uv run vigie --help
  # compétences et agents cités, absents
  grep -rohE '`[a-z]+(-[a-z]+)+`' CLAUDE.md HANDOFF.md .claude | tr -d '`' | sort -u \
    | while read -r n; do [ -e ".claude/skills/$n" ] || [ -e ".claude/agents/$n.md" ] || echo "non trouvé : $n"; done
  ```

  Jamais `.claude` en entier dans une recherche de fichiers : `.claude/worktrees/` contient des copies
  complètes du dépôt. Un chemin absent n'est pas forcément faux : l'arborescence de `docs/07` §3 naît
  au fil des phases, et la troisième commande sort aussi des noms qui ne sont ni des compétences ni
  des agents (`type-de-commit`, noms de fichiers) — lire la phrase.
- Les faits datés (« phase 0 », « pas encore écrit », versions d'outils) sont encore vrais : les
  confronter à la section « État » de `HANDOFF.md` et au dépôt.
- Les noms et alias de modèles des en-têtes (`model:`) et des consignes sont valides à date.

## 2. Économie du contexte toujours chargé

Mesurer avant et après, en caractères (une estimation en tokens se donne comme telle) :

```bash
wc -c CLAUDE.md                                         # + tout fichier importé par @chemin
grep -n '^@\|[[:space:]]@[A-Za-z.]' CLAUDE.md            # imports éventuels
wc -c ~/.claude/CLAUDE.md 2>/dev/null                   # consignes personnelles de l'auteur (local)
wc -c ~/.claude/projects/*vigie*/memory/MEMORY.md 2>/dev/null   # index de mémoire (local)
for f in .claude/skills/*/SKILL.md .claude/agents/*.md; do
  printf '%6d %s\n' "$(sed -n 's/^description: //p' "$f" | wc -c)" "$f"; done
```

- Ce qui ne sert qu'à certaines tâches sort du `CLAUDE.md` vers une compétence ou une référence
  chargée à la demande. L'« ordre de lecture obligatoire » de `CLAUDE.md` renvoie vers `docs/` sans
  les charger : le garder ainsi.
- Rien n'est dit deux fois entre `CLAUDE.md`, `HANDOFF.md`, compétences, agents, mémoire et
  `~/.claude/CLAUDE.md`.
- Rien n'énonce ce que le modèle sait déjà ou ce que le code, `pyproject.toml` ou la documentation
  de `docs/` disent déjà.
- Les descriptions de compétences et d'agents sont chargées à chaque session : chaque phrase y sert
  au déclenchement ou au routage, le reste va dans le corps.

## 3. Efficacité des consignes

- Chaque consigne est actionnable et vérifiable ; une consigne non évidente porte son pourquoi.
- Aucune contradiction entre deux sources, et l'ordre de priorité est dit une fois (`CLAUDE.md` :
  les sept règles priment ; `docs/02` prime sur `docs/07`).
- Le ton suit la veille du jour (par exemple sur l'emphase, les majuscules, les interdits sans
  alternative) ; une règle critique se distingue par sa place et sa raison, pas par sa typographie.
- Les exemples, s'il y en a, sont justes et représentatifs ; un exemple fictif est dit fictif.

## 4. Déclenchement et routage

- Chaque description de compétence dit quoi, quand, et quand pas ; deux compétences ne se disputent
  pas la même demande (`revue-de-code`, `conformite-specification`, `critique-specification`,
  `revue-contexte-agentique` ont des frontières écrites).
- Une compétence coûteuse ou à effet de bord est réservée au lancement manuel
  (`disable-model-invocation`).
- Chaque agent dit ce qu'on lui confie et ce qu'on ne lui confie pas ; le gabarit de brief produit un
  brief autonome (but, fichiers, ce qui est exclu, critère de fin).
- Un sous-agent reçoit sa définition, `CLAUDE.md` et l'état Git, pas la conversation ni la mémoire du
  parent (à revérifier dans la documentation des sous-agents à chaque veille) : sa définition ne
  répète pas `CLAUDE.md`, elle porte ce qui lui est propre.
- Les affectations de modèle et d'effort suivent la nature de la tâche et la veille du jour.

## 5. Garde-fous déterministes

- Une règle que la prose seule protège mal (écriture dans la spécification, le catalogue,
  `config/`, les textes fixes ; données nominatives dans `tests/fixtures/`) a-t-elle, ou
  mériterait-elle, un garde mécanique (permission partagée, crochet, contrôle de CI déjà prévu par
  `docs/07` §12) ? Proposer seulement : tout nouveau mécanisme revient à l'auteur.
- Les permissions utiles à tous sont dans `.claude/settings.json` versionné ; `settings.local.json`
  ne garde que le propre à une machine, et Git l'ignore.

## 6. Dépôt public et autres agents

- Un contributeur qui clone, sans la machine ni la mémoire de l'auteur, trouve tout ce qu'il faut :
  les consignes durables de la mémoire qui valent pour le projet sont recopiées dans `HANDOFF.md`.
- Rien de personnel ni d'absolu à une machine dans le versionné (hors archives signalées comme
  telles).
- Un contributeur qui utilise un autre agent que Claude Code trouve le point d'entrée (selon la
  veille : `AGENTS.md` ou équivalent), sans doublon à maintenir. C'est une exigence de la règle 6,
  pas un confort.
- Langue, terminologie et ton cohérents d'un fichier à l'autre, et avec le glossaire de `docs/02` §1.

## 7. Mémoire locale

- Chaque entrée de l'index pointe vers un fichier existant ; chaque fichier est encore vrai.
- Pas de doublon avec le dépôt ni avec `~/.claude/CLAUDE.md` ; ce qui vaut pour tout contributeur est
  dans le dépôt, pas seulement en mémoire.
- Les entrées périmées sont corrigées ou supprimées (la mémoire est locale : hors PR, à noter au
  rapport).
