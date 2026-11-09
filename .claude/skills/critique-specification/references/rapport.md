# Format des constats et du rapport — `docs/critiques/AAAA-MM-JJ.md`

## Format d'un constat (sous-agents et rapport)

```markdown
### <n>. <Le constat, en une phrase affirmative>

- **Lentille** : <A1, B2…>. **Gravité** : bloquant | majeur | mineur. **Coût** : texte seul |
  texte et code | texte, code et temps humain | données ou ressources nouvelles.
- **La spécification dit** : « <citation exacte> » (<fichier>, §x ou code de règle). Ou, pour une
  absence : « rien » — termes cherchés en vain : <termes>.
- **Ce qui en dépend** : <le signal, la preuve ou la phrase de rapport, le scénario concret, le sens
  de l'erreur, qui peut l'exploiter>.
- **Calcul** : <s'il y a lieu : hypothèses, résultat, et le script recopié dans le rapport ou en
  annexe (il vit hors du dépôt)>.
- **Sources extérieures** : <URL consultée et date, ou « aucune »>.
- **Réponse minimale** : rien | limite déclarée | précision | suppression | ajout.
  <Pour un texte : « ancien » → « nouveau », fichier et section. Pour une question à l'auteur :
  options réelles, coût de chacune, recommandation en premier.>
- **Défense** : <ce que la défense a opposé, et pourquoi le constat tient quand même>.
```

## Gravité

- **Bloquant** : un rapport affirmerait quelque chose de faux ou de non prouvé, un signal serait
  trompeur pour le lecteur visé, un tiers ne pourrait pas vérifier ou rejouer un rapport qu'on dit
  vérifiable, une donnée de personne serait exposée contre `docs/09`, ou le risque juridique est
  sérieux. À régler avant l'étape visée.
- **Majeur** : un spécialiste exigerait la correction pour accepter la méthode, mais le rapport reste
  juste si le lecteur connaît la limite. À régler avant l'étape visée, ou à déclarer comme limite par
  décision écrite de l'auteur.
- **Mineur** : une ambiguïté ou un manque sans effet sur un signal ni un rapport. Une ligne chacun,
  regroupés ; jamais de texte proposé au-delà d'une phrase.

Dans le doute entre deux gravités, prendre la plus basse et dire pourquoi : surestimer pousse à
réécrire.

## Gabarit du rapport

Un fichier par passe, jamais réécrit une fois la passe close, sauf la colonne *Statut*, que les PR
de révision mettent à jour. La passe suivante le lit en premier.

```markdown
# Critique de la spécification du AAAA-MM-JJ

Commit : `<sha court>`. Protocole : <PROTOCOL_VERSION>. Catalogue : <version>. Étape visée : <fin de
phase n | PROTOCOL_VERSION x.y | lancement public>. Modèles : session <tel qu'elle le connaît> ;
sous-agents `fable`, effort `max`.
Passe précédente : [AAAA-MM-JJ](AAAA-MM-JJ.md) (ou « aucune, première passe »).

## Prêt pour la suite ?

**<Oui | Non>.** <Si non : ce qui manque, dans l'ordre où le faire, en une ligne par point.>

## Bilan

| Gravité | Nombre | Dont questions à l'auteur | Repris de la passe précédente |
| --- | ---: | ---: | ---: |
| bloquant | | | |
| majeur | | | |
| mineur | | | |

<Trois phrases au plus : ce qui menace le plus la crédibilité des rapports, et ce qui est solide.>

## Tableau des constats

| # | Gravité | Lentille | Où | Constat | Réponse minimale | Coût | Statut |
| --- | --- | --- | --- | --- | --- | --- | --- |

## Constats bloquants et majeurs

<Un bloc au format ci-dessus par constat, numérotés par gravité puis par fichier.>

## Constats mineurs

<Une ligne chacun : où — constat — réponse minimale.>

## Écartés par la vérification ou la défense

<Une ligne chacun : constat proposé par une lentille — raison de l'écart (déjà traité dans <fichier>
§x, ADR NNNN, conséquence non démontrée, coût sans gain). Cette liste montre que la passe a lu sans
complaisance et n'a rien gardé par complaisance non plus.>

## Ce qui est solide

<Les choix de la spécification qu'un spécialiste approuverait, en quelques lignes, avec leur
emplacement : la passe ne sert pas qu'à trouver des fautes, et ce qui tient doit pouvoir être
défendu tel quel.>

## ADR proposés

<ADR NNNN, NNNN+1… avec leur question en une ligne ; ou « aucun ».>
```

Règles :
- Le titre d'un constat est une **affirmation** (« Une source nationale incomplète produit un signal
  G1 contre l'institution »), pas un thème (« Qualité des données »).
- Un constat repris d'une passe précédente garde son numéro d'origine entre parenthèses.
- Le statut est `ouvert`, `tranché : ADR NNNN`, `révisé (<fichier>, <version>)` ou `déclaré comme
  limite`.
