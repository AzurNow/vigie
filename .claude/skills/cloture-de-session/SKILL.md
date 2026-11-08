---
name: cloture-de-session
description: Clôture une session de travail sur Vigie en remplissant docs/DETTE.md (ce qui peut casser plus tard à cause de ce qui vient d'être fait), LESSONS.md (bonnes pratiques payées par l'expérience) et la section « État » de HANDOFF.md. À utiliser quand une session ou une tâche substantielle se termine, quand l'utilisateur dit qu'on s'arrête là, que c'est bon, qu'on clôture, ou demande le bilan de session — et avant de proposer un commit de fin de travail.
---

# Clôture de session

Deux fichiers, deux objets différents. `docs/DETTE.md` regarde en avant : ce qui va coûter.
`LESSONS.md` regarde en arrière : ce qu'on ne veut pas réapprendre. Ne pas écrire la même chose
dans les deux. `HANDOFF.md` dit où l'on en est, pour la session suivante.

Le travail utile ici est le **tri**, pas la rédaction. Une entrée de dette générique (« il faudrait
plus de tests ») est pire que rien : elle dilue les vraies. Si une session n'a produit aucun risque
réel, écrire qu'il n'y en a pas et passer à autre chose.

## Procédure

1. **Relire le diff de la session**, pas sa mémoire. `git diff`, `git status` et `git log` depuis
   le début de la session ; les fichiers ajoutés comptent autant que les modifiés.
2. **Extraire les risques réels.** Pour chaque décision prise, se demander : *qu'est-ce qui casse à
   cause d'elle, et comment le saurait-on ?* Un risque qui ne casse rien n'est pas un risque. Les
   trois questions de tri sont dans `references/tri.md`.
3. **Écrire l'entrée de `docs/DETTE.md`** au gabarit exact de `references/gabarits.md`, en tête du
   journal, sous l'en-tête du fichier. Une session = une entrée datée, même si elle ne contient
   qu'un point.
4. **Compléter `LESSONS.md`** — seulement si la session a *payé* quelque chose. Une session sans
   surprise n'ajoute aucune leçon, et c'est normal.
5. **Rayer ce qui est réglé.** Si la session a résolu un point de dette antérieur, barrer la ligne
   dans `docs/DETTE.md` et dire par quoi. Avant de barrer, `grep` la fonction ou le fichier que le
   point nomme et lire ce qu'il fait : une dette se règle dans le code, pas dans le récit.
6. **Réécrire la section « État » de `HANDOFF.md`** : phase de la roadmap en cours
   (`docs/08-roadmap.md`), livrables de la phase faits et restants, branches et PR ouvertes, agents
   en cours et leurs worktrees, prochaine action exacte. La section est réécrite, jamais empilée.
   Une question laissée à l'auteur devient le jour même un ADR au statut « proposé » dans
   `docs/decisions/` (voir `references/gabarits.md`) : une question qui ne vit que dans une
   conversation ou un corps de PR n'est plus lue par personne.
7. **Le dire en deux lignes** dans la réponse finale, sans recopier les fichiers.

## Ce qui va dans `docs/DETTE.md`, ce qui n'y va pas

| Va dans la dette | Ne va pas dans la dette |
| --- | --- |
| Une décision dont la facture arrivera plus tard | Un bug ouvert (il a sa place dans une issue) |
| Un invariant qu'aucun test ne garde | Une fonctionnalité pas encore écrite |
| Un couplage assumé mais coûteux | Une précaution générique sans cause identifiée |
| Une convention non écrite dont dépend la justesse d'un signal | Un trou de la **spécification** |

**Les trous de la spécification ne sont pas de la dette.** Une ambiguïté ou une contradiction des
documents normatifs (`docs/02` à `docs/07`, `docs/09`, `config/`) se signale à l'auteur et devient
un ADR « proposé » dans `docs/decisions/`, avec la lecture que le code a retenue en attendant.

## Gravité

- **haute** — casse silencieuse de ce qu'un rapport affirme. Un signal faux, une affirmation non
  ancrée, une preuve qui ne se relocalise plus, une donnée de personne indue, ou un rapport qui
  passe `vigie verify` et `vigie lint` alors qu'il ne le devrait pas — sans qu'aucun test n'échoue.
  C'est la seule catégorie qui justifie de retarder autre chose.
- **moyenne** — casse visible, coûteuse à réparer.
- **basse** — friction, surprise pour un nouvel arrivant.

Dans ce projet, la gravité se juge à une seule aune : *un rapport pourrait-il affirmer quelque chose
de faux, ou de non prouvé, sans que rien ne l'ait signalé ?* Une fausse accusation publique détruit
le capital de confiance du projet entier (`docs/01-vision-et-principes.md`, engagement 3) ; la
documentation éthique traite une erreur de règle comme un bug de sécurité (`docs/09`, §7).

## Critère d'entrée dans `LESSONS.md`

Une leçon n'entre que si elle a été **payée** : une décision tranchée après hésitation, un piège
évité de justesse, une erreur commise. Pas de vérités générales sur le génie logiciel. Le test :
quelqu'un qui n'a pas vécu la session doit pouvoir dire ce qui a failli mal tourner.

Ranger dans les sections existantes du fichier plutôt que d'en créer une nouvelle. Une leçon
formulée à l'impératif et suivie du cas concret qui l'a produite vaut mieux qu'un paragraphe.

## Gabarits et tri

- `references/gabarits.md` — la forme exacte d'une entrée de dette, d'une leçon et d'un ADR
  proposé. À lire avant d'écrire.
- `references/tri.md` — les trois questions qui séparent un vrai risque d'une précaution.
