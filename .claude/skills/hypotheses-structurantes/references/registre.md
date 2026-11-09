# Format du registre et de la passe — `docs/fondations/`

Deux fichiers. Le **registre** (`registre.md`) est vivant : une entrée par hypothèse, réécrite à
chaque passe, jamais empilée ; il dit où en est chaque pari. La **passe datée** (`AAAA-MM-JJ.md`)
n'est jamais réécrite : elle dit ce qui a changé ce jour-là, sur quels faits, et porte les calculs,
les sources et les pilotes.

## Entrée du registre

```markdown
### H<n>. <L'hypothèse, en une phrase affirmative, telle que la spécification la pose>

- **La spécification dit** : « <citation exacte> » (<fichier>, section).
- **Mécanisme supposé** : <par quel chemin cette phrase produit l'impact qu'elle promet — en une ou
  deux phrases ; s'il n'est écrit nulle part dans la spécification, le dire>.
- **Ce qui la falsifierait** : <l'observation ou le chiffre qui obligerait à l'abandonner>.
- **Ce qui tomberait avec elle** : <forme, ordre des utilisateurs, tête de pont, modèle de
  croissance, gouvernance, règle 4 à 7, ADR>.
- **Statut** : confirmée | infirmée | à tester | pari assumé — <date>, <passe>.
- **Faits** : <une ligne par fait, avec sa force (1 calcul, 2 usage révélé, 3 littérature, 4
  analogue), sa source (URL, date consultée) ou son script (en annexe de la passe)>.
- **Argument contraire** : <le plus fort que la passe a pu écrire, et pourquoi il ne renverse pas le
  statut — ou pourquoi il l'a renversé>.
- **Test ou pilote** : <pour « à tester » : la procédure, qui l'exécute (agent / auteur), le coût en
  heures, le résultat qui falsifierait ; renvoi à la passe qui le décrit>.
- **Alternative** : <une seule, la meilleure, si le statut est infirmée ou à tester : ce qu'elle
  change, la mesure de succès de docs/01 qu'elle sert, impact attendu, coût ; recommandation en
  premier. Sinon « aucune »>.
- **Décision** : <aucune | ADR NNNN proposé | tranché par l'auteur le AAAA-MM-JJ : …>.
```

Règles :
- Le titre est l'hypothèse **telle que la spécification la pose**, pas sa négation ni sa critique :
  « Le citoyen curieux disperse le projet », pas « Le citoyen ne dispersera pas ».
- Une hypothèse qui exigerait d'affaiblir une des règles 1, 2 ou 3 ne reçoit pas d'alternative :
  son champ « Alternative » dit « impossible (règle n) ».
- Un fait sans URL ni script n'est pas un fait ; il n'est pas écrit.
- « Pari assumé » n'est pas un échec de la passe : c'est la reconnaissance écrite qu'aucune preuve
  n'existe avant la réalité. Son entrée dit quelle réalité le tranchera (quel pilote, quelle phase).

## Statuts

| Statut | Signifie | Change quand |
| --- | --- | --- |
| confirmée | des faits pour, aucun contre | un fait contraire apparaît |
| infirmée | des faits contre ; l'alternative attend la décision de l'auteur | l'auteur tranche |
| à tester | un test ou un pilote est défini, pas encore exécuté | le résultat revient |
| pari assumé | aucune preuve possible avant la réalité ; l'auteur parie, par écrit | la réalité parle |

## Gabarit du registre

```markdown
# Hypothèses structurantes — registre

Dernière passe : [AAAA-MM-JJ](AAAA-MM-JJ.md). Hors champ : règles 1, 2 et 3 de `CLAUDE.md`
(décision de l'auteur, 2026-11-09).

| # | Hypothèse | Statut | Depuis | Décision |
| --- | --- | --- | --- | --- |

## Utilisateurs visés : comment chacun est observé

| Utilisateur (`docs/01`) | Observé par | Biais |
| --- | --- | --- |
| citoyen curieux | <traces directes | analogie | pas du tout> | <…> |
| journaliste local | | |
| association | | |
| contributeur technique | | |
| institution | | |
| opérateur agent (si retenu) | | |

## Entrées

<Une entrée au format ci-dessus par hypothèse, dans l'ordre des numéros ; les numéros ne sont jamais
réattribués.>

## Pilotes en attente

| Pilote | Hypothèses testées | Exécutant | Coût | Décrit dans |
| --- | --- | --- | --- | --- |
```

## Gabarit de la passe datée

```markdown
# Hypothèses structurantes — passe du AAAA-MM-JJ

Commit : `<sha court>`. Modèles : session <tel qu'elle le connaît> ; sous-agents `fable`, effort
`max`. Passe précédente : [AAAA-MM-JJ](AAAA-MM-JJ.md) ou « aucune, première passe ». Déclencheur :
<avant la première critique | après les premiers rapports réels | nouveau domaine : … | pari
rencontré : H<n>>.

## Ce qui a changé

<Une ligne par hypothèse dont le statut, les faits ou l'alternative ont changé : H<n> — ancien
statut → nouveau — le fait qui l'a fait bouger. Puis les hypothèses nouvelles, et celles retirées
(avec la raison : détail renvoyé à critique-specification, tranchée par l'auteur).>

## Le biais de cette passe

<Ce que les sources consultées sur-représentent ; quels utilisateurs visés n'ont été observés que
par analogie ou pas du tout ; ce que ça interdit de conclure.>

## Faits relevés

<Par hypothèse : les faits avec leur force, leur source (URL, date) ou leur calcul (hypothèses,
résultat, script en annexe). Ce que le relevé du monde a trouvé et que la vérification a écarté,
avec la raison, en une ligne chacun.>

## Pilotes préparés

<Par pilote : hypothèse(s) testée(s), procédure pas à pas pour l'auteur, ce qu'il faut observer, le
résultat qui falsifierait, le coût en heures, la façon de rapporter le résultat sans nommer
l'institution ni la personne.>

## Décisions demandées à l'auteur

<Une ligne par ADR proposé : numéro, question, recommandation.>

## Annexe — scripts

<Les scripts jetables recopiés tels qu'ils ont tourné, avec la date des données et le portail
d'origine. Ils vivent ici et nulle part ailleurs dans le dépôt.>
```
