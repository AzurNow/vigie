---
name: complexite-maitrisee
description: Découper une fonction trop complexe dans Vigie, sans violer les règles du dépôt. À utiliser quand une fonction approche ou dépasse une complexité cyclomatique de 10, quand ruff signale C901, quand une règle, un parseur de connecteur, le linter, verify ou le runner de playbooks accumule les branches, ou avant d'écrire une fonction dont on pressent qu'elle branchera beaucoup. Contient la façon de mesurer, les quatre découpes admises et les trois pièges propres à ce projet.
---

# Complexité maîtrisée

La règle est dans `CLAUDE.md` : toute fonction reste strictement sous 10 en complexité cyclomatique
(McCabe), soit 9 au plus, et sous 15 en complexité cognitive. Ce document ne répète pas la règle, il
dit **comment s'y conformer ici** — parce que la découpe naïve entre en collision avec trois
principes déjà posés dans ce dépôt.

## Mesurer avant de découper

Ne pas estimer à l'œil. La mesure qui fait foi est celle de `C901` de Ruff (configuration dans
`references/outillage.md`) : `+1` de base, `+1` par `if`, `elif`, `for`, `while`, `except`, et
`n − 1` pour un `match` à `n` branches. Ruff ne compte **ni** `and`/`or`, **ni** l'expression
conditionnelle, **ni** les compréhensions (vérifié avec ruff 0.16.8) : ces formes-là, c'est la
complexité cognitive qui les rattrape. Tant que `pyproject.toml` ne porte pas la règle, mesurer avec
`uvx ruff check --isolated --select C901` ou à la main avant de proposer une fonction.

## Les quatre découpes admises, dans cet ordre de préférence

1. **Déplacer les cas dans les données.** C'est la première à essayer dans ce projet, pas la
   dernière. Les seuils datés vivent dans `config/seuils.fr.yaml`, les paramètres de règle dans
   `config/regles.fr.yaml`, les strates dans `config/strates.fr.yaml`, les particularités d'une
   source dans le YAML de son connecteur. Une suite de `if procedure == …` ou `if date >= …` qui
   choisit un seuil est une lecture de table qui s'ignore. Un `if` sur une commune, un acheteur ou
   une juridiction est un cas particulier codé en dur : il appartient à une fiche ou à `config/`.
2. **Extraire des prédicats nommés.** `est_sous_seuil_dans_bande(montant, seuil, bande)`,
   `est_montant_maximum(contrat)`. Une condition composée de trois `and` devient un nom qui dit la
   règle du catalogue, et le test de cette règle devient possible isolément.
3. **Séparer les phases.** Valider, puis calculer, puis assembler le signal. Une fonction qui fait
   les trois branche pour les trois. Les frontières de phase de `docs/07` §1 (fetch → parse →
   normalize → rules → aggregate) sont les frontières naturelles de découpe.
4. **Remonter la garde à l'appelant.** Une fonction qui commence par cinq `if` de rejet fait en
   réalité deux métiers : filtrer et calculer. Le filtre remonte (par exemple dans `requires` de
   l'interface `Rule`, `docs/07` §5), le calcul reste pur.

## Trois pièges propres à ce dépôt

**Ne pas éparpiller une règle en la découpant.** Un signal = une règle = un fichier
(`vigie/rules/<famille>/<code>.py`). Découper une règle en quatre fonctions privées du même fichier
reste une seule définition — c'est licite. Le devient illicite dès qu'un morceau propre à la règle
part dans un autre module, ou qu'une fonction extraite recalcule une valeur que `vigie/aggregate/`
calcule déjà (percentile de strate, HHI). La règle réelle : **une définition, un endroit** ; un
calcul partagé par plusieurs règles vit une fois dans `vigie/aggregate/`, le reste dans le fichier
de la règle.

**Ne pas transformer une garde en valeur par défaut.** En remontant des gardes, la tentation est de
terminer la fonction extraite par un `or 0`, un `.get(clé, 0)` ou un `return "inconnu"`. Une donnée
absente est une information (`docs/02` §6) : `nombre_offres_recues` absent fait baisser la
confiance de A1 (catalogue), il ne vaut pas zéro. La fonction extraite renvoie l'absence (`None`,
typé), elle ne la comble pas.

**Ne pas perdre le caractère bloquant d'un contrôle en le découpant.** Les invariants du signal
(`preuves` non vide, valeurs issues des preuves, vocabulaire, fiche copiée : `docs/06` §1),
l'ancrage des textes (`docs/06` §2) et les étapes de `verify` (`docs/06` §3) sont des refus, pas des
scores. Après découpe, il doit rester impossible qu'un sous-résultat en échec se perde dans un
agrégat permissif. Vérifier que le refus remonte jusqu'au code de sortie (2 validation, 3
vérification, `docs/07` §11).

## Ce qui n'est jamais une réponse

- Relever le seuil, localement ou globalement.
- `# noqa: C901` ou toute désactivation équivalente.
- Remplacer dix `if` par une chaîne d'expressions conditionnelles ou un dictionnaire de lambdas
  illisible : la complexité mesurée baisse, la complexité réelle monte. Si la découpe ne rend pas
  le code plus clair, c'est que la bonne découpe n'a pas encore été trouvée.
- Fusionner des branches en trichant sur les cas limites. Les faux positifs connus du catalogue
  sont le sujet de ce projet, pas le détail.

## Quand la découpe résiste

Une fonction qui refuse de descendre sous 10 sans devenir illisible signale presque toujours une
règle mal modélisée — un cas particulier qui devrait être une donnée, ou une responsabilité qui
appartient à une autre étape du pipeline. Le dire plutôt que de contourner, comme pour un trou de la
spécification.

`references/outillage.md` — configuration de Ruff prête à coller, complexité cognitive, et comment
mesurer à la main en attendant.
