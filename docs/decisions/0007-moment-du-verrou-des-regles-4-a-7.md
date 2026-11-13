# ADR 0007 — Moment du verrou des règles 4 à 7

Date : 2026-11-09 · Statut : proposé

## Contexte

`docs/10` §2 : « les sept règles non négociables du `CLAUDE.md` ne peuvent être modifiées qu'à
l'unanimité et après consultation publique. » Les sept règles ont été écrites les 7 et 8 novembre
2026, avant toute ligne de code et tout contact avec un utilisateur. L'auteur a décidé le 2026-11-09
que les règles 1, 2 et 3 (jamais d'accusation ; institutions, jamais individus ; pas de preuve, pas
de signal) sont hors du champ de toute remise en cause : aucun gain d'impact ne compense leur perte.
La question ne porte donc que sur le **moment** où le verrou devient effectif pour les règles 4 à 7
(déterministe d'abord ; local-first ; agnostique au modèle ; reproductible), dont deux sont des choix
d'architecture que le registre des hypothèses examine (H8 sur la règle 5).

La passe `hypotheses-structurantes` du 2026-11-09 (registre, H9) a relevé que les cas documentés de
principes fondateurs contredits par la réalité portent sur la licence ou la liberté logicielle, pas
sur des principes de contenu : Debian a amendé son Contrat social en 2022 (289 voix contre 63,
au-dessus du seuil 3:1) après des années de contournement de fait ; Wikipédia a verrouillé ses
principes après une phase volontairement peu réglée, et son cofondateur regrette l'inverse. Aucun
fait ne tranche le moment optimal : c'est un pari.

Faute de décision, le texte actuel verrouille dès aujourd'hui, à l'unanimité d'un seul mainteneur,
des choix d'architecture que ce registre et la première `critique-specification` sont en train
d'examiner.

## Options

1. **Verrou effectif au lancement public de la phase 1 pour les règles 4 à 7 ; règles 1 à 3
   verrouillées dès maintenant** (recommandée) — Une phrase dans `docs/10` §2 : jusqu'au lancement
   public, les règles 4 à 7 se révisent par ADR accepté par l'auteur, après une passe du registre ;
   à partir du lancement, la règle d'unanimité et de consultation publique s'applique aux sept.
   Coût : une phrase. Risque : une révision de 4 à 7 avant le lancement, décidée seul — c'est
   précisément ce que le registre rend visible et argumenté.
2. **Verrou dès maintenant pour les sept** (texte actuel) — Protection maximale contre la dérive ;
   mais toute leçon des pilotes ou du test T1 qui toucherait les règles 5 ou 6 exigerait une
   « consultation publique » sans public, ou serait contournée en silence, comme chez Debian.
3. **Pas de verrou formel avant trois mainteneurs** — Cohérent avec `docs/10` §2 (« trois
   mainteneurs avant la phase 3 ») ; mais laisse les règles 1 à 3 sans protection écrite pendant la
   phase 1, ce que l'auteur a exclu.

## Recommandation

Option 1 : elle verrouille tout de suite ce qui ne doit jamais bouger, laisse respirer ce que le
projet est en train d'apprendre, et fixe une date de verrou liée à un fait (le lancement public,
quand des tiers commencent à compter sur les règles) plutôt qu'à une page écrite en deux jours.

## Conséquences

`docs/10` §2 : la phrase sur les sept règles distingue les règles 1 à 3 (verrouillées) des règles 4 à
7 (verrouillées au lancement public). `CLAUDE.md` « Où l'agent s'arrête » : inchangé (une révision
d'une règle 4 à 7 reste une proposition d'ADR, jamais un acte de l'agent). `hypotheses-structurantes`
: son hors-champ ne change pas ; après le lancement, une hypothèse touchant une règle 4 à 7 s'écrit
comme tension, plus comme alternative.
