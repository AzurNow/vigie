---
name: conformite-specification
description: Passe de conformité entre le code de Vigie et sa spécification normative (les sept règles de CLAUDE.md, docs/02 à docs/07, docs/09, config/), dans les deux sens — chaque exigence est-elle implémentée et testée, et chaque règle du code qui change un signal, une preuve ou un rapport est-elle écrite dans la spécification ? À utiliser quand l'utilisateur demande de vérifier la cohérence du code avec la spécification ou le catalogue, un audit de conformité, une traçabilité spécification ↔ code, « refaire la conformité », après un lot qui touche une règle, la chaîne de preuve, le linter ou verify, avant de déclarer une phase de la roadmap terminée et avant tout changement de PROTOCOL_VERSION. Pas pour la qualité du code (revue-de-code) ni pour juger la spécification (critique-specification). Produit un rapport daté dans docs/conformite/ et tient à jour la matrice docs/conformite/exigences.json ; ne corrige rien sans accord.
---

# Conformité à la spécification

La spécification fait autorité sur le code : les sept règles de `CLAUDE.md` priment sur tout,
`docs/02` prime sur `docs/07` (il le dit), et `docs/07` cède devant les formats du protocole
(`docs/04`, `05`, `06`), puisqu'il se dit remplaçable par toute implémentation qui les respecte.
Mais rien ne garantit mécaniquement que le code dit la même chose qu'eux : les écarts s'accumulent
en silence, dans les deux sens. Le code peut **contredire** une exigence, la **laisser sans test**,
ou fixer une règle qui change un signal sans que la spécification **l'écrive** — un départage, un
seuil de relocalisation, une façon de calculer la confiance. Ce dernier sens compte double ici :
`vigie verify` doit pouvoir rejouer un rapport chez n'importe qui, et une règle que seul le code
connaît n'est pas rejouable par une autre implémentation du protocole.

La passe est reproductible parce que trois choses sont fixées : la façon d'extraire et de trier
(`references/extraction.md`), la matrice des exigences (`references/matrice.md`, fichier
`docs/conformite/exigences.json`), et la forme du rapport (`references/rapport.md`). Deux
relecteurs, ou un agent dans six mois, doivent aboutir à la même matrice.

Ce que cette passe ne fait pas : juger la spécification (`critique-specification`), juger la
qualité du code (`revue-de-code`), corriger quoi que ce soit.

## Périmètre

Normatif, donc source d'exigences : les sept règles de `CLAUDE.md` (et « Ce que tu ne fais
jamais »), `docs/02-specification-metier.md`, `docs/03-catalogue-red-flags-marches-publics.md`,
`docs/04-format-playbook.md`, `docs/05-format-connecteur.md`, `docs/06-chaine-de-preuve.md`,
`docs/07-specification-technique.md`, `docs/09-ethique-et-juridique.md`, les ADR acceptés de
`docs/decisions/`, et les en-têtes des fichiers de `config/` (les valeurs n'y sont pas des
exigences : c'est le code qui doit les lire).

Non normatif : `docs/01` (vision), `docs/08` (roadmap, sert à nommer le lot d'une exigence
`a-implementer`), `docs/10` (gouvernance), `docs/references.md`. `docs/11` se lit quand on trace un
connecteur, sans produire d'exigence sauf s'il fixe un comportement.

## Procédure

1. **Partir d'un arbre propre**, sur une branche `docs/conformite-AAAA-MM-JJ`. Relever le commit de
   départ, `PROTOCOL_VERSION` (`vigie/protocol/__init__.py`), la version du catalogue et les
   empreintes des fichiers normatifs :
   `shasum -a 256 CLAUDE.md docs/0[2-7]-*.md docs/09-*.md config/*.yaml`. `uv run ruff check`,
   `uv run mypy --strict vigie/` et `uv run pytest` doivent passer ; sinon, le noter en tête du
   rapport.
2. **Relire la passe précédente** : le rapport le plus récent de `docs/conformite/` et la matrice.
   S'il n'y en a pas, c'est une **première passe** : aller à l'étape 3 sur tout le périmètre.
   Sinon, `git diff <commit de la passe précédente> -- CLAUDE.md docs/ config/` donne les sections à
   réextraire, et `git diff --stat <commit> -- vigie/ scripts/ catalogue/` le code à relire en
   détail.
3. **Extraire les exigences** des sections concernées (toutes à la première passe), selon
   `references/extraction.md`. Chaque exigence reçoit un identifiant stable et une citation exacte.
   Une exigence déjà dans la matrice garde son identifiant ; une exigence dont le texte a changé est
   mise à jour et repassée à `a-verifier`.
4. **Tracer chaque exigence** vers le code qui l'applique et le test qui la garde, par `grep` et
   lecture — jamais de mémoire ni d'après une docstring seule. Une docstring qui cite « doc 06 §2 »
   n'est pas une preuve : lire le code. Attribuer le statut (`references/matrice.md`).
5. **Revérifier le reste de la matrice** : pour chaque exigence non réextraite, confirmer que les
   fichiers et tests cités existent encore et disent toujours la même chose. Un test renommé ou un
   fichier déplacé se met à jour ; un test disparu fait retomber l'exigence en `sans-test`.
6. **Lire dans l'autre sens** : parcourir le code qui collecte, normalise, résout, applique les
   règles, agrège, ancre, vérifie ou rend (`vigie/connectors/`, `vigie/normalize/`, `vigie/rules/`,
   `vigie/aggregate/`, `vigie/llm/`, `vigie/lint/`, `vigie/verify/`, `vigie/report/`,
   `vigie/playbook/`, `vigie/protocol/`, `scripts/`, `catalogue/`) et relever chaque règle qui change
   un signal, une preuve ou un rapport sans être écrite dans la spécification
   (`references/extraction.md`, « Sens inverse »).
7. **Trier** chaque constat (gravité, et proposition : corriger le code, réviser la spécification,
   ou accepter) selon `references/extraction.md`.
8. **Écrire** `docs/conformite/AAAA-MM-JJ.md` au gabarit de `references/rapport.md`, mettre à jour
   `docs/conformite/exigences.json` (versions et empreintes, commit, date), valider la matrice
   (`python3 -m json.tool docs/conformite/exigences.json > /dev/null` au minimum, et les contrôles
   de `references/matrice.md`) et soumettre. **Ne rien corriger pendant la passe.**
9. **Après accord**, corriger par petites PR. Une contradiction se règle de deux façons, et le choix
   appartient **toujours** à l'auteur : corriger le code, ou réviser la spécification (ADR, et pour
   un format du protocole, nouvelle `PROTOCOL_VERSION` selon `docs/06` §7). Mettre à jour le statut
   des constats et de la matrice dans la PR qui corrige.

## Règles de la passe

- **Citer, ne pas paraphraser.** Chaque exigence porte la phrase exacte qui la fonde. Une exigence
  reformulée finit par dire autre chose que le texte.
- **Une preuve est un fichier et une fonction.** « Implémentée » exige un emplacement de code ;
  « testée » exige un test nommé qui échouerait si l'exigence était violée. Un test qui passe par
  hasard ne compte pas.
- **Absence n'est pas contradiction.** Une exigence d'une phase pas encore faite est
  `a-implementer` avec le lot de `docs/08` nommé, pas un constat.
- **Ne jamais trancher un trou de la spécification.** Si elle est ambiguë ou muette sur un point
  que le code a dû fixer, c'est un constat « règle non écrite » ou « spécification ambiguë », avec la
  lecture que le code a retenue. La décision revient à l'auteur.
- **Un [À VÉRIFIER] n'est pas levé par la passe.** Le relever, dire si du code en dépend ; seule la
  source officielle, consultée et citée, le lève, et l'auteur valide.
- **Aucune écriture dans la spécification, le catalogue, `config/`, les playbooks ou les
  connecteurs** pendant la passe. Les propositions de texte vont dans le rapport.

## Quand la lancer

- Après un lot qui touche une règle, un connecteur qui produit des preuves, la chaîne de preuve,
  le linter, `verify` ou le runner de playbooks.
- Avant tout changement de `PROTOCOL_VERSION` ou de version du catalogue : elle dit ce que le
  changement doit aussi couvrir.
- **Avant de déclarer une phase de `docs/08` terminée**, et obligatoirement avant le lancement de
  la phase 1 : aucune exigence à `contredite`, et chaque « règle non écrite » tranchée (écrite dans
  la spécification ou acceptée par écrit).

## Après la passe

Si la passe a *payé* quelque chose (une contradiction trouvée par un chemin inattendu, une catégorie
d'exigence que l'extraction ratait), l'ajouter à `LESSONS.md` et, si c'est une règle d'extraction, à
`references/extraction.md`.
