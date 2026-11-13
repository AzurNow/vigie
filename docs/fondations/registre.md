# Hypothèses structurantes — registre

Dernière passe : [2026-11-09](2026-11-09.md). Dernier test : T1, [2026-11-13](2026-11-13-t1.md). Hors champ : règles 1, 2 et 3 de `CLAUDE.md`
(décision de l'auteur, 2026-11-09). Tenu par la compétence `hypotheses-structurantes` ; une entrée
par hypothèse, réécrite à chaque passe ; les numéros ne sont jamais réattribués. Chaque fait porte
sa force (1 calcul, 2 usage révélé, 3 littérature, 4 analogue), sa source et sa date de consultation ;
tous ont été rouverts par la session avant d'entrer ici. Aucune institution publique n'est nommée.

| # | Hypothèse (telle que la spécification la pose) | Statut | Depuis | Décision |
| --- | --- | --- | --- | --- |
| H1 | L'observation dissuade, même sans publication par le projet | infirmée | 2026-11-09 | ADR 0004 proposé |
| H2 | Le citoyen curieux, non technique, disperse le projet | infirmée (pour la v1) | 2026-11-09 | ADR 0005 proposé |
| H3 | Les DECP d'une commune suffisent à produire au moins un signal examinable | infirmée (sous ~4 000 à 5 000 hab., mesuré par T1) | 2026-11-13 | ADR 0005 proposé |
| H4 | Le modèle OpenStreetMap attire et retient des contributeurs | à tester | 2026-11-09 | ADR 0006 proposé |
| H5 | Les huit mesures de succès tirent dans le même sens | infirmée | 2026-11-09 | ADR 0004 proposé |
| H6 | Le projet atteint seul ses premiers utilisateurs ; l'adossement attend la phase 3 | infirmée | 2026-11-09 | ADR 0006 proposé |
| H7 | L'opérateur de l'outil est un humain qui choisit une institution, lance, lit, agit | à tester | 2026-11-09 | ADR 0005 proposé |
| H8 | Un protocole ouvert suffit pour que des tiers construisent les plateformes | infirmée (comme condition suffisante) | 2026-11-09 | ADR 0006 proposé |
| H9 | Verrouiller les sept règles avant la réalité protège plus qu'il ne fige | pari assumé | 2026-11-09 | ADR 0007 proposé |
| H10 | Déterministe d'abord ; le LLM sert à trois choses seulement | confirmée (périmètre de la règle 4) ; pari assumé (ordre des phases) | 2026-11-09 | aucune |

## Utilisateurs visés : comment chacun est observé

| Utilisateur (`docs/01`) | Observé par | Biais |
| --- | --- | --- |
| citoyen curieux | pas du tout, sauf une trace indirecte (un juriste « assez nul en informatique » demandant un outil de filtrage clé en main, discussions data.gouv.fr) | celui qui ne publie rien ne laisse rien ; son absence n'est pas une absence de besoin |
| journaliste local | analogie (capacité data de la PQR, outils de veille de PV en rédaction aux États-Unis) ; aucune trace française d'usage des DECP | une enquête fondée sur les DECP peut exister sans que le mot apparaisse |
| association | traces directes (rapport d'impact et proposition PGO de TI France ; note PGO d'Anticor) | acteurs qui publient leur bilan |
| contributeur technique | traces directes (réutilisations déclarées, dépôts, discussions data.gouv.fr) | registre déclaratif ; usages internes invisibles |
| institution | pas du tout (aucune trace d'un service achats utilisant un outil de ce type) | — |
| opérateur agent (H7) | analogie (dispositifs automatisés étrangers) | fiches de promoteurs, sans groupe témoin |

## Entrées

### H1. L'observation dissuade, même sans publication par le projet

- **La spécification dit** : « Si l'observation devient universelle, permanente et crédible,
  l'équilibre change : chaque acheteur public, chaque conseil, chaque établissement se comporte
  comme s'il avait un reporter compétent assigné à vie. » (`docs/01`, La thèse) ; « Vigie ne publie
  rien. » (`docs/01`, Ce que Vigie n'est pas) ; « on ne discute pas d'institutions nommées dans les
  espaces du projet » (`docs/10` §3).
- **Mécanisme supposé** : un acheteur qui se sait observé change de comportement. Le chemin par
  lequel il l'apprend n'est écrit nulle part ; la littérature invoquée (« ce qui se passe quand un
  journal local ferme ») porte sur une observation publiée.
- **Ce qui la falsifierait** : aucun effet documenté d'une observation non notifiée ; tous les effets
  documentés passent par une notification ou une publication.
- **Ce qui tomberait avec elle** : la thèse d'impact telle qu'écrite ; la mesure « Indicateur de
  dissuasion à long terme ».
- **Statut** : infirmée — 2026-11-09, première passe.
- **Faits** :
  - [3] Audits municipaux tirés au sort au Brésil (Avis, Ferraz, Finan) : −7,9 % d'actes de
    corruption dans les communes auditées, ~72 % de l'effet attribué au canal judiciaire ; les
    communes voisines **non auditées** réduisent aussi (−7,5 % avec radio locale, −10,4 % avec
    télévision locale) et « aucun effet similaire » sans média local. — J-PAL,
    https://www.povertyactionlab.org/print/pdf/node/2327?lang=es, consulté le 2026-11-09. Analogie [4]
    : l'auditeur est une autorité avec pouvoir de sanction ; la contagion exige un relais médiatique.
  - [3] Expérience randomisée au Chili (Engel, Jordán, Rau, Repetto, 2023, ~200 agences) : une
    **lettre** de l'institution supérieure de contrôle annonçant un possible audit réduit de 33 % les
    achats de fin d'année. — https://repositorio-az.uc.cl/dspace/items/54300445-a18b-4f2d-832d-aa55f1d07d8c/full,
    consulté le 2026-11-09. Seul effet d'observation non publiée trouvé : l'institution est
    **explicitement notifiée**, par une autorité.
  - [3] Revue OGP/OCP 2022 (> 60 travaux, anglais seulement) : les effets sont attribués à des
    réformes groupées ; la menace d'audit externe et le suivi par des tiers ont un effet positif ;
    les portails doivent être « combinés à une motivation et une capacité suffisantes de la société
    civile à agir » ; « peu d'analyse des conditions nécessaires ou suffisantes qui permettent
    l'impact ». — https://www.opengovpartnership.org/wp-content/uploads/2022/05/State-of-the-Evidence-Open-Contracting.pdf,
    consulté le 2026-11-09.
  - [3] DIGIWHIST (3,5 M de marchés TED 2006-2015) : « publier l'information dans l'appel d'offres a
    un effet significatif sur les risques de corruption, la publier après l'attribution n'en a pas ».
    — https://www.open-contracting.org/2017/12/06/greater-transparency-calls-tenders-save-europe-billions/,
    consulté le 2026-11-09. Les DECP sont des données d'attribution.
  - [3] Learning review des pactes d'intégrité (2015) : « peu d'études d'impact », « preuves
    largement anecdotiques », « la plupart des pactes n'ont pas mesuré la perception avant ou
    après » ; la visibilité, « obtenue d'abord par la presse », est un moteur important du changement.
    — https://baselgovernance.org/sites/default/files/2020-04/2015_IntegrityPactsProcurement_LearningReview_EN_v2-3.pdf,
    consulté le 2026-11-09.
  - [2] Red Flags (Hongrie, deux ONG, ~130 k€) : des cas repris par les médias, « aucune enquête », le
    gouvernement n'a rien changé. — https://antifraud-knowledge-centre.ec.europa.eu/library-good-practices-and-case-studies/good-practices/early-warning-red-flags_en,
    consulté le 2026-11-09.
  - [2] DoZorro (Ukraine) : ~41 000 marchés à risque traités, ~37 000 **saisines** adressées aux
    autorités de contrôle et aux acheteurs, > 5 800 marchés « corrigés » (chiffres du porteur). —
    https://ti-ukraine.org/en/project/analytics-for-procurement/, consulté le 2026-11-09. L'acheteur
    apprend par la saisine.
- **Argument contraire** : la dissuasion est déclarée « à long terme (recherche externe) », le
  projet ne prétend pas la produire seul ; et l'engagement 4 fait des lecteurs le canal (un
  journaliste publie, un élu interroge, une demande CADA notifie). Réponse : ce canal n'est écrit
  nulle part comme *le* mécanisme, rien ne le mesure, et « universelle » suppose une couverture que
  des lecteurs volontaires ne donnent pas (H7). Le statut tient : l'hypothèse telle qu'écrite —
  l'observation suffit — est contredite ; sa version tenable — l'action notifiée dissuade — n'est
  pas écrite.
- **Test ou pilote** : pilote B (un rapport, une personne), passe du 2026-11-09.
- **Alternative** (recommandée) : écrire le mécanisme dans `docs/01` — la dissuasion passe par les
  actions des lecteurs (question en assemblée, demande CADA, article, saisine), qui notifient
  l'institution — et faire de « actions notifiées » la mesure de résultat (H5). Sert : « questions
  posées en assemblée délibérante, saisines », « reprises par des médias ». Impact : donne au projet
  une cible mesurable et oriente la phase 1 vers l'outillage de l'action (modèles, destinataires,
  suivi). Coût : texte seul, plus une fonction légère de suivi des actions dans le workspace local.
- **Décision** : ADR 0004 proposé.

### H2. Le citoyen curieux, non technique, disperse le projet

- **La spécification dit** : « Le citoyen curieux […] Pas technique. […] C'est lui qui disperse le
  projet. » (`docs/01`, Utilisateurs cibles) ; « Objectif v1.1 : installateur graphique minimal […],
  parce que le citoyen curieux n'ouvre pas de terminal. » (`docs/07` §13).
- **Mécanisme supposé** : des non-techniciens installent une CLI, l'exécutent et partagent avant la
  v1.1.
- **Ce qui la falsifierait** : aucun usage non technique des données ni des outils comparables ; les
  usages visibles sont techniques ou commerciaux.
- **Ce qui tomberait avec elle** : l'ordre des utilisateurs cibles pour la v1, l'ordre des livrables
  de la phase 1 (`docs/08`), la promesse « une commande » du `README`.
- **Statut** : infirmée pour la v1 — 2026-11-09, première passe.
- **Faits** :
  - [2] Les réutilisations déclarées du jeu DECP consolidé officiel sur data.gouv.fr (37 annoncées ; 6
    vues par la session, 18 par le sous-agent) sont des services de veille commerciale ou de
    prospection ; aucun média, aucune association, aucun chercheur parmi celles vues. —
    https://www.data.gouv.fr/datasets/donnees-essentielles-de-la-commande-publique-fichiers-consolides/reuses_and_dataservices,
    consulté le 2026-11-09.
  - [2] Dans les discussions du jeu consolidé communautaire, un juriste « assez nul en informatique »
    demande une solution clé en main. —
    https://www.data.gouv.fr/datasets/donnees-essentielles-de-la-commande-publique-consolidees-format-tabulaire/discussions,
    consulté le 2026-11-09.
  - [2] TI France (rapport d'impact 2024 : dix salariés, 112 adhérents, déficit de 46 958 €, 70 % de
    l'effectif renouvelé) : aucune mention des DECP ni d'analyse de marchés locaux ; ses outils
    portent sur la HATVP et une carte d'affaires « relatées dans la presse ». —
    https://transparency-france.org/wp-content/uploads/2025/09/Rapport-dimpact-2024_compressed.pdf,
    consulté le 2026-11-09. Sa proposition PGO 2024 demande un répertoire centralisé des DECP et cite
    des outils étrangers comme modèles, sans décrire d'usage propre. —
    https://transparency-france.org/wp-content/uploads/2024/03/Proposition-commande-publique.pdf,
    consulté le 2026-11-09.
  - [2] Presse régionale (une dizaine d'entretiens, 2022) : pôles data chez les grands groupes
    seulement, « une ou quelques personnes » ailleurs, petites rédactions « contraintes par le temps
    à revoir leurs ambitions à la baisse » ; collectif Data+local de 13 quotidiens ; aucune enquête
    sur les marchés publics citée. — https://ijnet.org/es/node/12279, consulté le 2026-11-09.
  - [2] Le plan de lancement de la phase 1 vise des communautés techniques et journalistiques (GIJN,
    Data for Good, Hacks/Hackers, OpenAleph) — `docs/08`, phase 1 : la spécification elle-même
    n'attend pas le citoyen en premier.
- **Argument contraire** : l'ordre de `docs/01` dit qui *compte*, pas qui vient en premier, et la
  v1.1 est prévue. Réponse : les exigences de la phase 1 (« zéro jargon », « mode voisin », quinze
  minutes) sont écrites pour un lecteur que la spécification sait absent avant la v1.1, pendant que
  les lecteurs qui viendront (journaliste-data, association, contributeur) ont des besoins
  identifiés (exports, lot, delta, preuves ouvrables) placés au même rang. La réponse de la défense
  renforce le constat au lieu de le lever.
- **Test ou pilote** : pilote A (audit à la main) et pilote B.
- **Alternative** (recommandée) : pour la v1, mettre en premier le **journaliste-data et
  l'association** (et le contributeur technique), le citoyen non technique devenant la cible de la
  v1.1 avec l'installateur. Sert : « rapports partagés publiquement », « reprises par des médias ».
  Impact : la phase 1 outille ceux qui agiront réellement ; le citoyen hérite d'un outil déjà
  éprouvé. Coût : réordonner `docs/01` « Utilisateurs cibles » et les exigences de `docs/02` §2 ;
  texte seul.
- **Décision** : ADR 0005 proposé.

### H3. Les DECP d'une commune suffisent à produire au moins un signal examinable

- **La spécification dit** : « Marchés publics, France, communes. Parce que : […] échelon communal =
  35 000 institutions, le plus proche des citoyens et le moins couvert par la presse. » (`docs/01`,
  Tête de pont) ; qualité des DECP « hétérogène. Nombre d'offres reçues souvent absent ;
  modifications mal renseignées ; doublons entre profils d'acheteurs » (`docs/11`).
- **Mécanisme supposé** : pour la commune que l'utilisateur choisit, les DECP contiennent assez de
  marchés, assez renseignés, pour qu'au moins une règle de phase 1 produise un signal.
- **Ce qui la falsifierait** : la commune médiane a trop peu de marchés publiés sur 36 mois pour
  qu'une règle par acheteur ait de la matière, ou les champs requis manquent structurellement.
- **Ce qui tomberait avec elle** : la tête de pont (communes de toutes tailles), le premier persona,
  le playbook de phase 1 tel qu'il promet.
- **Statut** : infirmée pour les communes de moins d'environ 4 000 à 5 000 habitants — 2026-11-13,
  mesurée par le test T1 (2026-11-09, première passe : infirmée sous ~3 500 habitants, sur
  estimation).
- **Faits** :
  - [1] Dénominateur : 34 875 communes au 1er janvier 2025, dont 18 267 de moins de 500 habitants
    (52 %), 29 397 de moins de 2 000 (84 %), 3 228 de 3 500 et plus (9,3 %), 1 039 de 10 000 et plus
    (3,0 %) — calcul de la passe sur les 26 tranches de la DGCL (script en annexe de la passe). —
    DGCL, « Les collectivités locales en chiffres 2025 », fiche 2-3,
    https://www.collectivites-locales.gouv.fr/files/files/Etudes-et-statistiques/Les%20collectivit%C3%A9s%20locales%20en%20chiffres/Les%20collectivit%C3%A9s%20en%20chiffres/2025/BATweb-CL_en_chiffres_2025.pdf,
    consulté le 2026-11-09.
  - [2] Part des dépenses communales d'équipement 2023 (hors capitale) par strate : < 500 hab. 6,8 %
    (18 126 communes) ; 500–2 000 : 16,1 % (11 275) ; 2 000–3 500 : 9,1 % ; 3 500–10 000 : 19,8 % ;
    10 000–30 000 : 19,2 % ; 30 000–100 000 : 19,6 % ; > 100 000 : 9,4 % (41). Les communes de moins
    de 2 000 habitants (84 %) portent 23 % de l'équipement. — La Banque Postale / AMF,
    https://www.lagazettedescommunes.com/wp-content/uploads/2024/11/cdm24etudelbpamf.pdf, consulté le
    2026-11-09.
  - [2] Recensement OECP 2024 : 223 383 marchés, dont 159 435 pour le secteur public local
    (100,7 Md€) ; seuil de recensement abaissé de 90 000 à 40 000 € HT, donc série non comparable aux
    années précédentes. — https://www.seban-associes.avocat.fr/panorama-de-la-commande-publique-les-chiffres-de-2024/
    (16 avril 2026) et https://acteurspublics.fr/articles/commande-publique-les-marches-recenses-bondissent-a-233-milliards-deuros-en-2024-la-commande-publique-franchit-la-barre-des-230-milliards-deuros-grace-a-une-meilleure-collecte-des-donnees/
    (26 novembre 2025), consultés le 2026-11-09. Les communes pèsent 49 % des montants du bloc
    local en 2023 (21 920 M€ sur 44 930). — Baromètre Intercommunalités de France / Banque des
    Territoires, https://www.intercommunalites.fr/app/uploads/2024/10/V3_Barometre-commande-publique-2023_2024.pdf,
    consulté le 2026-11-09.
  - [1, mesuré] Marchés publiés en 36 mois (2023-11 → 2026-10) par commune, consolidé communautaire
    du 2026-11-13 : part des communes ayant au moins 10 marchés — < 500 hab. : 1,0 % ; 500–2 000 :
    6,8 % ; 2 000–3 500 : 26,0 % ; 3 500–10 000 : 60,2 % ; 10 000–30 000 : 90,3 % ; au-delà : 94 à
    98 %. Moyennes : 0,5 ; 2,2 ; 7,4 ; 24,5 ; 94 ; 227 ; 885. La moitié des communes atteint 10
    marchés vers 4 500 habitants (4 000 à 5 500 selon la variante : dédoublonnage inter-sources, seuil
    de 40 000 €, fenêtre mûre, nombre d'offres renseigné). Parmi les communes de 500 à 3 500
    habitants, 8 à 11 % ont au moins 10 marchés. EPCI : 79 % en ont au moins 10 (médiane 48) ;
    départements : 95 % (médiane 485). L'estimation de la passe du 2026-11-09 était juste sous 3 500
    habitants et sous-estimait les grandes communes d'un facteur 1,5 à 3. — Test T1,
    [2026-11-13](2026-11-13-t1.md), scripts et empreintes des entrées en annexe.
  - [1, mesuré] Qualité, marchés communaux de la même fenêtre (270 385) : nombre d'offres renseigné
    pour 78,5 % (84 à 87 % des marchés notifiés en 2024 et 2025, 17 à 27 % de 2020 à 2023) ; une
    seule offre pour 24,8 % de ceux où il l'est ; procédure 98,6 % ; titulaire 94,6 % ; doublons
    probables entre sources 11,3 % ; montant < 40 000 € 18,7 %. — Test T1,
    [2026-11-13](2026-11-13-t1.md).
  - [2] Couverture des acheteurs : « les données de seulement 7 700 acheteurs (sur un total de
    70 000) sont récupérées par le Ministère des finances » (mai 2021). —
    https://teamopendata.org/t/decp-info-les-donnees-de-la-commande-publique-pour-tous-questions-reponses-discussions/2948,
    consulté le 2026-11-09. « 12 226 communes qui publient des marchés » dans le consolidé
    communautaire, toutes années (réutilisation Qipu, octobre 2026), soit 35 % des communes [1]. —
    https://www.data.gouv.fr/reuses/les-marches-publics-de-chaque-commune, consulté le 2026-11-09.
    Le producteur du consolidé : « il n'existe pas de référentiel à partir duquel nous pourrions
    comparer l'exhaustivité des DECP ». —
    https://www.data.gouv.fr/datasets/donnees-essentielles-de-la-commande-publique-consolidees-format-tabulaire/discussions,
    consulté le 2026-11-09.
  - [2] Le « nombre d'offres reçues » **n'existe pas** dans l'arrêté du 22 mars 2019 (16 données par
    marché) et n'apparaît qu'au 18° de l'arrêté du 22 décembre 2022, en vigueur au 1er janvier 2024 :
    la règle A1 est inapplicable aux marchés notifiés avant 2024 et ne dispose que d'environ 22 mois
    de données. — https://www.marche-public.fr/Marches-publics/Textes/Arretes/Arrete_2019_03_22_donnees-essentielles-ECOM1831542A.htm
    et https://www.marche-public.fr/Marches-publics/Textes/Arretes/Arrete_2022_12_22_donnees-essentielles-mp-ECOM2235715A.htm,
    consultés le 2026-11-09 (Légifrance inaccessible depuis l'environnement).
  - [2] Seuils : publication des données essentielles à partir de 40 000 € HT dans les deux mois ;
    entre 25 000 et 40 000 € HT, une liste annuelle (objet, montant, date, attributaire) suffit, sans
    procédure ni nombre d'offres. — R2196-1, version du 1er janvier 2024,
    https://www.marche-public.fr/ccp/R2196-01-code-commande-publique.htm, consulté le 2026-11-09. Le
    seuil de dispense de mise en concurrence est passé à 60 000 € au 1er avril 2026, les seuils de
    déclaration sont maintenus et « il n'existe à ce jour pas de sanction en cas de non-déclaration ».
    — https://acteurspublics.fr/articles/bercy-clarifie-les-obligations-de-declarations-de-marches-publics-suite-a-la-hausse-des-seuils-marches-publics-bercy-preserve-la-transparence-malgre-le-relevement-des-seuils/
    (7 avril 2026), consulté le 2026-11-09.
  - [2] Le consolidé ministériel vu par le registre OCP : 519 020 marchés, **0 tenders, 0
    amendments**, données jusqu'en février 2025 ; fichiers annuels de 12 à 20 Mo pour 2019-2022, 2 Mo
    en 2023, < 1 Mo pour 2024, 2025 et 2026. — https://data.open-contracting.org/en/publication/23,
    consulté le 2026-11-09. La source officielle « s'est presque tarie depuis le 1er janvier 2024 »
    (producteur du consolidé communautaire, janvier 2025 ; reprise quotidienne en janvier 2026). Toute
    fenêtre de 36 mois à cheval sur 2024 mesure la transition de format autant que les acheteurs.
  - [1, mesuré] Le tarissement ne vaut pas pour le consolidé communautaire : 2024 (365 357 marchés) et
    2025 (354 525) y sont les années les plus fournies, portées par le flux comptable de la DGFiP
    (`pes_marche_2024`), qui fournit 47 % des marchés communaux récents et environ 68 % de ceux des
    communes de moins de 3 500 habitants. Sa qualité propre n'est pas mesurée. — Test T1,
    [2026-11-13](2026-11-13-t1.md).
- **Argument contraire** : une DECP vide n'est pas rien : G1 en fait un signal (`docs/02` §6).
  Réponse : G1 exige le connecteur budget (phase 1), ne distingue pas la sous-publication de la
  faible activité, et un rapport dont le seul signal est « rien n'est publié » offre une action mince
  ; sous 40 000 € HT l'absence est légale. Le statut tient pour les petites communes ; T1 l'a mesuré
  (2026-11-13).
- **Test ou pilote** : test T1 **exécuté le 2026-11-13** ([compte rendu](2026-11-13-t1.md)) : le
  critère de falsification de l'alternative (plus de la moitié des communes de 500 à 3 500 habitants
  avec ≥ 10 marchés sur 36 mois) n'est pas atteint — 8 à 11 %. Reste le pilote A (audit à la main), exécuté par l'agent dès qu'une session a
  le réseau (voir « Pilote A » en fin de registre) : T1 donne la distribution, le pilote A donne ce
  qui sort réellement pour une institution.
- **Alternative** (recommandée) : tête de pont de la phase 1 = **communes de 3 500 habitants et
  plus, EPCI et départements**, en lot, avec le playbook qui déclare dès l'en-tête ce qu'il ne peut
  pas dire pour une petite commune. Sert : « nombre d'institutions ayant fait l'objet d'au moins un
  audit » (vrais audits, pas des rapports vides), « pistes confirmées ». Impact : concentre la phase 1
  sur les 9 % de communes qui portent 68 % de l'équipement et sur des institutions moins visibles
  que les communes. T1 place la bascule plutôt vers 4 000 à 5 000 habitants (60 % des communes de
  3 500 à 10 000 habitants ont au moins 10 marchés, 41,5 % dans la tranche 3 500–4 000) : la borne de
  3 500, tranche de la DGCL, reste défendable, mais le chiffre exact revient à l'auteur dans l'ADR
  0005. Coût : une ligne de `docs/01`, les strates de `config/strates.fr.yaml`, l'en-tête
  du playbook ; texte seul.
- **Décision** : ADR 0005 proposé.

### H4. Le modèle OpenStreetMap attire et retient des contributeurs

- **La spécification dit** : « Le modèle est OpenStreetMap : personne ne cartographie le monde,
  chacun cartographie son quartier. » (`docs/01`) ; « pas de vue agrégée nationale centralisée (les
  résumés de strates publiés dans le dépôt en tiennent lieu) » (ADR 0001) ; « un contributeur externe
  a fait accepter une règle ou un connecteur. » (`docs/08`, phase 1).
- **Mécanisme supposé** : des contributeurs viennent et restent parce que leur contribution a une
  valeur visible pour d'autres.
- **Ce qui la falsifierait** : les communs de connecteurs par territoire sans artefact partagé ni
  organisation porteuse ne dépassent pas quelques contributeurs.
- **Ce qui tomberait avec elle** : le modèle de croissance ; le critère « terminé » de la phase 1 ;
  l'absence de vue agrégée (ADR 0001).
- **Statut** : à tester — 2026-11-09, première passe.
- **Faits** :
  - [3] OpenStreetMap, ~1 300 messages de validation analysés : un retour envoyé dans les 28 heures
    (délai médian) réduit le risque d'abandon des nouveaux à 80 % ; « chaque jour de retard
    supplémentaire augmentait le taux d'abandon ». — https://www.openstreetmap.org/user/dekstop/diary/40421,
    consulté le 2026-11-09. Le facteur documenté est la réaction humaine rapide au premier geste.
  - [2] EveryPolitician (mySociety) : « des centaines de scrapers tournent sur morph.io »,
    « contributions de dizaines d'autres organisations et individus », 233 pays ; **mis en pause** en
    juin 2019 : « nous n'avons tout simplement pas le financement », « chaque parlement est un cas
    particulier », « trop pour une organisation sous-financée », et, dès 2018, ne pas savoir qui
    utilise les données « rend difficile de prouver que le projet a un impact ». —
    https://www.mysociety.org/category/archived/everypolitician/, consulté le 2026-11-09. Analogue [4]
    le plus proche d'un commun de connecteurs par territoire : il a eu des dizaines de contributeurs
    parce qu'une organisation le portait, et s'est arrêté avec elle.
  - [2] OpenAddresses : dépôt de références de sources par territoire, 3,3 k étoiles, 884 forks,
    15 156 commits, 985 issues, financement Open Collective, et un pipeline central (« weekly batch
    run ») qui exécute les sources. — https://github.com/openaddresses/openaddresses, consulté le
    2026-11-09. Analogue [4] : il casse parce que le produit (des adresses) a une demande économique
    massive et qu'un artefact partagé — le fichier mondial — existe, même sans carte.
  - [3] Open North (trois standards ouverts) : l'adoption suit la demande et la simplicité ; les
    standards étaient « hautement personnalisés (portés par 1-2 individus) » et « les limites de
    financement » ont arrêté leur développement. —
    https://opennorth.ca/wp-content/uploads/legacy/OpenNorth_standards_report__1_.pdf, consulté le
    2026-11-09.
- **Argument contraire** : le commun a des artefacts partagés (catalogue, connecteurs, résumés de
  strates). Réponse : une fiche de commune n'a de valeur visible que pour les utilisateurs de cette
  commune ; rien ne montre à un contributeur que « son quartier est vide ». Aucun fait ne tranche
  encore : statut « à tester ».
- **Test ou pilote** : pilote C (une fiche, un contributeur).
- **Alternative** (recommandée, si le pilote falsifie) : un **artefact partagé visible** sans serveur
  — une carte de couverture des fiches et des connecteurs (quelles communes ont une fiche, à quelle
  date), générée dans le dépôt, plus les résumés de strates déjà prévus — et un engagement écrit de
  réponse à toute première contribution sous 48 heures. Sert : « contributeurs actifs sur 90 jours ».
  Coût : un script de génération et une page statique ; une règle de `docs/10` §3.
- **Décision** : ADR 0006 proposé (partie « artefact partagé »).

### H5. Les huit mesures de succès tirent dans le même sens

- **La spécification dit** : « Le succès n'est pas le nombre de signaux. C'est : » suivi de huit
  mesures (`docs/01`, Mesures de succès).
- **Mécanisme supposé** : optimiser l'une ne dégrade pas les autres ; aucune ne manque.
- **Ce qui la falsifierait** : une mesure d'activité peut croître sans la mesure de résultat ; une
  mesure manque (le dommage causé).
- **Ce qui tomberait avec elle** : la phase 1 (on optimise ce qu'on mesure) ; le critère
  d'acceptation v1 (`docs/02` §10).
- **Statut** : infirmée — 2026-11-09, première passe.
- **Faits** :
  - [1, lecture] Six des huit mesures comptent une activité (institutions auditées, territoires,
    contributeurs, rapports partagés, taux de faux positifs déclarés, reprises) ; une seule compte un
    résultat sur l'institution (dissuasion, « recherche externe ») ; une compte un jugement humain
    (pistes confirmées). Aucune ne compte le dommage : une publication fausse par un utilisateur.
  - [2] Post-mortem d'Open Contracts (Slovaquie, 2012) : « aucune “étape finale” proposée sur le
    site », d'où l'impossibilité de « mesurer les interventions déclenchées par le site » ; leçon :
    « avoir des cas d'usage par groupe cible et des actions que vous voulez qu'ils accomplissent, et
    les mesurer ». — https://sunlightfoundation.com/2012/11/07/online-contracts-monitoring-first-year-lessons-learned/,
    consulté le 2026-11-09.
  - [2] EveryPolitician : ne pas savoir qui utilise les données « rend difficile de prouver que le
    projet a un impact » (2018). — https://www.mysociety.org/category/archived/everypolitician/,
    consulté le 2026-11-09.
- **Argument contraire** : la liste refuse déjà le nombre de signaux, et les retours volontaires
  mesurent un résultat. Réponse : aucune mesure n'est désignée comme celle qu'on optimise, et le
  dommage n'est pas compté alors que `docs/01` dit qu'une fausse accusation détruit le projet.
- **Test ou pilote** : pilote B (quelle action, en combien de temps).
- **Alternative** (recommandée) : une étoile polaire — **actions notifiées à une institution**
  (question en assemblée, demande CADA, article, saisine, auto-correction déclarée) par rapport
  produit — et une mesure de dommage (publications contestées ou retirées). Sert : « questions posées
  en assemblée délibérante, saisines », « reprises par des médias ». Coût : texte seul, et un suivi
  local optionnel des actions dans le workspace.
- **Décision** : ADR 0004 proposé.

### H6. Le projet atteint seul ses premiers utilisateurs ; l'adossement attend la phase 3

- **La spécification dit** : « Un particulier, sans organisation, veut l'impact le plus large
  possible. » (ADR 0001) ; « Phase solo (phases 0-1) : un mainteneur. » et « Hébergement
  institutionnel (phase 3+) : adossement à une structure à but non lucratif existante » (`docs/10`
  §2).
- **Mécanisme supposé** : un mainteneur seul produit la phase 1 et le lancement, et obtient les
  premiers utilisateurs par les communautés listées.
- **Ce qui la falsifierait** : les projets comparables à impact documenté étaient adossés ; aucun
  projet porté par un particulier seul n'a d'impact documenté.
- **Ce qui tomberait avec elle** : le moment de l'adossement (`docs/10` §2), la stratégie de
  lancement (`docs/08`, phase 1), et par ricochet H1, H4, H8.
- **Statut** : infirmée — 2026-11-09, première passe.
- **Faits** :
  - [2] Civio (Espagne) : fondée en 2012 par deux personnes, aujourd'hui dix salariés, plus de 2 000
    adhérents fournissant 35-40 % du budget, huit contentieux, enquêtes « à impact réel » ; « ils
    passent des semaines à nettoyer » les données régionales. —
    https://gijn.org/stories/member-profile-civio-data-journalism-pioneer-spain/, consulté le
    2026-11-09. L'impact est venu après la structuration et par le journalisme.
  - [2] Red Flags (Hongrie) : adossé à deux ONG, financé (~130 k€), ~20 consultations par jour pour
    700-800 inscrits ; aucune enquête ouverte. — fiche de la Commission européenne citée en H1.
  - [2] Regards Citoyens, association « restée entièrement bénévole depuis sa création en 2009 » :
    en juillet 2022, « il s'agira de la dernière législature pour l'équipe actuelle », forces
    bénévoles « peu renouvelées et même épuisées », appel à reprise. —
    https://next.ink/brief-article/xvi%e1%b5%89-legislature-sur-nosdeputes-fr-regards-citoyens-lance-un-appel-pour-reprendre-la-plateforme/
    (8 juillet 2022), consulté le 2026-11-09.
  - [2] TI France : dix salariés, 112 adhérents, déficit, 70 % de l'effectif renouvelé en 2024, pas
    de capacité d'analyse de marchés locaux (H2). Anticor (note PGO, février 2024) : travaille avec
    l'ENS Rennes « sur un projet visant à mobiliser les données numériques afin d'évaluer les
    conditions de passation de marchés publics » ; « seul un petit nombre publie des informations en
    open data ». — https://www.anticor.org/wp-content/uploads/2024/02/PGO-Association-Anticor.pdf,
    consulté le 2026-11-09.
- **Argument contraire** : l'ADR 0001 a choisi le protocole précisément parce qu'un particulier ne
  porte pas une plateforme ; l'adossement tardif évite la capture. Réponse : l'adossement n'est pas
  une plateforme, et aucun des trois chemins dont dépend l'impact — visibilité (H1), premiers lecteurs
  (H2), contributeurs (H4) — n'a de trace de réussite sans organisation. L'anti-capture de `docs/10`
  §2 et §4 s'applique à un partenaire comme à un financeur.
- **Test ou pilote** : pilote B (la personne qui reçoit le rapport peut être le premier partenaire).
- **Alternative** (recommandée) : chercher **dès la phase 1** un partenaire opérationnel — une
  structure qui analyse déjà, ou veut analyser, des marchés locaux (le projet d'Anticor avec l'ENS
  Rennes, dont une « charte du Datalab » de mai 2025 est annoncée par le titre d'un document de ses
  archives, non lu ; le collectif Data+local ; un pôle data de presse régionale) — comme premier
  utilisateur,
  canal de publication et relais de contributeurs, sous les conditions anti-capture de `docs/10`.
  Sert : « rapports partagés publiquement », « reprises par des médias », « contributeurs actifs ».
  Coût : des conversations, pas du code ; une section de `docs/10` §2.
- **Décision** : ADR 0006 proposé.

### H7. L'opérateur de l'outil est un humain qui choisit une institution, lance, lit, agit

- **La spécification dit** (implicitement) : « Les grands modèles de langage sont la première
  technologie qui rend la lecture compétente de ces données quasi gratuite. » (`docs/01`) ;
  « N'importe quel agent capable d'appeler des outils et de produire du JSON validé peut exécuter un
  playbook » (`docs/04`) ; « Il n'y a pas de mode « sans humain » pour la livraison d'un rapport ; il
  y en a un pour la collecte et le calcul (`--batch`) » (`docs/02` §5) ; « Un agent autonome qui
  enquête seul. Le playbook contient des points de contrôle humains obligatoires. » (`docs/01`).
- **Mécanisme supposé** : la couverture « universelle, permanente » vient de la multiplication
  d'opérateurs humains qui auditent chacun leur territoire.
- **Ce qui la falsifierait** : la veille automatisée couvre un territoire à coût marginal nul et
  produit des actions humaines ; la couverture par volontaires reste marginale.
- **Ce qui tomberait avec elle** : l'ordre des utilisateurs (association et journaliste d'abord), la
  forme de la v1 (lot et delta plutôt qu'« une commande » par commune), le coût de la couverture que
  H1 suppose.
- **Statut** : à tester — 2026-11-09, première passe.
- **Faits** :
  - [2] DoZorro : des alertes automatiques et une communauté d'experts, ~37 000 saisines formelles, un
    tiers des cas examinés « résolus » (chiffre du porteur). — https://ti-ukraine.org/en/project/analytics-for-procurement/
    et https://oecd-opsi.org/?p=9366, consultés le 2026-11-09. L'automatisation produit des actions
    quand un étage humain et un canal de saisine existent.
  - [2] Red Flags (Hongrie) : analyse automatisée quotidienne, 20 consultations par jour, aucune
    enquête (fiche citée en H1). L'automatisation sans étage d'action ne produit rien de mesurable.
  - [2] Rédactions américaines : LocalLens suit ~80 conseils scolaires dans 30 États, Civic Sunlight
    un territoire de 43 communes ; « c'est un tuyau, il faut le vérifier » ; « une poignée »
    d'articles ; transcriptions « loin d'être soignées ». —
    https://niemanlab.org/2025/03/local-newsrooms-are-using-ai-to-listen-in-on-public-meetings,
    consulté le 2026-11-09.
  - [2] Open Contracts (Slovaquie) : sans « étape finale », pas d'action mesurable (citée en H5).
- **Argument contraire** : `--batch` et le persona association existent déjà ; rien n'interdit un
  opérateur agent. Réponse : la forme de la v1 (« une commande » pour « votre commune »), l'ordre des
  personas et la mesure du temps (« quinze minutes ») sont écrits pour un opérateur humain
  occasionnel ; un opérateur agent périodique demande d'abord un lot, un delta et une file d'actions
  pour un humain. Les faits montrent que la couverture automatisée ne vaut que couplée à un étage
  humain et à un canal (H1, H6) : statut « à tester », pas « infirmée ».
- **Test ou pilote** : pilote B, puis un pilote « lot » quand le code existe : un agent déroule le
  playbook sur toutes les communes ≥ 3 500 hab. d'un département, un humain traite la file.
- **Alternative** (recommandée) : écrire l'opérateur agent comme **mode principal** de la v1 — lot
  périodique, delta, file d'actions à signer par un humain aux points de contrôle (règles 1 à 4
  inchangées) — et l'opérateur humain occasionnel comme mode secondaire. Sert : « nombre
  d'institutions ayant fait l'objet d'au moins un audit », « territoires couverts ». Coût : texte de
  `docs/01` et `docs/02` §2 ; l'ordre des livrables de `docs/08` (lot et delta en phase 1 sont déjà
  prévus).
- **Décision** : ADR 0005 proposé.

### H8. Un protocole ouvert suffit pour que des tiers construisent les plateformes

- **La spécification dit** : « Local-first, zéro serveur. […] Le projet n'héberge rien. »
  (`CLAUDE.md`, règle 5) ; « Les plateformes, ce sont les autres qui les construisent, mille fois. »
  (`CLAUDE.md`) ; « Un service hébergé. Une API publique. […] Tout ce qui ferait du projet une cible
  plutôt qu'un protocole. » (`docs/08`, Ce qui n'est pas prévu).
- **Mécanisme supposé** : un format ouvert plus une implémentation de référence suffisent pour que
  des tiers hébergent, agrègent et diffusent.
- **Ce qui la falsifierait** : les protocoles pour lesquels des tiers ont construit des plateformes
  avaient une demande préexistante ou un acteur intéressé ; sans cela, protocole sans plateforme.
- **Ce qui tomberait avec elle** : rien dans la règle 5 ; mais « n'héberge rien » et « d'autres
  hébergeront » ne tiennent ensemble qu'avec un acteur nommé (H6).
- **Statut** : infirmée comme condition suffisante — 2026-11-09, première passe.
- **Faits** :
  - [3] Open North : « Represent, bien que le moins collaboratif, est sans doute le plus réussi des
    trois standards » (né d'une API déjà utilisée) ; « Open511, la collaboration la plus structurée
    des trois, a connu la moindre adoption » ; recommandations : « créer des standards fondés sur la
    demande de données », « simples et légers », « des outils logiciels » (validateurs, convertisseurs),
    « le soutien des éditeurs de logiciels ». — rapport cité en H4.
  - [2] GTFS : né d'un besoin (itinéraires) et d'un consommateur lancé en décembre 2005 ; CSV « aussi
    simple que possible » ; « des centaines de villes » en 2013 ; « une incitation commerciale
    tangible pour les agences de transport » parmi les ingrédients. —
    https://beyondtransparency.org/part-2/pioneering-open-data-standards-the-gtfs-story/, consulté le
    2026-11-09. Analogue [4] : la plateforme consommatrice existait avant le format, et le producteur
    gagnait à publier — ce qui n'est pas le cas d'une institution auditée.
  - [2] OpenSpending v2 et Where Does My Money Go : arrêt annoncé le 31 mai 2018 pour le 1er août,
    « plus de 500 $/mois d'hébergement », toute suite « devrait être portée par des mainteneurs
    communautaires » ; personne ne s'est proposé dans le fil. —
    https://discuss.okfn.org/t/end-of-life-for-openspending-v2-and-where-does-my-money-go/6799,
    consulté le 2026-11-09.
- **Argument contraire** : la spécification sait que c'est un pari (« dispersion par copie »), et
  Apache-2.0 vise les médias et institutions. Réponse : un pari écrit comme une certitude (« mille
  fois ») oriente le projet vers le protocole quand les faits disent que l'adoption suit un produit
  utilisé et un acteur qui en a besoin.
- **Test ou pilote** : pilote B et le partenariat de H6.
- **Alternative** (recommandée) : réécrire « mille fois » en pari explicite dans `CLAUDE.md` et
  `docs/01`, et faire de l'implémentation de référence le produit utilisable de la v1 (ce qu'elle est
  déjà) avec un premier consommateur nommé (H6). Sert : « territoires couverts par au moins un
  connecteur maintenu ». Coût : texte seul ; la règle 5 ne change pas.
- **Décision** : ADR 0006 proposé.

### H9. Verrouiller les sept règles avant la réalité protège plus qu'il ne fige

- **La spécification dit** : « les sept règles non négociables du `CLAUDE.md` ne peuvent être
  modifiées qu'à l'unanimité et après consultation publique. » (`docs/10` §2).
- **Mécanisme supposé** : un coût de changement très élevé protège de la dérive et de la capture.
- **Ce qui la falsifierait** : des projets ayant écrit des principes intangibles avant leurs
  utilisateurs les ont contournés en silence ou révisés tard ; un verrou posé après usage protège
  autant.
- **Ce qui tomberait avec elle** : le moment du verrou pour les règles 4 à 7 ; les règles 1 à 3
  restent hors champ.
- **Statut** : pari assumé — 2026-11-09, première passe.
- **Faits** :
  - [2] Debian, résolution générale 2022 : le Contrat social a été amendé pour autoriser des
    firmwares non libres sur les supports officiels, option 5 adoptée à 289 voix contre 63 au-dessus
    du seuil 3:1 requis pour modifier un document fondateur. —
    https://www.debian.org/vote/2022/vote_003.en.html, consulté le 2026-11-09. Un principe fondateur
    contredit par le matériel, révisé tard par une procédure lourde et prévue.
  - [2] Wikipédia : lancée « avec peu de règles » ; « Ignore all rules » conçue comme une
    « injonction temporaire et humoristique » ; son auteur juge ensuite qu'une « charte fondatrice »
    aurait aidé et la rejette pour Citizendium. — https://en.wikipedia.org/wiki/Ignore_all_rules,
    consulté le 2026-11-09. Cas inverse : principes verrouillés après usage ; le cofondateur regrette
    l'inverse.
  - Rien de trouvé sur des principes de *contenu* (du type « jamais d'accusation ») écrits avant
    usage dans un projet civique ; les cas portent sur la licence et la liberté logicielle.
- **Argument contraire** : les règles sont l'identité ; les changer sous pression est le risque de
  capture ; elles peuvent être déplacées ou mieux formulées, jamais affaiblies. Réponse : l'argument
  tient pour les règles 1 à 3 (hors champ) et pour le principe du verrou ; les faits ne tranchent pas
  le *moment* du verrou pour les règles 4 à 7, dont deux (5 et 6) sont des choix d'architecture que ce
  registre examine (H8). Statut « pari assumé », et le pari se décide par écrit.
- **Test ou pilote** : aucun ; la réalité tranchera au premier conflit entre une règle 4 à 7 et un
  fait.
- **Alternative** (recommandée) : verrou effectif au **lancement public de la phase 1**, après ce
  registre et la première `critique-specification`, pour les règles 4 à 7 ; les règles 1 à 3
  verrouillées dès maintenant. Coût : une phrase dans `docs/10` §2.
- **Décision** : ADR 0007 proposé.

### H10. Déterministe d'abord ; le LLM sert à trois choses seulement

- **La spécification dit** : « Déterministe d'abord. » et « Les règles sur données structurées sont
  des fonctions Python pures, testées, sans LLM. Le modèle d'IA sert à trois choses seulement »
  (`CLAUDE.md`, règle 4) ; « Les LLM sont non déterministes et peuvent halluciner. » et « Le rapport
  doit exister sans LLM. » (ADR 0002) ; « procès-verbaux des assemblées délibérantes (là où le LLM
  apporte le plus, car non structuré) » (`docs/01`, Tête de pont).
- **Mécanisme supposé** : la crédibilité exige qu'aucun signal ne dépende d'un LLM ; la phase 1
  déterministe produit assez de valeur pour un premier impact ; la source où le LLM apporte le plus
  attend la phase 2.
- **Ce qui la falsifierait** : (a) l'extraction par LLM avec preuve relocalisée atteint une fiabilité
  documentée comparable au parsing déterministe sur des documents publics ; (b) la valeur de la phase
  1 est trop faible pour un premier impact (H3) alors que les PV sont riches.
- **Ce qui tomberait avec elle** : le périmètre « trois choses seulement » (règle 4, `docs/10` §2
  s'applique) ; l'ordre des phases.
- **Statut** : confirmée pour le périmètre de la règle 4 ; pari assumé pour l'ordre des phases —
  2026-11-09, première passe.
- **Faits** :
  - [3] Extraction de votes dans des PV municipaux (portugais, janvier 2026) : 93,2 % de macro-F1
    pour un encodeur affiné en intra-domaine, « surpassant les approches génératives » ; en
    validation inter-communes, « dégradation substantielle » des encodeurs, les LLM en few-shot
    baissant moins. — https://arxiv.org/abs/2601.03997v1, consulté le 2026-11-09.
  - [3] Extraction champ par champ à correspondance exacte sur documents scannés (15 modèles, 2026) :
    82,4 % pour le meilleur modèle propriétaire, 79,1 % pour le meilleur modèle ouvert de 8 Md ;
    erreurs : « confusion de chiffres », rôles de champs confondus, et « prédictions non ancrées dans
    le contenu du document », y compris des « prédictions confiantes mais non étayées » quand la
    valeur est masquée. — https://arxiv.org/pdf/2602.07038, consulté le 2026-11-09.
  - [3] Résumés de documents juridiques et de presse (2025, 40 réponses annotées, résultat
    « directionnel ») : 30 % des réponses contiennent une hallucination ; 13 % pour l'outil qui cite
    ses passages, 40 % pour les autres ; « plutôt que d'inventer des faits ou des chiffres, les
    modèles ont fait preuve d'une surconfiance par excès d'interprétation » ; recommandation :
    « exiger des marqueurs de passage explicites pour chaque affirmation », distinguer
    « interprétation et extraction ». — https://arxiv.org/pdf/2509.25498, consulté le 2026-11-09.
  - [2] Rédactions : « toute information ou citation issue des résumés par IA devait être
    entièrement vérifiée » ; « c'est un tuyau ». — Nieman Lab, cité en H7.
- **Argument contraire** : rien à opposer sur le périmètre : les trois usages de la règle 4
  (extraire avec preuve, aider la résolution, rédiger ancré) sont exactement ceux que la littérature
  juge tenables sous contrôle humain, et l'erreur la plus dangereuse — une valeur inventée là où le
  document ne dit rien — est documentée. Sur l'ordre des phases : avancer les PV ne sauverait pas la
  petite commune (H3), parce que l'extraction y vaut au mieux deux tiers à neuf dixièmes et exige une
  validation humaine ; la dépendance technique (store, preuves, `verify`) impose de toute façon la
  phase 0.
- **Test ou pilote** : aucun pour le périmètre. Pour l'ordre des phases : revoir après le pilote A et
  le test C1.
- **Alternative** : aucune pour le périmètre. Pour l'ordre : si C1 confirme que la phase 1 ne produit
  rien pour la plupart des communes, avancer le connecteur de PV en phase 1 bis pour les communes
  ≥ 3 500 habitants, en gardant la règle 4 (extraction avec preuve, jamais décision de signal) —
  re-priorisation de `docs/08`, à décider après les faits.
- **Décision** : aucune.

## Pilotes en attente

| Pilote | Hypothèses testées | Exécutant | Coût | Décrit dans |
| --- | --- | --- | --- | --- |
| A — L'audit à la main | H2, H3, H10, promesse « quinze minutes » | **agent, avec réseau** (décision de l'auteur, 2026-11-09) ; la question finale reste à l'auteur | 3 à 5 h | [2026-11-09](2026-11-09.md) et « Pilote A » ci-dessous |
| B — Un rapport, une personne | H1, H5, H6, H7, engagement 4 | auteur (un agent ne peut pas être le destinataire) | 2 h + 4 semaines d'attente | [2026-11-09](2026-11-09.md) |
| C — Une fiche, un contributeur | H4, « 20 lignes de YAML » | auteur + une personne extérieure | 1 h + 1 à 2 h | [2026-11-09](2026-11-09.md) |
| T1 — Détectabilité par strate (test agent) | H3 | agent, avec réseau — **exécuté le 2026-11-13** | 1 heure | [2026-11-09](2026-11-09.md) ; résultat : [2026-11-13](2026-11-13-t1.md) |

### Pilote A — exécution par l'agent (ajouté le 2026-11-09, après la passe)

L'auteur a décidé que le pilote A serait exécuté par l'agent dans une session **ayant le réseau**,
pour éprouver l'utilité réelle du système avant d'écrire du code. Ce que cela change et ce que cela
ne change pas :

- **Ce que l'agent fait** : les étapes 1 à 5 de la procédure (récupérer les marchés d'une
  institution sur 36 mois, mesurer la complétude des champs, appliquer à la main les règles qui ne
  demandent pas de statistiques nationales — A1 sur 2024+, A3 au contrat, B1, B2, C2, C3, D1, E1,
  G2, G4 —, rédiger chaque constat au format de `docs/02` §4 et le relire contre le vocabulaire
  interdit de `docs/02` §4.2), sur **au moins deux institutions de strates différentes** dont une de
  moins de 2 000 habitants et une de 3 500 et plus. Chronométrer chaque étape.
- **Ce qui reste à l'auteur** : l'étape 6 — ce qu'il ferait de ce résultat le lendemain matin — et
  le pilote B, puisque le projet ne publie rien et que la livraison exige un humain (`docs/02` §5).
- **Préalable** : relever les seuils réellement en vigueur à la date de chaque marché sur la source
  officielle (page de la direction des affaires juridiques), faute de quoi B1 n'est pas calculable.
  L'agent **rapporte** ces valeurs et leurs URL ; il n'écrit pas dans `config/seuils.fr.yaml` et ne
  lève aucun **[À VÉRIFIER]** de lui-même (`CLAUDE.md`, « Où l'agent s'arrête »).
- **Contraintes** : aucune institution nommée dans ce qui remonte au dépôt (`docs/10` §3) — les
  résultats sont des agrégats et des strates ; aucune donnée de personne physique au-delà de
  `docs/09` §2, en particulier si un attributaire est un entrepreneur individuel ; les données
  téléchargées et les scripts restent hors du dépôt (`LESSONS.md`, « Outillage »).
- **Ce que le pilote décide** : si rien d'examinable ne sort pour l'institution de moins de 2 000
  habitants et qu'il en sort pour celle de 3 500 et plus, H3 et l'alternative de l'ADR 0005 sont
  confirmées ; si l'inverse, l'alternative tombe et le registre est réécrit. Le pilote **n'attend
  pas** les décisions sur les ADR 0004 à 0007 : c'est une mesure qui les informe.
- **Ce qui revient au registre** : nombre de marchés, part des champs renseignés, règles applicables
  et inapplicables avec la raison, signaux par règle, temps par étape, et les phrases de constat qui
  ont heurté le vocabulaire interdit. Statut de H3 mis à jour en conséquence.
