# 11 — Sources de données publiques, France

Toutes les URL et modalités sont à confirmer sur la source avant écriture du connecteur **[À VÉRIFIER]** : les portails changent. Ce document est un point de départ métier, pas une documentation technique.

Principe : n'utiliser que des sources **institutionnelles** publiées en vertu d'une obligation légale ou d'une politique d'ouverture. Jamais de réseaux sociaux, de presse, de sites personnels. Respecter les licences (la plupart : Licence Ouverte 2.0 / Etalab). Respecter les conditions d'usage des API (clés, quotas) et s'identifier avec un User-Agent explicite (`vigie/<version> (+url du projet)`).

## Phase 1 — Marchés publics et identité des acteurs

### DECP — Données essentielles de la commande publique
- **Quoi** : pour chaque marché au-dessus du seuil de publication, l'acheteur publie : identifiant, objet, CPV, procédure, montant, durée, attributaires (SIRET), date de notification, modifications. Obligation prévue par le code de la commande publique.
- **Seuil de publication** : 25 000 € HT **[À VÉRIFIER]**.
- **Où** : jeu(x) consolidé(s) sur data.gouv.fr (consolidation par l'État et, historiquement, par des initiatives communautaires) **[À VÉRIFIER : nom exact du jeu, format courant DECP v2 JSON, fréquence]**.
- **Qualité connue** : hétérogène. Nombre d'offres reçues souvent absent ; modifications mal renseignées ; doublons entre profils d'acheteurs ; montants parfois maximums d'accords-cadres. D'où la famille G.
- **Clé de jointure** : SIRET acheteur, SIRET attributaire.
- **Connecteur** : `fr/decp` (phase 1, prioritaire).

### BOAMP — Bulletin officiel des annonces de marchés publics
- **Quoi** : avis de publicité (appel à la concurrence, attribution, rectificatif) avec dates de publication et de limite de remise des offres, nombre d'offres reçues dans les avis d'attribution.
- **Où** : API et jeu de données ouverts (boamp.fr / data.gouv.fr) **[À VÉRIFIER]**.
- **Usage** : règles A1 (nombre d'offres), A2 (délais), F2. Jointure avec DECP par acheteur + objet + date (jointure floue, rapprochement « probable »).
- **Connecteur** : `fr/boamp` (phase 1 bis).

### TED — Tenders Electronic Daily (UE)
- **Quoi** : avis européens au-dessus des seuils UE, dont les avis d'attribution avec nombre d'offres.
- **Où** : API TED / open data **[À VÉRIFIER]** ; schéma eForms.
- **Connecteur** : `eu/ted` (phase 1 bis ; réutilisable à l'international).

### SIRENE (INSEE) et Annuaire des entreprises
- **Quoi** : identité des entreprises et établissements : SIREN/SIRET, raison sociale, date de création, état administratif, code NAF, tranche d'effectif, adresse.
- **Où** : API Sirene (INSEE, clé requise) ou API « Recherche d'entreprises » de l'annuaire des entreprises (sans clé) **[À VÉRIFIER]** ; base complète téléchargeable sur data.gouv.fr.
- **Usage** : E1, E2, E3, E5, E6, résolution d'entités.
- **Connecteur** : `fr/sirene` (phase 1).

### BODACC
- **Quoi** : annonces légales : créations, modifications (dirigeants, siège), procédures collectives, radiations.
- **Où** : open data bodacc.fr / data.gouv.fr **[À VÉRIFIER]**.
- **Usage** : E1, E2, E4 (historique des dirigeants — données de personnes : voir doc 09).
- **Connecteur** : `fr/bodacc` (phase 2).

### RNE — Registre national des entreprises (INPI)
- **Quoi** : registre officiel : dirigeants, forme, capital, comptes annuels déposés.
- **Où** : data.inpi.fr, API avec compte **[À VÉRIFIER]**.
- **Usage** : E4, H3. Données de personnes : usage strictement limité à la fonction de dirigeant, voir doc 09.
- **Connecteur** : `fr/rne` (phase 2).

### RBE — Registre des bénéficiaires effectifs
- **Accès restreint** depuis l'arrêt de la CJUE de novembre 2022 ; en France, accès réservé aux personnes justifiant d'un intérêt légitime (dont journalistes et associations anticorruption) **[À VÉRIFIER : modalités actuelles]**.
- **Décision** : pas de connecteur dans le commons. Un utilisateur disposant d'un accès peut charger ses propres extractions localement (import manuel). Documenter le format.

## Phase 1 — Finances

### Comptes des collectivités (DGFiP)
- **Quoi** : balances comptables et comptes individuels des communes, EPCI, départements, régions : dépenses d'équipement, charges à caractère général (achats), dette.
- **Où** : data.gouv.fr (balances comptables), portail collectivites-locales.gouv.fr (comptes individuels), data.ofgl.fr (OFGL, données retraitées) **[À VÉRIFIER]**.
- **Usage** : G1 (volume attendu de marchés), contexte de strate, D1 (capacité).
- **Connecteur** : `fr/comptes-collectivites` (phase 1).

### Strates et référentiels
- **Population et code commune** : INSEE (code officiel géographique, populations légales).
- **Strates DGCL** : tranches de population utilisées dans les ratios financiers officiels. À reprendre dans `config/strates.fr.yaml` **[À VÉRIFIER]**.
- **Calendrier électoral** : élections municipales, départementales, régionales (`config/elections.fr.yaml`).
- **CPV** : vocabulaire commun des marchés publics (UE), table complète en `config/cpv.csv`.

## Phase 2 — Assemblées délibérantes

### Procès-verbaux et listes des délibérations
- **Quoi** : depuis juillet 2022, les communes publient en ligne la liste des délibérations de chaque séance et le procès-verbal après approbation ; les modalités dépendent de la taille de la commune **[À VÉRIFIER : CGCT art. L2121-15 et suivants, décret 2021-1311]**. Même logique pour EPCI, départements, régions.
- **Où** : site de chaque collectivité (pas de portail national). D'où le modèle « un connecteur = 20 lignes de YAML par commune » : URL de la page, sélecteur, format (PDF, HTML), périodicité.
- **Traitement** : OCR si nécessaire, extraction LLM → entités « Séance », « Délibération » (numéro, objet, montant, attributaire, votes, élus intéressés mentionnés, déports). Chaque extraction liée au PDF, page, et extrait textuel (preuve).
- **Usage** : I1 à I4, H1 (vérification des déports), B1/B2 (estimation initiale).
- **Connecteur** : `fr/pv-conseil` générique + fiches par commune dans `connectors/fr/communes/<code_insee>.yaml`.

### Registre des décisions prises par délégation
- Souvent non publié en ligne ; demande CADA. Modèle de demande dans `templates/cada/`.

### Recueils des actes administratifs
- Pour départements, régions, préfectures. Source des arrêtés et décisions. Phase 3.

## Phase 3 — Liens, subventions, contrôle

### HATVP — Déclarations d'intérêts
- **Quoi** : déclarations d'intérêts (et d'activités) des élus et responsables soumis : maires des communes de plus d'un seuil de population, présidents d'EPCI, conseillers départementaux et régionaux, etc. **[À VÉRIFIER : périmètre]**. Publiées en open data.
- **Usage** : H1. Uniquement pour rapprocher un **attributaire** avec un **lien déclaré**. Pas de stockage des déclarations au-delà des champs utiles ; pas d'affichage nominatif par défaut. Voir doc 09.
- **Connecteur** : `fr/hatvp` (phase 3).

### Subventions des collectivités
- **Quoi** : données essentielles des conventions de subvention (au-dessus d'un seuil, 23 000 € **[À VÉRIFIER]**), publiées sur data.gouv.fr.
- **Usage** : H2, H3 ; playbook subventions.
- **Connecteur** : `fr/subventions` (phase 3).

### Rapports des chambres régionales des comptes et de la Cour des comptes
- **Quoi** : rapports d'observations définitives sur les collectivités et établissements, en PDF, sur ccomptes.fr.
- **Usage** : contexte et « déjà examiné » : si une CRC a déjà relevé un point, le rapport Vigie le signale. Extraction LLM des observations liées aux marchés. Phase 3.

### Journal officiel — Associations
- **Quoi** : créations, dirigeants déclarés (données de personnes, usage limité), dissolutions.
- **Usage** : H3.

## International (phase 5)

- **OCDS** : Open Contracting Data Standard. Connecteur générique `ocds` acceptant n'importe quelle API ou export OCDS (releases/records). Exemples connus : Prozorro (Ukraine), Contracts Finder (Royaume-Uni), SECOP (Colombie), DNCP (Paraguay), portails de nombreuses villes. Les règles des familles A à D et G fonctionnent directement sur OCDS ; E et H dépendent de registres nationaux (OpenCorporates, OpenSanctions comme sources d'entités).
- **OpenSanctions / OpenCorporates** : résolution d'entités internationales, schéma FollowTheMoney natif.

## Sources explicitement exclues

- Réseaux sociaux, forums, presse (pas des sources primaires ; risque de reproduction d'accusations).
- Données de personnes hors capacité publique ou légale (adresses privées, patrimoine, famille).
- Toute source nécessitant de contourner un contrôle d'accès.
- Données obtenues par fuite. Vigie n'est pas un outil d'exploitation de leaks ; OpenAleph et Datashare existent pour ça et nos exports FollowTheMoney s'y chargent.
