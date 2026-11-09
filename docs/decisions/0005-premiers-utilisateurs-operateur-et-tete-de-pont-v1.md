# ADR 0005 — Premiers utilisateurs, opérateur et tête de pont de la v1

Date : 2026-11-09 · Statut : proposé

## Contexte

Trois phrases de la spécification fixent pour qui, par qui et sur quoi la v1 est construite :

- `docs/01`, « Utilisateurs cibles » : « Le citoyen curieux […] Pas technique. […] C'est lui qui
  disperse le projet. » — alors que `docs/07` §13 prévoit l'installateur sans terminal pour la v1.1
  seulement, « parce que le citoyen curieux n'ouvre pas de terminal ».
- `docs/01`, « Tête de pont » : « Marchés publics, France, communes. Parce que : […] échelon
  communal = 35 000 institutions, le plus proche des citoyens et le moins couvert par la presse. »
- `docs/02` §5 : « Il n'y a pas de mode « sans humain » pour la livraison d'un rapport ; il y en a un
  pour la collecte et le calcul (`--batch`) » — l'opérateur implicite est un humain qui choisit une
  institution, lance « une commande », lit, agit.

La passe `hypotheses-structurantes` du 2026-11-09 (registre, H2, H3, H7) a relevé :

- Aucun usage non technique ni civique des DECP n'a de trace publique : les réutilisations déclarées
  sont commerciales ; la principale association du domaine ne les analyse pas et demande un
  répertoire centralisé ; la presse régionale a des moyens data concentrés chez quelques groupes.
  Le plan de lancement de `docs/08` vise lui-même des communautés techniques.
- 84 % des communes ont moins de 2 000 habitants et portent 23 % des dépenses d'équipement ; sous
  hypothèses écrites et à publication parfaite, moins d'un marché en 36 mois par commune de moins de
  500 habitants, 1,7 à 3,4 entre 500 et 2 000 ; le seuil « au moins 10 marchés en 36 mois » n'est
  atteint qu'à partir d'environ 3 500 habitants, soit 9 % des communes (68 % de l'équipement). La
  couverture des consolidés est minoritaire (7 700 acheteurs sur 70 000 en 2021 ; 35 % des communes
  présentes toutes années confondues en 2026) ; le « nombre d'offres reçues » n'existe dans les DECP
  que pour les marchés notifiés depuis le 1er janvier 2024 (règle A1 inapplicable avant).
- Les dispositifs automatisés produisent des actions quand un étage humain et un canal de saisine
  existent (DoZorro) et rien de mesurable sans (Red Flags, Hongrie : 20 consultations par jour,
  aucune enquête).

Faute de décision, la phase 1 outille un lecteur absent avant la v1.1, sur des institutions dont la
plupart n'ont pas de matière, pour un opérateur occasionnel.

## Options

1. **Réordonner pour la v1 : journaliste-data et association d'abord, lot périodique opéré par un
   agent, tête de pont communes ≥ 3 500 habitants + EPCI + départements** (recommandée) — `docs/01`
   : ordre des utilisateurs pour la v1 (journaliste local et association, puis contributeur, puis
   institution, le citoyen non technique explicitement renvoyé à la v1.1) ; mode principal =
   `--batch` périodique sur un territoire, delta, et **file d'actions** à traiter par un humain aux
   points de contrôle (règles 1 à 4 et `docs/02` §5 inchangés : rien n'est livré sans humain) ; mode
   secondaire = « une commande » pour une institution. `config/strates.fr.yaml` : strates sur les 26
   tranches DGCL ; le playbook dit dès son en-tête ce qu'il ne peut pas dire sous 3 500 habitants.
   Coût : texte dans `docs/01`, `docs/02` §2 et §9, en-tête du playbook, `docs/08` (lot et delta sont
   déjà en phase 1 : c'est un réordonnancement, pas un ajout). Les quatre engagements et les sept
   règles ne bougent pas.
2. **Garder l'ordre actuel et déclarer les limites** — Une section « Limites » du playbook et une
   phrase de `docs/01` disant que les petites communes produiront surtout des signaux de
   transparence (G1) et que la v1 s'adresse de fait aux techniciens. Coût nul ; mais la phase 1 reste
   écrite pour un lecteur absent, et les exigences « zéro jargon », « quinze minutes », « mode voisin »
   consomment du travail avant d'avoir un lecteur.
3. **Changer de tête de pont** — Les EPCI, départements ou hôpitaux comme première cible, les
   communes ensuite. Plus de matière par institution, mais la spécification entière (playbook,
   strates, fiches, templates) a été écrite pour les communes ; coût élevé pour un gain que l'option
   1 obtient en gardant les communes qui ont de la matière.

## Recommandation

Option 1 : elle suit les faits (qui utilise les données, où est la matière, ce qui produit des
actions), ne coûte que du texte et un réordonnancement de livrables déjà prévus, et laisse le citoyen
non technique hériter en v1.1 d'un outil éprouvé plutôt que de servir de premier cobaye sur une
commune vide. Le test T1 (détectabilité par strate, une journée agent avec réseau) et le pilote A
(audit à la main) peuvent la confirmer ou l'infirmer avant d'écrire une ligne de code.

## Conséquences

`docs/01` : « Utilisateurs cibles » réordonnés pour la v1, « Tête de pont » précisée (≥ 3 500 hab.,
EPCI, départements), le mode agent écrit comme mode principal. `docs/02` §2 : parcours 2.3
(association) et 2.2 (journaliste) en premier ; §9 : la cible de temps devient celle d'un lot.
`docs/08` phase 1 : lot, delta et file d'actions avant « zéro jargon » et « mode voisin » ; la v1.1
porte l'installateur et le parcours citoyen. `config/strates.fr.yaml` : tranches DGCL sourcées.
`playbooks/fr/audit-marches-publics-commune` : en-tête et section « Limites ». Les règles 1 à 7 et
`docs/02` §5 restent tels quels.
