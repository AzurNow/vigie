# 03 — Catalogue des signaux (red flags) : marchés publics

Version du catalogue : `0.1.0`. Chaque règle a un code stable. Une règle retirée garde son code (jamais réattribué). Les seuils numériques ne sont jamais dans ce document ni dans le code : ils sont dans `config/seuils.<pays>.yaml` et `config/regles.<pays>.yaml`, avec leur source.

Les règles s'appuient sur les travaux de l'Open Contracting Partnership (guide des red flags), de la Banque mondiale, de l'OCDE, du système italien ANAC et de Prozorro/DoZorro (Ukraine), ainsi que sur la doctrine française (DAJ, Cour des comptes, CRC). Les références exactes sont à compléter dans `docs/references.md` **[À VÉRIFIER]**.

## Structure d'une fiche

```
Code — Nom
Famille · Sévérité par défaut · Phase
Détecte : ...
Données : champs OCDS/DECP requis
Calcul : règle déterministe
Paramètres : clés dans config/
Confiance : ce qui la fait baisser
Explications ordinaires : liste (reprise telle quelle dans le rapport)
Vérifier ensuite : liste concrète (reprise telle quelle dans le rapport)
Littérature : pourquoi c'est examiné (formulation au conditionnel, seule section où une notion juridique peut apparaître)
```

Toute fiche est reprise dans le rapport **sans reformulation** par le LLM pour les sections « explications ordinaires », « vérifier ensuite » et « pourquoi c'est examiné ». Le LLM ne rédige que le constat et la synthèse.

---

## Famille A — Concurrence

### A1 — Offre unique sur procédure ouverte
A · moyenne · phase 1
- **Détecte** : marché passé en procédure formalisée ou adaptée avec publicité, pour lequel une seule offre a été reçue, ou attributaire unique alors que le nombre d'offres n'est pas renseigné sur une période où d'autres marchés similaires en reçoivent plusieurs.
- **Données** : `procedure`, `nombre_offres_recues` (souvent absent des DECP → confiance basse), `attributaires`, `cpv`, `montant`.
- **Calcul** : signal par marché si `nombre_offres_recues == 1` ; signal agrégé par acheteur si la part de marchés à offre unique sur la période dépasse le percentile configuré de la strate.
- **Paramètres** : `A1.percentile_strate`, `A1.min_marches`.
- **Confiance** : basse si `nombre_offres_recues` absent ; moyenne si déduit ; haute si renseigné.
- **Explications ordinaires** : marché très spécialisé ou de faible montant ; marché de maintenance d'un équipement propriétaire ; zone rurale avec peu de prestataires ; publication en période estivale.
- **Vérifier ensuite** : le rapport d'analyse des offres (demande CADA) ; l'avis de publicité (BOAMP/TED) et son délai ; si le même fournisseur gagne aussi les autres marchés de l'acheteur (voir C2).
- **Littérature** : l'absence de concurrence effective est l'indicateur le plus documenté d'un risque d'attribution orientée (cahier des charges taillé, publicité insuffisante).

### A2 — Délai de remise des offres court
A · faible · phase 1
- **Détecte** : durée entre publication de l'avis et date limite de remise des offres inférieure au minimum légal ou au percentile bas de la strate pour la procédure et le montant.
- **Données** : `date_publication_avis` (BOAMP/TED), `date_limite_offres`, `procedure`, `montant`. Souvent absent des DECP → requiert le connecteur BOAMP/TED.
- **Calcul** : `delai = date_limite - date_publication` ; signal si `delai < min_legal(procedure)` (sévérité forte) ou `< percentile_strate` (faible).
- **Paramètres** : `A2.min_legal` par procédure **[À VÉRIFIER]**, `A2.percentile_strate`.
- **Explications ordinaires** : urgence réelle documentée ; procédure négociée autorisée ; erreur de saisie de date.
- **Vérifier ensuite** : l'avis complet ; la justification d'urgence dans la délibération ou la décision ; qui a obtenu le marché et depuis quand il travaille avec l'acheteur.
- **Littérature** : un délai court réduit mécaniquement le nombre de candidats et favorise ceux qui étaient informés à l'avance.

### A3 — Part élevée de marchés sans publicité ni mise en concurrence
A · moyenne · phase 1
- **Détecte** : part (en nombre et en montant) des marchés passés sans publicité ni mise en concurrence préalable, au-dessus du percentile configuré de la strate ; ou tout marché de ce type au-dessus du seuil de dispense.
- **Données** : `procedure`, `montant`.
- **Calcul** : par acheteur et période ; signal par marché si `procedure == sans_publicite` et `montant >= seuil_dispense` ; signal agrégé sur la part.
- **Paramètres** : `seuils.dispense_publicite` **[À VÉRIFIER]**, `A3.percentile_strate`.
- **Explications ordinaires** : marchés de faible montant nombreux et légitimes ; prestations artistiques ou brevetées ; marché complémentaire autorisé ; erreur de codification de la procédure dans les DECP.
- **Vérifier ensuite** : la motivation de la procédure (demande CADA) ; la récurrence du fournisseur.
- **Littérature** : le recours excessif aux procédures sans concurrence est le premier indicateur suivi par les cours des comptes.

### A4 — Recours répété à la procédure négociée sans publication
A · faible · phase 1
- **Détecte** : plusieurs marchés sur la période en procédure négociée sans publication préalable, pour le même acheteur, hors cas de faible montant.
- **Données** : `procedure`, `montant`, `date`.
- **Calcul** : compte par acheteur ; signal si `n >= A4.min` et part > percentile strate.
- **Explications ordinaires** : marchés de défense ou de sécurité ; fournisseur unique démontrable ; infructuosité préalable documentée.
- **Vérifier ensuite** : chaque motivation ; l'existence d'un appel d'offres infructueux antérieur.
- **Littérature** : procédure dérogatoire dont l'abus est documenté.

## Famille B — Seuils et fractionnement

### B1 — Montant juste sous un seuil
B · moyenne · phase 1
- **Détecte** : montant estimé ou attribué situé dans une bande étroite immédiatement sous un seuil réglementaire (dispense de publicité, publicité adaptée, seuils européens).
- **Données** : `montant`, `date`, `nature` (travaux / fournitures / services), `type_acheteur`.
- **Calcul** : pour chaque seuil applicable à la date et à la nature, signal si `seuil × (1 − bande) <= montant < seuil`. Sévérité augmente si plusieurs marchés du même acheteur sont dans la bande.
- **Paramètres** : `seuils.*` par date d'application **[À VÉRIFIER]**, `B1.bande` (ex. 5 %).
- **Confiance** : haute (calcul simple) ; basse si `montant` est un montant maximum d'accord-cadre plutôt qu'un montant attribué.
- **Explications ordinaires** : estimation sincère tombée là par hasard ; budget plafonné volontairement par l'acheteur ; montant HT/TTC confondu.
- **Vérifier ensuite** : l'estimation initiale dans la délibération ; si des avenants ont ensuite dépassé le seuil (D1) ; si d'autres marchés proches en objet existent (B2).
- **Littérature** : la concentration de montants juste sous les seuils est un indicateur classique de contournement des obligations de publicité.

### B2 — Fractionnement (saucissonnage)
B · forte · phase 1
- **Détecte** : plusieurs marchés du même acheteur, au même attributaire ou pour le même objet (même famille CPV), sur une fenêtre glissante, dont la somme franchit un seuil que chacun pris isolément ne franchit pas.
- **Données** : `acheteur`, `attributaire`, `cpv`, `montant`, `date_notification`.
- **Calcul** : fenêtre glissante `B2.fenetre_mois` ; regroupement par (attributaire) et par (famille CPV à `B2.cpv_niveau` chiffres) ; signal si `somme >= seuil` et `max(individuel) < seuil`.
- **Paramètres** : `B2.fenetre_mois` (12 par défaut), `B2.cpv_niveau`, `seuils.*`.
- **Confiance** : haute si attributaire identique ; moyenne si regroupement par CPV seulement.
- **Explications ordinaires** : besoins réellement distincts (sites différents, opérations différentes) ; accord-cadre à bons de commande mal codé en DECP comme marchés séparés ; marchés subséquents d'un accord-cadre.
- **Vérifier ensuite** : la nature exacte de chaque marché ; l'existence d'un accord-cadre parent ; la computation des seuils telle que faite par l'acheteur (demande CADA).
- **Littérature** : le fractionnement artificiel est explicitement prohibé et constitue l'un des indicateurs les plus robustes dans la littérature.

### B3 — Montants ronds répétés
B · info · phase 2
- **Détecte** : proportion de montants attribués ronds (multiples de 1 000 ou 5 000) supérieure au percentile de la strate.
- **Explications ordinaires** : montants maximums d'accords-cadres (normaux) ; forfaits.
- **Vérifier ensuite** : rien seul ; indicateur de contexte uniquement, à combiner.
- **Littérature** : signal faible, utilisé dans certains systèmes de scoring comme indice de négociation hors procédure.

## Famille C — Concentration et dépendance

### C1 — Concentration des attributaires par famille d'achat
C · moyenne · phase 1
- **Détecte** : indice de Herfindahl-Hirschman des montants par attributaire, au sein d'une famille CPV, au-dessus du percentile de la strate.
- **Données** : `attributaire`, `cpv`, `montant`.
- **Calcul** : HHI par (acheteur, famille CPV, période) ; signal si `HHI > percentile_strate` et `n_marches >= C1.min`.
- **Explications ordinaires** : marché local avec un seul prestataire crédible ; accord-cadre mono-attributaire légitime ; famille CPV très étroite.
- **Vérifier ensuite** : combien de candidats aux derniers appels d'offres de cette famille ; existence de prestataires alternatifs dans le département (SIRENE).
- **Littérature** : la concentration durable est associée à un risque de dépendance ou de relation privilégiée.

### C2 — Fournisseur dominant
C · moyenne · phase 1
- **Détecte** : un attributaire reçoit plus de `C2.part` des montants de l'acheteur sur la période, toutes familles confondues, hors délégations de service public et grands marchés de travaux identifiés.
- **Calcul** : part par attributaire ; signal si `part >= C2.part` et `montant_total >= C2.min_montant`.
- **Explications ordinaires** : un gros chantier unique ; prestataire de services généraux (énergie, assurance) ; petite commune avec peu de marchés.
- **Vérifier ensuite** : la nature des marchés ; la date de création et la taille du fournisseur (E1, E2) ; ses liens éventuels (H1).
- **Littérature** : concentration des flux financiers vers un acteur unique.

### C3 — Fidélité anormale sur un même objet
C · moyenne · phase 1
- **Détecte** : le même attributaire remporte `C3.n` renouvellements consécutifs d'un marché de même objet (même CPV, même nature, montants comparables).
- **Calcul** : chaîne de marchés similaires ordonnés dans le temps ; signal si `n_consecutifs >= C3.n`.
- **Explications ordinaires** : prestataire excellent et moins cher ; absence réelle de concurrents ; marché de maintenance lié à un équipement.
- **Vérifier ensuite** : nombre d'offres à chaque renouvellement (A1) ; évolution du prix ; si le cahier des charges a changé.
- **Littérature** : reconduction systématique sans remise en concurrence effective.

### C4 — Attributaire récurrent chez plusieurs acheteurs liés
C · faible · phase 3
- **Détecte** : le même attributaire domine chez une commune, son EPCI, son CCAS et ses satellites (SEM, syndicats) simultanément.
- **Données** : graphe institution ↔ satellites (à construire, `config/`), DECP de chacun.
- **Explications ordinaires** : groupement de commandes légitime ; prestataire régional incontournable.
- **Vérifier ensuite** : existence d'un groupement de commandes ; liens déclarés (H1).

## Famille D — Exécution et modifications

### D1 — Avenants cumulés élevés
D · moyenne · phase 1
- **Détecte** : somme des modifications d'un marché supérieure à `D1.part` du montant initial, ou franchissant un seuil que le montant initial ne franchissait pas (lien avec B1).
- **Données** : `modifications[]` (DECP : `montant_modifie`, `date_modification`), `montant_initial`.
- **Calcul** : `(montant_final − montant_initial) / montant_initial`.
- **Confiance** : moyenne (les modifications sont souvent mal renseignées dans les DECP).
- **Explications ordinaires** : aléas de chantier documentés ; révision de prix contractuelle ; prolongation Covid.
- **Vérifier ensuite** : les avenants et leurs motivations (délibérations, CADA) ; si le marché initial avait une offre unique (A1).
- **Littérature** : avenants importants = contournement possible de la mise en concurrence initiale ou offre initiale anormalement basse.

### D2 — Modification précoce
D · faible · phase 1
- **Détecte** : première modification moins de `D2.mois` après notification.
- **Explications ordinaires** : erreur matérielle corrigée ; changement de périmètre voulu par l'acheteur.
- **Vérifier ensuite** : l'objet de l'avenant.
- **Littérature** : modification rapide = cahier des charges initial inadéquat ou offre tactique.

### D3 — Durée ou prolongation atypique
D · faible · phase 2
- **Détecte** : durée du marché, ou cumul de prolongations, au-dessus du percentile de la strate pour la famille CPV, ou au-delà des durées usuelles des accords-cadres.
- **Paramètres** : `D3.duree_max_accord_cadre` **[À VÉRIFIER]**.
- **Explications ordinaires** : marchés de travaux pluriannuels ; concessions.
- **Vérifier ensuite** : la nature contractuelle ; la justification de la durée.

## Famille E — Attributaire

### E1 — Entreprise créée peu avant l'attribution
E · moyenne · phase 1
- **Détecte** : date de création de l'attributaire (SIRENE) antérieure de moins de `E1.mois` à la notification, pour un marché au-dessus de `E1.min_montant`.
- **Données** : `attributaire.siren`, SIRENE `date_creation`, `date_notification`, `montant`.
- **Explications ordinaires** : filiale ou reprise d'activité existante ; société de projet (SPV) normale pour un grand chantier ; changement de forme juridique.
- **Vérifier ensuite** : l'historique de l'entreprise (BODACC : création, cession, dirigeants) ; si elle a une activité ailleurs.
- **Littérature** : sociétés créées pour un marché = indicateur de structure ad hoc.

### E2 — Attributaire sans salarié ou en cessation
E · moyenne · phase 1
- **Détecte** : tranche d'effectif SIRENE « 0 salarié » ou « non renseignée » pour un marché de travaux ou services au-dessus de `E2.min_montant` ; ou état administratif « cessée » ou procédure collective (BODACC) à la date de notification.
- **Explications ordinaires** : entreprise individuelle réelle ; sous-traitance massive légale ; décalage de mise à jour SIRENE.
- **Vérifier ensuite** : déclaration de sous-traitance ; comptes annuels déposés.
- **Littérature** : sociétés coquilles.

### E3 — Adresse partagée entre attributaires concurrents
E · forte · phase 2
- **Détecte** : deux attributaires distincts ayant répondu aux mêmes marchés (ou alternant sur des marchés similaires du même acheteur) partagent une adresse d'établissement (hors domiciliations commerciales connues et pépinières).
- **Données** : SIRENE `adresse`, candidats (si disponibles) ou attributaires alternants.
- **Confiance** : moyenne (normalisation d'adresse).
- **Explications ordinaires** : pépinière d'entreprises ; domiciliation ; groupe avec filiales distinctes.
- **Vérifier ensuite** : dirigeants et actionnaires au RNE/BODACC ; identité des signataires des offres.
- **Littérature** : offres de couverture, entente entre soumissionnaires.

### E4 — Dirigeant commun entre attributaires alternants
E · forte · phase 2
- **Détecte** : même personne inscrite comme dirigeant (registre public des entreprises) de deux attributaires qui alternent sur les marchés d'un acheteur.
- **Contraintes éthiques** : la personne n'apparaît que par sa fonction au registre (« dirigeant commun inscrit au RNE »), jamais nommée dans le rapport par défaut ; nom disponible dans la preuve (document du registre) seulement. Voir `09-ethique-et-juridique.md`.
- **Explications ordinaires** : groupe d'entreprises transparent ; dirigeant de plusieurs sociétés dans des secteurs distincts.
- **Vérifier ensuite** : structure du groupe ; si l'acheteur en avait connaissance.
- **Littérature** : entente ; simulation de concurrence.

### E5 — Secteur d'activité incohérent avec l'objet
E · faible · phase 2
- **Détecte** : code NAF de l'attributaire sans rapport avec la famille CPV du marché (table de correspondance `config/naf_cpv.yaml`).
- **Explications ordinaires** : code NAF obsolète ; entreprise multi-activités ; mandataire d'un groupement.
- **Vérifier ensuite** : la composition du groupement ; la sous-traitance.

### E6 — Attributaire géographiquement atypique
E · info · phase 2
- **Détecte** : pour des prestations de proximité (nettoyage, espaces verts, restauration), attributaire situé à plus de `E6.km` sans établissement local.
- **Explications ordinaires** : grand groupe national ; absence d'offre locale.

## Famille F — Temporalité

### F1 — Pic d'attributions en fin de mandat
F · faible · phase 2
- **Détecte** : montant ou nombre de marchés notifiés dans les `F1.mois` précédant une élection locale supérieur au percentile de la strate pour la même fenêtre.
- **Données** : calendrier électoral `config/elections.fr.yaml`.
- **Explications ordinaires** : fin de programme pluriannuel ; calendrier budgétaire.
- **Vérifier ensuite** : nature des marchés ; délibérations correspondantes.
- **Littérature** : cycles électoraux de la dépense publique.

### F2 — Attribution en période creuse
F · info · phase 2
- **Détecte** : publication d'avis ou date limite d'offres en période de faible activité (première quinzaine d'août, dernière semaine de décembre) au-dessus du percentile de la strate.
- **Explications ordinaires** : calendrier contraint ; nombreux faux positifs.

### F3 — Procédure d'urgence
F · faible · phase 2
- **Détecte** : procédure motivée par l'urgence (si codifiée) ou délai A2 sous le minimum.
- **Vérifier ensuite** : la réalité de l'urgence.

## Famille G — Transparence et qualité des données

### G1 — Données essentielles absentes ou très lacunaires
G · moyenne · phase 1
- **Détecte** : aucun marché publié en DECP pour l'acheteur sur la période alors que la strate en publie ; ou volume publié très inférieur à ce que les dépenses d'investissement et de fonctionnement (budget) laissent attendre.
- **Données** : DECP, budget (comptes de gestion), strate.
- **Calcul** : `ratio = montant_DECP / (depenses_equipement + achats_externes)` ; signal si `ratio < percentile_bas_strate`.
- **Explications ordinaires** : marchés passés via un groupement de commandes (publiés par un autre acheteur) ; publication sur un profil d'acheteur non consolidé ; petite commune sous les seuils de publication.
- **Vérifier ensuite** : le profil d'acheteur de la commune ; les groupements de commandes auxquels elle adhère.
- **Littérature** : le non-respect des obligations de publication est en soi un constat de transparence.

### G2 — Champs obligatoires manquants
G · info · phase 1
- **Détecte** : part de marchés sans `montant`, sans `attributaire`, sans `procedure`, sans `cpv`, sans `date_notification`.
- **Rôle** : alimente la confiance de toutes les autres règles ; apparaît dans la section « périmètre et limites ».

### G3 — Retard de publication
G · faible · phase 2
- **Détecte** : délai entre notification et publication DECP supérieur au délai réglementaire **[À VÉRIFIER]**.
- **Explications ordinaires** : publication groupée annuelle ; prestataire de profil d'acheteur en retard.

### G4 — Incohérences internes des DECP
G · info · phase 1
- **Détecte** : montants négatifs ou nuls, dates incohérentes (notification avant publication d'avis, modification avant notification), SIRET invalide, doublons.
- **Rôle** : nettoyage et confiance.

## Famille H — Liens et conflits d'intérêts (sources croisées)

Règles sensibles : voir impérativement `09-ethique-et-juridique.md` avant implémentation. Elles s'appuient exclusivement sur des liens **déclarés** dans des registres publics institutionnels (HATVP, RNE, délibérations).

### H1 — Attributaire figurant dans une déclaration d'intérêts d'un élu de l'institution
H · forte · phase 3
- **Détecte** : un attributaire (SIREN) ou sa maison mère apparaît dans la déclaration d'intérêts publiée (HATVP) d'un élu exécutif de l'institution, pour la période du marché.
- **Formulation imposée dans le rapport** : « Un lien avec cet attributaire est déclaré dans une déclaration d'intérêts publique d'un membre de l'exécutif (source HATVP). La déclaration d'intérêts est précisément l'outil prévu pour permettre le déport. » Pas de nom par défaut ; preuve = document HATVP.
- **Explications ordinaires** : déport effectué conformément aux règles (vérifier la délibération) ; lien ancien ou minime.
- **Vérifier ensuite** : si l'élu s'est déporté (PV de la séance) ; nature et date du lien déclaré.
- **Littérature** : conflit d'intérêts potentiel ; la déclaration existe justement pour que le public puisse vérifier le déport.

### H2 — Subvention et marché à la même entité
H · faible · phase 3
- **Détecte** : une entité reçoit à la fois une subvention et un ou plusieurs marchés de la même institution sur la période.
- **Explications ordinaires** : très fréquent et légal (associations prestataires) ; signal de contexte.
- **Vérifier ensuite** : objet de la subvention vs objet du marché (double financement ?).

### H3 — Attributaire contrôlé par l'institution ou ses élus
H · moyenne · phase 3
- **Détecte** : l'attributaire est une SEM, SPL, association ou société dont l'institution ou ses élus sont administrateurs (sources : statuts publiés, délibérations de désignation, RNE).
- **Explications ordinaires** : quasi-régie (in house) parfaitement légale.
- **Vérifier ensuite** : le régime juridique de la relation.

## Famille I — Assemblées délibérantes (phase 2, source PV)

### I1 — Marché sans délibération ou décision retrouvée
I · moyenne · phase 2
- **Détecte** : marché au-dessus du seuil nécessitant une autorisation de l'assemblée (ou hors délégation consentie à l'exécutif) sans délibération ni décision correspondante trouvée dans les PV et listes des délibérations de la période.
- **Données** : PV et listes (extraction LLM → entités « Délibération » avec objet, montant, attributaire, vote), DECP, délégations consenties (délibération de début de mandat).
- **Confiance** : moyenne (extraction LLM ; PV incomplets).
- **Explications ordinaires** : décision prise par délégation et listée ailleurs (registre des décisions) ; délibération dans une séance non publiée en ligne.
- **Vérifier ensuite** : le registre des décisions du maire/président ; demande CADA.

### I2 — Écart entre montant délibéré et montant attribué
I · moyenne · phase 2
- **Détecte** : montant autorisé en délibération différent de plus de `I2.part` du montant DECP.
- **Explications ordinaires** : estimation vs attribution (normal si à la baisse) ; options et tranches.

### I3 — Décision hors périmètre de délégation
I · forte · phase 2
- **Détecte** : décision de l'exécutif sur un marché dont le montant dépasse le plafond de la délégation consentie par l'assemblée.
- **Données** : délibération de délégation (début de mandat), décisions.
- **Vérifier ensuite** : texte exact de la délégation ; éventuelles modifications ultérieures.

### I4 — Votes partagés récurrents sur un même attributaire
I · info · phase 2
- **Détecte** : délibérations concernant le même attributaire adoptées avec une part de votes contre ou abstentions supérieure à la médiane de l'assemblée.
- **Rôle** : contexte ; indique qu'une opposition a déjà soulevé des questions.

---

## Combinaisons

Le rapport signale explicitement les **co-occurrences** suivantes, qui augmentent la priorité d'examen (pas la sévérité individuelle) :

- A1 + C2 ou C3 (offre unique et fournisseur récurrent)
- B1 + D1 (sous le seuil puis avenant au-dessus)
- B2 + C2 (fractionnement vers un fournisseur dominant)
- E1 + C2 (entreprise récente devenue dominante)
- E3 ou E4 + A1 (concurrence apparente entre entités liées)
- H1 + n'importe quel signal de famille A ou C
- G1 + n'importe quoi (lacunes de publication autour d'un signal)

## Ajouter une règle au catalogue

1. Proposer la fiche complète dans ce document, avec littérature.
2. Implémenter `vigie/rules/<famille>/<code>.py` : fonction pure `(entities, config) -> list[Signal]`.
3. Tests : au moins un vrai positif synthétique, au moins un faux positif connu (qui ne doit **pas** déclencher ou doit déclencher à confiance basse).
4. Ajouter les paramètres dans `config/regles.fr.yaml` avec valeurs par défaut et commentaire.
5. Une règle sans section « explications ordinaires » non vide est refusée.
