# ADR 0006 — Adossement dès la phase 1 et artefact partagé du commun

Date : 2026-11-09 · Statut : proposé

## Contexte

L'ADR 0001 part de « Un particulier, sans organisation, veut l'impact le plus large possible. »
`docs/10` §2 : « Phase solo (phases 0-1) : un mainteneur. » et « Hébergement institutionnel
(phase 3+) : adossement à une structure à but non lucratif existante ». `CLAUDE.md` : « Les
plateformes, ce sont les autres qui les construisent, mille fois. » `docs/01` : « Le modèle est
OpenStreetMap : personne ne cartographie le monde, chacun cartographie son quartier. »

La passe `hypotheses-structurantes` du 2026-11-09 (registre, H4, H6, H8) a relevé :

- Aucun projet de veille des marchés publics porté par un particulier seul n'a d'impact documenté ;
  ceux qui en ont un (Civio, DoZorro) sont des organisations, et l'impact est venu par le
  journalisme et les saisines. L'association civic-tech française la plus connue, entièrement
  bénévole, a appelé à sa reprise en 2022 pour épuisement. La principale association du domaine en
  France n'a pas de capacité d'analyse de marchés locaux ; une autre annonce un projet avec une école
  normale supérieure pour « mobiliser les données numériques afin d'évaluer les conditions de
  passation de marchés publics ».
- Les communs de connecteurs par territoire ont atteint des dizaines de contributeurs quand une
  organisation financée les portait (EveryPolitician) et se sont arrêtés avec elle ; celui qui vit
  sans organisation dominante (OpenAddresses) a une demande économique massive et un artefact
  partagé (un fichier mondial produit par un pipeline central). Chez OpenStreetMap, la rétention des
  nouveaux dépend d'un retour humain dans les 28 heures.
- Pour les standards ouverts, l'adoption suit un produit déjà utilisé et un acteur qui en a besoin
  (Represent, GTFS) ; les protocoles sans consommateur nommé sont restés sans plateforme ou sont
  morts avec leur implémentation de référence (OpenSpending, Popolo).

Faute de décision, les trois chemins dont dépend l'impact — visibilité, premiers lecteurs,
contributeurs — reposent sur une personne et sur l'espoir que « d'autres » construisent.

## Options

1. **Chercher un partenaire opérationnel dès la phase 1, réécrire « mille fois » en pari, et donner
   au commun un artefact partagé visible** (recommandée) — Un partenaire qui analyse déjà ou veut
   analyser des marchés locaux (une association de transparence et son laboratoire de données ; un
   collectif de presse régionale ; un pôle data de quotidien) comme premier utilisateur, canal de
   publication et relais de contributeurs, sous les conditions anti-capture de `docs/10` §2 et §4
   (pas de siège à la gouvernance, pas de conditionnalité sur les règles, financements publics). Dans
   `CLAUDE.md` et `docs/01`, « mille fois » devient un pari explicite, et l'implémentation de référence
   est désignée comme le produit de la v1 (ce qu'elle est). Dans le dépôt, une **carte de couverture**
   des fiches et connecteurs (quelle commune a une fiche, vérifiée quand) générée par un script — un
   fichier statique, sans serveur — et un engagement de réponse à toute première contribution sous
   48 heures dans `docs/10` §3. Coût : des conversations ; un script et une page ; trois phrases.
2. **Garder le calendrier (adossement en phase 3), mais préparer le terrain** — Nommer dans `docs/10`
   les partenaires possibles et leur écrire au lancement public. Coût nul maintenant ; mais la phase
   1 et son lancement (20 communes, billet, annonces) se font seul, sans canal de publication ni
   relais, et les leçons des pilotes B et C arrivent après l'effort.
3. **Devenir soi-même une organisation** — Créer une association. `docs/10` §2 l'écarte déjà (charge
   administrative, risque de capture) ; écarté pour mémoire.

## Recommandation

Option 1 : les trois hypothèses dont dépend l'impact pointent vers la même réponse, elle ne coûte
pas de code, et elle est compatible avec tout ce que le projet refuse (aucun serveur, aucun nœud
central, aucun financeur à la gouvernance). Le pilote B (un rapport, une personne) est l'occasion
naturelle du premier contact.

## Conséquences

`docs/10` §2 : « Phase solo » devient « un mainteneur et un partenaire opérationnel » avec les
conditions anti-capture ; §3 : engagement de réponse sous 48 heures. `CLAUDE.md` « Ce qu'est ce
projet » et `docs/01` « Pourquoi un protocole » : « mille fois » reformulé en pari. ADR 0001 : la
conséquence « pas de vue agrégée nationale » est complétée par la carte de couverture des fiches
(données de sources, pas d'audits : `docs/10` §3 reste respecté). `docs/08` phase 1 : le lancement
se fait avec le partenaire.
