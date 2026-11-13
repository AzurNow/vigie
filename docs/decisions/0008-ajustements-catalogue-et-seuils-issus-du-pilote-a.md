# ADR 0008 — Ajustements du catalogue et des seuils issus du pilote A

Date : 2026-11-13 · Statut : proposé

## Contexte

Le pilote A ([compte rendu](../fondations/2026-11-13-pilote-a.md)) a appliqué à la main les règles A1,
A3, B1, B2, C2, C3, D1, E1, E2, G2 et G4 à deux communes réelles, sur le consolidé DECP du 2026-11-13.
Sur 17 déclenchements, 6 sont des faux positifs évidents que les fiches ne prévoient pas, une règle
agrégée est dégénérée sur une strate, et `config/seuils.fr.yaml` est en retard sur les textes de 2026.
Tout cela relève de « Où l'agent s'arrête » (fiches du catalogue, seuils et paramètres de `config/`,
levée de mentions **[À VÉRIFIER]**) : l'agent propose, l'auteur décide.

Faits relevés :

1. **Lots d'une même consultation (B2, et B1 par ricochet).** Les 5 déclenchements B2 portent tous sur
   des lots notifiés le même jour, au même acheteur, issus d'une consultation publiée (2 à 6 offres
   par lot). La somme des lots franchit le seuil que chaque lot pris isolément ne franchit pas : c'est
   la définition même de l'allotissement, pas un fractionnement. La fiche B2 cite les marchés
   subséquents d'un accord-cadre, pas les lots.
2. **Seuil de dispense franchi en procédure concurrentielle (B1).** Un marché de travaux situé dans les 5 %
   sous le seuil de dispense de 100 000 € a été passé en procédure adaptée et a reçu 4 offres : le
   contournement que la fiche vise n'a pas d'objet quand l'acheteur a conduit une procédure plus
   exigeante que requis.
3. **Percentile nul (A3).** Dans la strate C2 (500 à 1 999 habitants, 1 586 communes ayant au moins 5
   marchés), le 80e percentile de la part de marchés sans publicité vaut 0 : un seul marché sans
   publicité, même de 1 000 €, suffirait à déclencher le signal agrégé.
4. **Seuils (`config/seuils.fr.yaml`)**, relevés sur marche-public.fr (reprise de Légifrance), le site
   de la DAJ refusant les accès automatisés :
   - dispense fournitures et services : 40 000 € jusqu'au 2026-03-31, **60 000 € depuis le
     2026-04-01** (décret n° 2025-1386, art. 1er 1° b et art. 3) — absent du fichier ;
   - dispense travaux : **100 000 €, permanent depuis le 2026-01-01** (même décret, art. 1er 1° a) —
     le fichier le dit temporaire, sans date de fin ;
   - publication des DECP : **40 000 € depuis le 2024-01-01** (R2196-1) — le fichier dit 25 000 € ;
   - seuils européens 2026-2027 : 216 000 € (fournitures et services, collectivités), 432 000 €
     (entités adjudicatrices), 140 000 € (État), 5 404 000 € (travaux) — absents du fichier.
5. **Connecteur `fr/decp`** : sur les lignes de modification du flux comptable de la DGFiP,
   `dateNotification` porte la date de la modification ; 0,27 % des lignes du consolidé ont un
   identifiant d'attributaire de moins de 9 caractères (G4).

## Options

1. **Tout reprendre dans une PR de révision unique** — fiches B1, B2, A3 (et le plancher des règles à
   percentile), `config/seuils.fr.yaml`, `docs/11` pour le connecteur. Un seul passage de relecture
   pour l'auteur ; la PR est plus longue, et un seuil contesté retient le reste.
2. **Deux PR** — (a) les seuils, faits sourcés que l'auteur vérifie sur la source officielle (lever un
   [À VÉRIFIER] exige la source citée) ; (b) les fiches et le plancher, qui sont des choix de méthode.
   Deux relectures courtes, et chacune avance à son rythme.
3. **Attendre le code** — garder ces constats dans le registre et ne réviser qu'après les premières
   règles implémentées. Coût nul maintenant ; mais B1, B2 et A3 font partie de la phase 0 et de la
   phase 1, et les tests se rédigeraient sur des fiches dont on sait déjà qu'elles produisent des
   faux positifs systématiques.

## Recommandation

Option 2. Les seuils sont des faits datés dont l'auteur vérifie seul la source officielle (le site de
la DAJ, que l'agent ne peut pas lire) ; les fiches sont des choix de méthode. Contenu proposé pour la
seconde PR :

- **B2** : calcul « hors lots d'une même consultation » (même acheteur, même date de notification,
  même objet ou même référence de consultation), et explication ordinaire « lots d'une consultation
  unique, allotissement ».
- **B1** : confiance basse, ou pas de signal, quand la procédure suivie est plus exigeante que celle
  dont le seuil dispense et que plusieurs offres ont été reçues ; explication ordinaire
  correspondante.
- **Règles à percentile de strate (A1, A3, et les suivantes)** : un plancher absolu en plus du
  percentile (par exemple, au moins 2 marchés concernés et une part supérieure à une valeur fixe),
  valeur à fixer par l'auteur.
- **`docs/11`** : les deux points du connecteur (date des lignes de modification, identifiants
  invalides).

## Conséquences

`docs/03` (fiches B1, B2 et la note sur les règles à percentile), `config/regles.fr.yaml` (plancher),
`config/seuils.fr.yaml` (valeurs 2026, seuil de publication), `docs/11` ; incrément de la version du
catalogue. Aucun changement des règles 1 à 7 ni des formats du protocole. Les tests des règles B1 et
B2 en phase 0 incluent un cas « lots d'une même consultation » comme faux positif connu.
