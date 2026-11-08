---
name: revue-de-code
description: Passe complète de robustesse et de propreté sur le code de Vigie, reproductible d'une fois sur l'autre. À utiliser quand l'utilisateur demande une revue du code, une passe de qualité, de maintenabilité ou de lisibilité, un audit de propreté, ou « refaire la revue » — pas pour relire un diff isolé (la commande intégrée de revue de diff suffit). Produit un rapport daté dans docs/revues/, trié par gravité, qui repart du rapport précédent ; ne corrige rien sans accord.
---

# Revue de code

Le but est un code qu'un relecteur tiers lit d'une traite et qu'on maintient des années : simple,
sûr aux frontières, sans surprise. Le cœur doit rester « lisible en une journée » (`docs/07` §1).
**Lisible veut dire simple, pas commenté.** Un commentaire qui paraphrase le code est un défaut ;
un commentaire qui dit *pourquoi* ce n'est pas fait autrement est une qualité.

La revue est reproductible parce que trois choses sont fixées : la grille (`references/grille.md`),
le tri (`references/tri.md`) et la forme du rapport (`references/rapport.md`). Deux relecteurs, ou
un agent dans six mois, doivent trouver la même chose.

## Procédure

1. **Partir d'un arbre propre**, sur une branche `docs/revue-AAAA-MM-JJ`. `uv run ruff check`,
   `uv run mypy --strict vigie/` et `uv run pytest` doivent déjà passer ; sinon, le noter en tête du
   rapport comme constat.
2. **Relire le rapport précédent** (`docs/revues/`, le plus récent). Ce qui y est marqué *accepté*
   n'est pas re-signalé, sauf si le code a changé depuis ; ce qui est *à corriger* et ne l'a pas été
   est repris tel quel, avec sa date d'origine. S'il n'y en a pas, c'est une première revue.
3. **Relever les signaux** avec les commandes de `references/grille.md` (« Signaux mécaniques »).
   Copier le tableau des comptes dans le rapport. Les signaux ne sont pas des constats : ils disent
   où lire.
4. **Lire tout le code de production** (`vigie/`, `scripts/`), module par module, avec la grille.
   Les signaux orientent la lecture, ils ne la remplacent pas : les défauts qui comptent (une règle
   qui fait de l'I/O, une preuve dont l'extrait est retouché, une frontière non validée) n'ont pas de
   signal.
5. **Lire les tests plus vite** : l'axe 4 de la grille seulement, plus les signaux sur `tests/`.
6. **Trier chaque constat** avec `references/tri.md` : gravité, et « corriger » ou « accepter ».
7. **Écrire le rapport** `docs/revues/AAAA-MM-JJ.md` au gabarit de `references/rapport.md`, et le
   soumettre. **Ne rien corriger pendant la passe** : l'auteur choisit ce qu'on corrige.
8. **Corriger par petites PR**, une par axe ou par module, après accord. Chaque PR garde ruff, mypy
   et pytest verts, laisse les rapports golden (`tests/golden/`) identiques et ne change aucun
   comportement observable. Mettre à jour le statut des constats dans le rapport (`corrigé dans #NN`).

## Ce qu'une correction de revue ne fait jamais

- Changer une règle, un seuil, une fiche du catalogue, un format du protocole (`docs/04`, `05`,
  `06`), la liste du vocabulaire interdit ou un texte fixe : ce sont des décisions de l'auteur
  (`CLAUDE.md`, « Où l'agent s'arrête »). Le constat est rapporté, la décision proposée.
- Rendre un test vert en affaiblissant son assertion, régénérer un rapport golden pour faire passer
  un écart, ou relever un seuil de lint.
- Mélanger deux axes dans une PR. Une PR de revue se relit en dix minutes ou elle est trop grosse.
- Découper une fonction sans la compétence `complexite-maitrisee`.

## Après la passe

Si la revue a *payé* quelque chose (un défaut réel trouvé par un chemin inattendu, une question de
grille qui manquait), l'ajouter à `LESSONS.md` et, si c'est une question de lecture, à
`references/grille.md` : la grille s'enrichit de ce que chaque revue a appris. Un signal qu'une
commande aurait pu relever mécaniquement mérite d'entrer dans « Signaux mécaniques ».
