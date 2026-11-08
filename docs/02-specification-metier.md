# 02 — Spécification métier

Ce document décrit *ce que* Vigie fait du point de vue de l'utilisateur et du métier (audit de l'exercice du pouvoir public), indépendamment de la façon dont c'est implémenté. En cas de conflit avec la spécification technique, c'est ce document qui prime.

## 1. Glossaire

| Terme | Définition |
|-------|------------|
| **Institution** | Toute entité qui exerce un pouvoir public ou gère de l'argent public : commune, EPCI, département, région, établissement public, hôpital, bailleur social, SEM/SPL, syndicat, association subventionnée, État et ses services. Identifiée par un SIREN en France. C'est le seul type de sujet d'un audit. |
| **Acheteur** | Institution dans son rôle de pouvoir adjudicateur (passation de marchés). |
| **Attributaire / Fournisseur** | Entreprise ou organisme qui obtient un marché. Identifié par SIREN/SIRET. |
| **Source** | Un jeu de données ou un emplacement de documents publics (DECP, BOAMP, site de la commune, SIRENE…). Décrit par un **connecteur**. |
| **Document** | Un fichier obtenu d'une source, archivé avec sa provenance (URL, date, empreinte). |
| **Entité** | Une représentation normalisée (schéma FollowTheMoney) d'une institution, d'une entreprise, d'un marché, d'une délibération, etc. |
| **Règle** | Une fonction déterministe qui, appliquée à des entités, produit zéro ou plusieurs signaux. Une règle a un code (ex. `A1`), une famille, une sévérité par défaut, des faux positifs connus. |
| **Signal** | Le résultat d'une règle : une configuration de données qui mérite un examen humain, reliée à ses preuves. Jamais une conclusion. |
| **Preuve** | Le lien entre un signal et un document : identifiant du document, localisation (page, ligne, chemin JSON), extrait, méthode d'extraction. |
| **Playbook** | Une recette d'audit en Markdown : objectif, sources, étapes, points de contrôle humain, sortie attendue, pièges connus. |
| **Audit** | L'exécution d'un playbook sur une institution, pour une période. Produit un **rapport**. |
| **Rapport** | Le livrable : signaux priorisés, méthodologie, qualité des données, manifeste de preuves. Formats : JSON (canonique), HTML (lecture), Markdown (reprise). |
| **Strate** | Groupe d'institutions comparables (communes de même tranche de population, hôpitaux de même catégorie) servant de référence pour les comparaisons. |
| **Sévérité** | Importance d'un signal s'il est confirmé : `info`, `faible`, `moyenne`, `forte`. |
| **Confiance** | Fiabilité du signal compte tenu de la qualité des données : `basse`, `moyenne`, `haute`. Indépendante de la sévérité. |
| **Point de contrôle humain** | Étape d'un playbook où l'exécution s'arrête et attend une validation ou une décision de l'utilisateur. |

## 2. Utilisateurs et parcours

### 2.1 Le citoyen curieux — « Est-ce que quelque chose cloche dans ma commune ? »

1. Installe l'outil (une commande ; un installateur sans terminal est un objectif de v1).
2. `vigie audit --commune "Villeneuve-sur-X"`.
3. L'outil identifie l'institution (SIREN), télécharge les données publiques disponibles, applique le playbook par défaut, s'arrête aux points de contrôle (ex. « J'ai trouvé 2 acheteurs portant ce nom, lequel ? »).
4. Obtient en moins de 15 minutes un rapport HTML lisible sans formation : ce qui a été examiné, ce qui ressort, ce que ça peut vouloir dire (et ne pas vouloir dire), ce qu'un humain devrait vérifier ensuite, et les documents.
5. Peut partager le rapport (fichier autonome) ou poser une question en conseil municipal avec les références exactes.

Exigences : zéro jargon dans le rapport par défaut ; chaque signal accompagné d'une phrase « explication innocente possible » ; mode « expliquer comme à un voisin ».

### 2.2 Le journaliste local — « Donne-moi des pistes que je peux vérifier aujourd'hui »

1. Audite une ou plusieurs institutions de sa zone.
2. Lit le rapport en mode « enquêteur » : signaux triés par sévérité × confiance, preuves ouvrables en un clic, export des tableaux.
3. Suit les « suites d'enquête suggérées » du playbook : demande CADA à formuler, délibération à relire, registre à consulter.
4. Marque les signaux examinés (confirmé / explication trouvée / faux positif) dans son workspace local. Peut exporter ce retour anonymisé pour calibrer les règles.

Exigences : export FollowTheMoney (chargeable dans OpenAleph) ; export CSV ; modèle de demande d'accès aux documents administratifs pré-rempli ; rapport Markdown reprenable dans un article.

### 2.3 L'association — « Audite les 40 communes du département, chaque trimestre »

1. `vigie audit --playbook ... --liste communes.csv --periode 2024`.
2. Obtient un rapport par institution plus une synthèse comparative par strate.
3. Relance trimestriellement ; obtient les deltas (nouveaux signaux, signaux disparus).

Exigences : exécution par lot ; comparaison inter-institutions ; détection des changements entre deux audits ; aucune donnée envoyée à un serveur.

### 2.4 Le contributeur — « J'ajoute ma ville / ma source / ma règle »

1. Copie un connecteur existant, adapte les URL et le parseur, `vigie connector validate`.
2. Ou écrit un playbook pour un nouveau type d'institution.
3. Ou propose une règle avec ses tests et sa fiche au catalogue.
4. Ouvre une pull request. Le CI vérifie formats, tests, licence, absence de données personnelles dans les fixtures.

Exigences : ajouter une commune dont les PV sont sur un site standard doit tenir en 20 lignes de YAML ; une règle, en un fichier Python et un fichier de test.

### 2.5 L'institution elle-même — « Auditons-nous avant les autres »

Même outil, même rapport. Aucune fonctionnalité spécifique en v1, mais le rapport doit être suffisamment neutre et pédagogique pour qu'un service achats puisse s'en servir sans se sentir accusé. C'est le mécanisme de dissuasion.

## 3. Cycle de vie d'un audit

```
Identifier → Collecter → Normaliser → Résoudre → Appliquer les règles → Contextualiser → Rédiger → Vérifier → Livrer
```

1. **Identifier** l'institution : nom ou SIREN → fiche (SIREN, type, strate, territoire, sites web connus). Point de contrôle humain en cas d'ambiguïté.
2. **Collecter** : pour chaque source prévue par le playbook, le connecteur télécharge et archive les documents avec provenance. Ce qui est indisponible est consigné (le rapport dit ce qui n'a *pas* pu être examiné).
3. **Normaliser** : conversion en entités FollowTheMoney / contrats OCDS. Les champs manquants ou invalides sont consignés comme **signaux de qualité des données** (famille G).
4. **Résoudre** les entités : rapprochement attributaires ↔ SIRENE, acheteur ↔ budget, délibération ↔ marché. Le LLM peut proposer des rapprochements ; chaque rapprochement non exact est marqué « probable » et apparaît comme tel dans le rapport.
5. **Appliquer les règles** du catalogue retenu par le playbook, avec les seuils de la juridiction (`config/`).
6. **Contextualiser** : comparer chaque indicateur à la strate (percentile) ; attacher aux signaux les explications innocentes connues ; calculer la confiance d'après la qualité des données.
7. **Rédiger** : le LLM produit le texte du rapport à partir des signaux uniquement. Chaque phrase factuelle référence un ou plusieurs identifiants de signal ; chaque chiffre, nom, date, montant est copié depuis une preuve. Le linter rejette toute phrase non ancrée et tout vocabulaire interdit.
8. **Vérifier** : `vigie verify` recalcule les empreintes, rejoue les règles, vérifie l'ancrage. Un rapport non vérifiable n'est pas livré.
9. **Livrer** : rapport JSON canonique + HTML autonome + Markdown + exports.

## 4. Contrat de sortie : le rapport

Un rapport est un objet dont la structure est stable (versionnée) et dont le contenu obéit aux règles ci-dessous. Voir `06-chaine-de-preuve.md` pour le schéma exact.

### 4.1 Sections obligatoires, dans l'ordre

1. **En-tête** : institution (nom, SIREN, type, strate), période examinée, date de l'audit, playbook et version, version du catalogue, version de Vigie, modèle d'IA utilisé (nom, version) ou « aucun ».
2. **Ce que ce rapport est et n'est pas** (texte fixe, non modifiable par le LLM) : rappel que les signaux ne sont pas des conclusions, que la plupart ont une explication ordinaire, que la vérification humaine est indispensable, et que l'auteur de toute publication en est seul responsable.
3. **Périmètre et limites** : sources examinées (avec dates de récupération), sources prévues mais indisponibles, qualité des données (complétude par champ), ce qui n'a pas été examiné.
4. **Synthèse** : nombre de signaux par sévérité et par famille ; les 3 à 5 signaux prioritaires avec une phrase chacun ; comparaison globale à la strate en une phrase factuelle (« la part de marchés à attributaire unique est de 41 %, contre 22 % pour les communes de même taille examinées »).
5. **Signaux**, triés par priorité d'examen (sévérité × confiance, puis récence). Pour chaque signal :
   - identifiant stable (`SIG-2024-000017`), règle (code, nom, version), sévérité, confiance ;
   - **le constat** en une phrase factuelle, uniquement des faits présents dans les preuves ;
   - **pourquoi c'est examiné** (une phrase tirée de la fiche de la règle : ce que la littérature associe à cette configuration) ;
   - **explications ordinaires possibles** (liste tirée de la fiche de la règle, éventuellement filtrée selon le contexte) ;
   - **ce qu'un humain devrait vérifier** (liste tirée de la fiche, concrète : quel document, quelle question, quelle demande) ;
   - **preuves** : liste d'éléments cliquables (document, page/ligne, extrait).
6. **Tableaux de données** : les marchés examinés, les attributaires, les indicateurs calculés, avec les valeurs brutes.
7. **Méthodologie** : règles appliquées avec leurs seuils effectifs, strate de comparaison et sa composition, traitement des données manquantes.
8. **Manifeste des preuves** : liste de tous les documents (URL, date, empreinte, taille), qui permet à quiconque de re-télécharger et de vérifier.
9. **Suites possibles** : modèle de question en assemblée, modèle de demande CADA, contacts institutionnels pertinents (génériques : CRC, DGFiP, préfecture), jamais de contacts personnels.

### 4.2 Règles de vocabulaire

- Interdits en tant qu'affirmation : *corruption, fraude, favoritisme, détournement, prise illégale d'intérêts, délit, infraction, illégal, suspect, douteux, louche, collusion, entente, clientélisme, malversation, trucage*. Le linter les bloque.
- Autorisés : *signal, configuration, indicateur, écart, concentration, atypique par rapport à la strate, mérite vérification, non documenté dans les sources examinées, absence de trace de.*
- Jamais d'adjectif évaluatif sur une personne. Jamais de verbe d'intention (« a voulu », « a cherché à »).
- Les notions juridiques peuvent être nommées **uniquement** dans la fiche de règle (section « pourquoi c'est examiné ») sous la forme « la littérature anticorruption associe cette configuration à un risque de … », jamais rapportée à l'institution auditée.
- Les chiffres sont toujours accompagnés de leur base (« 7 marchés sur 17 », pas « 41 % » seul) et de la période.

### 4.3 Sévérité et confiance

- **Sévérité** (fixée par la fiche de règle, modulable par le contexte : montant, récurrence) : `info` (qualité des données, curiosité), `faible`, `moyenne`, `forte`. Un signal `forte` seul ne justifie jamais une conclusion ; il justifie une vérification en priorité.
- **Confiance** (calculée) : dépend de la complétude des champs utilisés, de la qualité du rapprochement d'entités (exact / probable), de la taille de l'échantillon, de la couverture de la source. Une règle appliquée sur une source lacunaire produit des signaux à confiance `basse` et le dit.
- **Priorité d'examen** = ordre de tri uniquement. Pas de score agrégé par institution. Si une vue comparative est nécessaire (associations), elle présente des indicateurs bruts par strate, pas un classement.

### 4.4 Comparaison par strate

Chaque indicateur quantitatif (part d'attributaire unique, concentration, part d'avenants…) est présenté avec le percentile de l'institution dans sa strate, calculé sur les institutions de même strate disponibles dans les données nationales (pas seulement celles auditées). Les strates par défaut pour les communes sont les tranches INSEE de population (`config/strates.fr.yaml`). Un signal qui repose sur un écart à la strate indique la taille de la strate et la période.

### 4.5 Faux positifs et retour

- Chaque fiche de règle liste ses faux positifs connus ; ils apparaissent dans le rapport sous « explications ordinaires possibles ».
- L'utilisateur peut marquer localement un signal : `confirmé`, `expliqué`, `faux positif`, avec commentaire. Ces marques restent locales.
- Un export anonymisé et volontaire (code de règle, strate, marque, sans identifiant d'institution) peut être envoyé au dépôt sous forme de pull request pour recalibrer les seuils. Jamais automatique.

## 5. Points de contrôle humain

Un playbook déclare ses points de contrôle. Ceux-ci sont **obligatoires** pour tout playbook accepté dans le commons :

1. Confirmation de l'institution identifiée (si ambiguïté ou si le nom est fourni sans SIREN).
2. Validation des rapprochements d'entités « probables » qui soutiennent un signal `forte`.
3. Avant tout export destiné à être partagé : affichage du rappel de responsabilité et des signaux à confiance `basse`.

L'outil ne publie rien et ne contacte personne. Il n'y a pas de mode « sans humain » pour la livraison d'un rapport ; il y en a un pour la collecte et le calcul (`--batch`), qui s'arrête avant la rédaction.

## 6. Qualité et absence de données

L'absence de données est une information. Le rapport distingue : source non prévue par le playbook ; source prévue mais inaccessible (erreur, site hors ligne) ; source accessible mais vide pour la période ; source accessible mais lacunaire (champs manquants). Les deux derniers cas produisent des signaux de famille G (transparence), parce qu'une institution qui ne publie pas ce que la loi l'oblige à publier, c'est déjà un constat.

## 7. Suivi dans le temps

Un audit est daté. Deux audits de la même institution avec le même playbook produisent un **delta** : signaux nouveaux, signaux disparus, signaux dont la sévérité ou la confiance a changé, documents nouveaux. Le delta est un rapport à part entière. C'est le mode « veille » : une association ou un journaliste relance tous les trimestres.

## 8. Périmètre par type d'institution (progressif)

| Type | Sources v1 | Playbook | Phase |
|------|-----------|----------|-------|
| Commune | DECP, SIRENE, budget | `fr/audit-marches-publics-commune` | 1 |
| Commune | + PV de conseil, liste des délibérations | `fr/audit-deliberations-commune` | 2 |
| Commune | + subventions, HATVP (liens déclarés) | `fr/audit-subventions-commune` | 3 |
| EPCI, département, région | mêmes sources, strates adaptées | variantes | 3 |
| Hôpital, bailleur, SEM/SPL | DECP, comptes publiés | à écrire | 4 |
| International | OCDS générique | `ocds/audit-buyer` | 5 |

## 9. Exigences non fonctionnelles (métier)

- **Temps jusqu'au premier rapport** : < 15 minutes pour une commune, données déjà en cache ou non, connexion ordinaire.
- **Aucune expertise requise** pour lire le rapport par défaut. Le mode enquêteur est un niveau de détail, pas un autre outil.
- **Hors ligne après collecte** : la rédaction et la vérification doivent fonctionner sans réseau si le modèle est local.
- **Langue** : français en v1 ; tout texte utilisateur passe par i18n ; les fiches de règles sont traduisibles indépendamment.
- **Portabilité** : Linux, macOS, Windows. Un rapport HTML est un fichier unique, lisible sans Vigie.
- **Coût** : un audit communal complet avec un modèle commercial doit rester de l'ordre du prix d'un café ; sans LLM, il doit fonctionner (rapport sans texte rédigé, signaux et tableaux seulement).

## 10. Critères d'acceptation de la v1 (phase 1)

- Un utilisateur sans compétence technique installe Vigie et obtient un rapport pour n'importe quelle commune française présente dans les DECP en moins de 15 minutes.
- Au moins 10 règles du catalogue (familles A, B, C, D, E, G) implémentées, testées, documentées.
- 100 % des signaux d'un rapport ont au moins une preuve ; `vigie verify` passe sur 20 rapports de communes réelles choisies aléatoirement.
- Le linter de vocabulaire ne laisse rien passer de la liste interdite sur ces 20 rapports.
- Un contributeur externe a ajouté un connecteur ou une règle acceptée dans le commons.
- Aucune donnée de personne physique dans les fixtures de test ni dans les rapports au-delà de ce qu'autorise `09-ethique-et-juridique.md`.
