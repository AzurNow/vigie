# ADR 0003 — Donner un point d'entrée aux agents de code autres que Claude Code

Date : 2026-11-08 · Statut : proposé

## Contexte
La règle 6 de `CLAUDE.md` veut un projet « agnostique au modèle » : les playbooks sont exécutables
par n'importe quel agent, et un contributeur doit pouvoir développer Vigie avec l'agent de son choix.
Or les instructions de développement vivent dans `CLAUDE.md`, que lit Claude Code ; d'autres agents
de code lisent un fichier `AGENTS.md` à la racine. La documentation de Claude Code
(https://code.claude.com/docs/en/memory, consultée le 2026-11-08) indique que Claude Code lit
`CLAUDE.md` et qu'un `CLAUDE.md` peut importer un autre fichier par la syntaxe `@chemin`, chemin
relatif au fichier qui importe ; elle recommande, pour un dépôt qui utilise `AGENTS.md`, un
`CLAUDE.md` qui l'importe, suivi des consignes propres à Claude Code. Aujourd'hui, un contributeur
avec un autre agent ne trouve rien.

## Options
1. **`AGENTS.md` porte les consignes communes, `CLAUDE.md` l'importe** (recommandée) — déplacer dans
   `AGENTS.md` tout ce qui vaut pour n'importe quel agent (ce qu'est le projet, ordre de lecture, les
   sept règles, stack, workflow, « Où l'agent s'arrête », « Ce que tu ne fais jamais », commandes) ;
   `CLAUDE.md` ne garde que `@AGENTS.md` et ce qui est propre à Claude Code (compétences, sous-agents,
   `HANDOFF.md`). Une seule source, aucun doublon ; coût : un déplacement de texte, et vérifier que
   la taille chargée par Claude Code ne change pas.
2. **Lien symbolique `CLAUDE.md` → `AGENTS.md`** — un seul fichier ; mais plus de place pour les
   consignes propres à Claude Code, et les liens symboliques se comportent mal sous Windows, visé
   par le projet (`docs/02` §9).
3. **Ne rien faire** — aucun coût, mais la règle 6 n'est pas tenue pour les contributeurs.

## Recommandation
Option 1 : elle tient la règle 6 sans dupliquer une seule consigne, et la documentation de Claude
Code la décrit pour ce cas précis. Les sept règles sont déplacées mot pour mot, sans en changer une
lettre (`docs/10` §2).

## Conséquences
`AGENTS.md` devient le point d'entrée cité par `README.md` (« Pour développer avec un agent de
code »). La compétence `revue-contexte-agentique` mesure alors `CLAUDE.md` plus `AGENTS.md`.
