# Gabarit du rapport — `docs/conformite/AAAA-MM-JJ.md`

Un fichier par passe, jamais réécrit une fois la passe close, sauf la colonne *Statut* que les PR
de correction mettent à jour. La passe suivante le lit en premier.

```markdown
# Conformité à la spécification du AAAA-MM-JJ

Commit de départ : `<sha court>`. Protocole : <PROTOCOL_VERSION>. Catalogue : <version>.
Empreintes : voir `exigences.json`.
Passe précédente : [AAAA-MM-JJ](AAAA-MM-JJ.md) (ou « aucune, première passe »).
Périmètre : <tout | sections réextraites : D06§2, B2 ; code relu en détail : …>.

## Bilan de la matrice

| Statut | Nombre | Depuis la passe précédente |
| --- | ---: | ---: |
| testee | | |
| sans-test | | |
| contredite | | |
| partielle | | |
| a-implementer | | |
| hors-code | | |
| retiree | | |

<Une ou deux phrases : ce qui a bougé et pourquoi.>

## Constats

| # | Gravité | Sens | Exigence | Où | Constat | Proposition | Statut |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | haute | spécification → code | B2.03 | `chemin:fonction` | <une phrase affirmative> | corriger le code | ouvert |
| 2 | haute | code → spécification | — | `chemin:fonction` | <la règle, et le signal qu'elle change> | réviser la spécification | ouvert |
| 3 | basse | spécification → code | D07§11.02 | … | … | accepter : <pourquoi> | — |

## Détail des constats hauts et moyens

### 1. <le constat, en une phrase affirmative>

**La spécification dit** : « <citation exacte> » (<fichier>, §x ou code de règle).
**Le code fait** : <ce qui se passe, fichier et fonction>.
**Ce que ça change** : <le signal, la preuve ou la section du rapport qui en dépend, et le scénario>.
**Proposition** : <corriger le code : comment ; ou réviser la spécification : texte proposé, entre
guillemets, prêt à être relu par l'auteur, et effet sur `PROTOCOL_VERSION` s'il y a lieu>.

## Exigences sans test

<Liste des `sans-test` touchant un signal ou une preuve, avec le test qui manquerait (son nom) — ou
« aucune ».>

## [À VÉRIFIER] dont du code dépend

<Chaque valeur ou affirmation marquée [À VÉRIFIER] que du code implémenté lit, avec le fichier qui la
lit — ou « aucune ».>

## Repris de la passe précédente

<Constats encore ouverts, avec leur date d'origine ; ou « aucun ».>
```

Règles :
- Le titre d'un constat est une **affirmation** (« La règle B1 compte un montant égal au seuil dans la
  bande, ce que la fiche exclut »), pas un thème (« Bornes »).
- Les constats sont numérotés par gravité, puis par sens (spécification → code d'abord), puis par
  source.
- *Sens* : `spécification → code` (exigence contredite, partielle ou sans test) ou
  `code → spécification` (règle non écrite, ambiguïté que le code a tranchée).
- *Statut* : `ouvert`, `corrigé dans #NN`, `spécification révisée (ADR NNNN)`, `abandonné :
  <pourquoi>`, `—` pour un constat accepté.
- Un texte de révision proposé est **complet et citable** : l'auteur doit pouvoir l'accepter tel
  quel.
