# ADR 0004 — Écrire le mécanisme de dissuasion et choisir la mesure de résultat

Date : 2026-11-09 · Statut : proposé

## Contexte

`docs/01` (« La thèse ») : « Si l'observation devient universelle, permanente et crédible,
l'équilibre change : chaque acheteur public, chaque conseil, chaque établissement se comporte comme
s'il avait un reporter compétent assigné à vie. » Et, plus bas : « Vigie ne publie rien. » Le chemin
par lequel un acheteur apprend qu'il est observé n'est écrit nulle part.

La passe `hypotheses-structurantes` du 2026-11-09 (registre, H1 et H5) a relevé que tous les effets
de dissuasion documentés passent par une **notification** ou une **publication** : audits
brésiliens dont l'effet ne se propage aux communes voisines qu'en présence d'un média local ; lettre
d'une autorité de contrôle au Chili ; saisines formelles de DoZorro ; pactes d'intégrité dont la
visibilité vient « d'abord de la presse ». Aucune étude ne montre d'effet d'une observation
silencieuse par un tiers sans pouvoir.

Les huit mesures de succès de `docs/01` comptent surtout une activité (institutions auditées,
territoires, contributeurs, rapports partagés) ; aucune n'est désignée comme celle qu'on optimise ;
aucune ne compte le dommage (une publication fausse par un utilisateur). Le post-mortem d'un portail
slovaque de contrats (2012) tire la leçon inverse : « avoir des cas d'usage par groupe cible et des
actions que vous voulez qu'ils accomplissent, et les mesurer ».

Faute de décision, la phase 1 optimise la production de rapports, pas ce qu'ils déclenchent.

## Options

1. **Écrire le mécanisme et l'étoile polaire** (recommandée) — Dans `docs/01`, dire que la
   dissuasion passe par les **actions des lecteurs qui notifient l'institution** (question en
   assemblée, demande CADA, article, saisine, auto-correction déclarée), et faire du nombre
   d'« actions notifiées » par rapport produit la mesure de résultat ; ajouter une mesure du dommage
   (publications contestées ou retirées, erreurs de règle signalées). Coût : texte seul dans
   `docs/01` ; plus tard, un suivi local optionnel des actions dans le workspace (`vigie mark` existe
   déjà pour les signaux, l'étendre aux actions). Ne touche à aucune des sept règles.
2. **Garder la thèse telle quelle et la déclarer pari** — Ajouter une phrase dans `docs/01` disant que
   l'effet d'une observation non publiée n'est pas établi et que le projet parie dessus. Coût nul ;
   mais la phase 1 reste orientée vers la production de rapports, et rien ne mesure l'impact.
3. **Prévoir un canal de notification porté par le projet** — Le projet signalerait lui-même aux
   institutions l'existence d'un rapport. Incompatible avec « Vigie ne publie rien », avec
   l'engagement 1 (aucun nœud central) et avec la règle 5 ; écarté pour mémoire.

## Recommandation

Option 1 : c'est la seule version de la thèse que les faits soutiennent, elle ne coûte qu'un texte,
et elle donne à la phase 1 une cible mesurable qui oriente l'outillage vers l'action (modèles,
destinataires, suivi) plutôt que vers le volume de rapports.

## Conséquences

`docs/01` : « La thèse » complétée d'un paragraphe sur le mécanisme ; « Mesures de succès »
réordonnées autour de l'étoile polaire, avec la mesure du dommage. `docs/02` §4.1 point 9 (« Suites
possibles ») devient la section la plus importante du rapport pour l'impact, et `docs/08` phase 1
peut prévoir le suivi des actions. `critique-specification` (lentille E1, « chaîne du signal à
l'action ») mesurera la spécification à cette étoile polaire.
