# Format des constats et du rapport — `docs/critiques/AAAA-MM-JJ.md`

## Format d'un constat (sous-agents et rapport)

```markdown
### <n>. <Le constat, en une phrase affirmative>

- **Lentille** : <A1, B2, E1…>. **Gravité** : bloquant | majeur | mineur. **Coût** : texte seul |
  texte et code | texte, code et temps humain | données ou ressources nouvelles.
- **La spécification dit** : « <citation exacte> » (<fichier>, §x ou code de règle). Ou, pour une
  absence : « rien » — termes cherchés en vain : <termes>.
- **Ce qui en dépend** : <le signal, la preuve ou la phrase de rapport, le scénario concret, le sens
  de l'erreur, qui peut l'exploiter. Pour un constat d'utilité (E) : le lecteur de `docs/02` §2,
  l'action concrète qui devient possible, impossible ou plus tardive, et la mesure de succès de
  `docs/01` qui bouge — les trois, nommés>.
- **Calcul** : <s'il y a lieu : hypothèses, résultat, et le script recopié dans le rapport ou en
  annexe (il vit hors du dépôt)>.
- **Sources extérieures** : <URL consultée et date, ou « aucune »>.
- **Réponse minimale** : rien | limite déclarée | précision | suppression | re-priorisation | ajout.
  <Pour un texte : « ancien » → « nouveau », fichier et section. Pour une re-priorisation : la ligne
  de `docs/08` actuelle, la ligne proposée, le motif chiffré. Pour une question à l'auteur : options
  réelles, coût de chacune, recommandation en premier.>
- **Défense** : <ce que la défense a opposé, et pourquoi le constat tient quand même>.
```

Une **tension** (règle 11 de la passe : une proposition d'utilité qui coûterait de la crédibilité, ou
l'inverse) se rédige au même format, avec « tension » à la place de la gravité, ce qu'elle gagnerait
et ce qu'elle coûterait dans « Ce qui en dépend », et jamais de texte proposé : la décision est à
l'auteur.

## Gravité

Les deux faces de la passe ont chacune leur définition ; un constat relève de l'une ou de l'autre,
jamais des deux.

Crédibilité (lentilles A à D) :

- **Bloquant** : un rapport affirmerait quelque chose de faux ou de non prouvé, un signal serait
  trompeur pour le lecteur visé, un tiers ne pourrait pas vérifier ou rejouer un rapport qu'on dit
  vérifiable, une donnée de personne serait exposée contre `docs/09`, ou le risque juridique est
  sérieux. À régler avant l'étape visée.
- **Majeur** : un spécialiste exigerait la correction pour accepter la méthode, mais le rapport reste
  juste si le lecteur connaît la limite. À régler avant l'étape visée, ou à déclarer comme limite par
  décision écrite de l'auteur.
- **Mineur** : une ambiguïté ou un manque sans effet sur un signal ni un rapport. Une ligne chacun,
  regroupés ; jamais de texte proposé au-delà d'une phrase.

Utilité (lentille E) :

- **Bloquant** : la spécification promet au lecteur de l'étape visée quelque chose qu'elle ne peut pas
  tenir telle qu'écrite (`docs/02` §2, §9 et §10 : les quinze minutes, la lecture sans formation,
  « n'importe quelle commune française présente dans les DECP »…), ou le premier persona de l'étape
  n'a, à la sortie du rapport, aucune action outillée (`docs/01`, engagement 4). À régler ou à
  re-prioriser avant l'étape visée : la première déception décide de la dispersion.
- **Majeur** : une mesure de succès de `docs/01` n'a aucun mécanisme de la spécification qui la serve ;
  une exigence coûteuse n'en sert aucune ; ou un livrable à fort impact est placé après un livrable à
  faible impact sans raison écrite dans `docs/08` ou un ADR. À trancher par l'auteur avant l'étape
  visée : re-priorisé, corrigé, ou report motivé par écrit.
- **Mineur** : une friction pour un lecteur, sans effet sur une action ni une mesure de succès. Une
  ligne chacun, regroupés.

Dans le doute entre deux gravités, prendre la plus basse et dire pourquoi : surestimer pousse à
réécrire.

## Gabarit du rapport

Un fichier par passe, jamais réécrit une fois la passe close, sauf la colonne *Statut*, que les PR
de révision mettent à jour. La passe suivante le lit en premier.

```markdown
# Critique de la spécification du AAAA-MM-JJ

Commit : `<sha court>`. Protocole : <PROTOCOL_VERSION, ou l'en-tête de docs/06>. Catalogue :
<version>. Étape visée : <protocole 0.1 prêt à être codé | fin de phase n | PROTOCOL_VERSION x.y |
lancement public | passe périodique — prochaine étape : …>. Modèles : session <tel qu'elle le
connaît> ; sous-agents `fable`, effort `max` ; lecture naïve `fable`.
Passe précédente : [AAAA-MM-JJ](AAAA-MM-JJ.md) (ou « aucune, première passe »).

## Prêt pour la suite ?

**<Oui | Non>.** <Si non : ce qui manque, dans l'ordre où le faire, en une ligne par point.>

## Bilan

| Gravité | Crédibilité (A–D) | Utilité (E) | Dont questions à l'auteur | Repris de la passe précédente |
| --- | ---: | ---: | ---: | ---: |
| bloquant | | | | |
| majeur | | | | |
| mineur | | | | |
| tension | — | — | | |

<Trois phrases au plus sur ce qui menace le plus la crédibilité des rapports ; trois au plus sur ce
qui limite le plus leur utilité et l'impact du projet ; une sur ce qui est solide.>

## Lecture naïve

<Ce que le sous-agent a reçu (texte fixe, fiche, signal synthétique) ; ce qu'il a compris, ce qu'il
dirait à un voisin, ce qu'il ferait ensuite — ses mots, en cinq lignes au plus. C'est une donnée,
reprise par les constats C1 et E1 qui s'y appuient.>

## Tableau des constats

| # | Gravité | Lentille | Où | Constat | Réponse minimale | Coût | Statut |
| --- | --- | --- | --- | --- | --- | --- | --- |

## Constats bloquants et majeurs

<Un bloc au format ci-dessus par constat, numérotés par gravité puis par fichier ; crédibilité et
utilité dans la même liste, la lentille dit laquelle.>

## Tensions

<Un bloc par tension, ou « aucune ».>

## Constats mineurs

<Une ligne chacun : où — constat — réponse minimale.>

## Écartés par la vérification ou la défense

<Une ligne chacun : constat proposé par une lentille — raison de l'écart (déjà traité dans <fichier>
§x, ADR NNNN, prévu en phase n de docs/08, conséquence non démontrée, mesure de succès non nommée,
coût sans gain). Cette liste montre que la passe a lu sans complaisance et n'a rien gardé par
complaisance non plus.>

## Ce qui est solide

<Les choix de la spécification qu'un spécialiste approuverait et ceux qu'un lecteur visé
utiliserait tels quels, en quelques lignes, avec leur emplacement : la passe ne sert pas qu'à
trouver des fautes, et ce qui tient doit pouvoir être défendu tel quel.>

## ADR proposés

<ADR NNNN, NNNN+1… avec leur question en une ligne ; ou « aucun ».>
```

Règles :
- Le titre d'un constat est une **affirmation** (« Une source nationale incomplète produit un signal
  G1 contre l'institution » ; « Le citoyen de la commune médiane n'obtient aucun signal examinable
  avec le playbook de phase 1 »), pas un thème (« Qualité des données », « Utilité »).
- Un constat repris d'une passe précédente garde son numéro d'origine entre parenthèses.
- Le statut est `ouvert`, `tranché : ADR NNNN`, `révisé (<fichier>, <version>)`, `re-priorisé
  (docs/08, <commit>)` ou `déclaré comme limite`.
