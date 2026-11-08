# Gabarits

## Entrée de `docs/DETTE.md`

Une session = une entrée, insérée **en tête** du journal, juste après l'en-tête du fichier. Date au
format `AAAA-MM-JJ`, suivie de ce qui a été touché et de son chemin.

```markdown
## AAAA-MM-JJ — <ce qui a été fait> (`<chemin/>`)

### 1. <Le risque, en une phrase affirmative> — *haute | moyenne | basse*

<Ce qui est en cause. Deux à quatre lignes. Nommer les fichiers, les champs, les fonctions, les
codes de règle.>

**Pourquoi ça casse.** <Le scénario concret. Pas « risque d'incohérence » mais « quand X arrive,
le signal Y se déclenche à tort et ni `verify` ni les tests ne le voient ».>

**Ce qu'il faut faire.** <L'action, et qui la décide si ce n'est pas l'agent.>
```

Le titre du point est une **affirmation**, pas un thème. « La règle B2 compare des montants HT et
TTC sans le savoir » plutôt que « Montants ».

Quand un point est réglé, le barrer sur place et dire par quoi :

```markdown
### ~~2. <Le risque>~~ — réglé le AAAA-MM-JJ par <ce qui l'a réglé>
```

## Entrée de `LESSONS.md`

Une leçon tient en un paragraphe : un titre en gras à l'impératif, puis le cas concret qui l'a
produite.

```markdown
**<Règle à l'impératif, une ligne.>** <Le cas qui l'a payée : ce qui s'est passé, ce que ça aurait
coûté. Deux à quatre lignes. Citer le fichier ou l'exemple réel.>
```

Se ranger dans une section existante (*Modélisation des données*, *Tests*, *Outillage*, *Méthode*).
Créer une section seulement quand trois leçons au moins la peuplent.

## ADR « proposé » — `docs/decisions/NNNN-<slug>.md`

Une question qui revient à l'auteur. Numéro suivant le dernier ADR existant, au format des ADR déjà
présents. L'auteur passe le statut à « accepté » ou « refusé » ; l'agent ne le fait jamais.

```markdown
# ADR NNNN — <La question, formulée comme une décision à prendre>

Date : AAAA-MM-JJ · Statut : proposé

## Contexte
<D'où vient la question : fichier, fonction, phrase exacte de la spécification citée entre
guillemets avec son document et sa section. Ce qui se passe aujourd'hui faute de décision.>

## Options
1. **<Option recommandée>** (recommandée) — <ce qu'elle fait, ce qu'elle coûte>.
2. **<Autre option réelle>** — <ce qu'elle fait, ce qu'elle coûte>.

## Recommandation
<Une à trois phrases : pourquoi l'option 1.>

## Conséquences
<Ce qui change dans le code, la spécification ou les rapports selon l'option retenue.>
```

## Exemple, à imiter

Dette, gravité haute, exemple **fictif** construit sur le catalogue (aucune session ne l'a encore
produit) — il montre le niveau de précision attendu :

> ### 1. La règle B1 traite un montant maximum d'accord-cadre comme un montant attribué — *haute*
>
> `vigie/rules/B/b1.py` lit `value.amount` sans regarder `value.is_maximum`. Le catalogue (B1,
> « Confiance ») exige une confiance basse quand le montant est un maximum d'accord-cadre.
>
> **Pourquoi ça casse.** Un accord-cadre dont le maximum tombe dans la bande sous un seuil produit un
> signal B1 à confiance haute ; `verify` le rejoue à l'identique, donc ne le voit pas, et le rapport
> affirme une configuration avec une assurance que la donnée ne permet pas.
>
> **Ce qu'il faut faire.** Un test de faux positif connu « montant maximum d'accord-cadre », puis lire
> `is_maximum` dans le calcul de la confiance.
