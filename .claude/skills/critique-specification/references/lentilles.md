# Lentilles de lecture

Chaque lentille est le regard d'un spécialiste qui relirait la spécification pour une revue
exigeante, ou d'un adversaire qui chercherait à faire tomber un rapport. Les questions sont des
**pistes de lecture**, pas une liste à cocher : une question sans objet ne produit rien, et un
problème qu'aucune question ne prévoit reste un constat. Chaque piste se termine par la même
épreuve : **quel signal, quelle preuve ou quelle phrase de rapport en dépend, et qui peut
l'exploiter ?**

Les sous-agents de l'étape 3 reçoivent leur groupe (A, B, C ou D) en entier.

---

## Groupe A

### A1. Validité des signaux et statistique

- **Ce que mesure chaque règle.** Pour chaque fiche du catalogue, la configuration de données
  détectée correspond-elle à ce que la littérature citée associe à un risque, ou à une version
  affaiblie par les données disponibles (DECP plutôt que dossiers de consultation) ? La fiche
  dit-elle l'écart ?
- **Déclenchement par construction.** Une règle à seuil de percentile de strate signale, par
  définition, une part fixe des institutions. Combien de règles de ce type un playbook applique-t-il,
  et quelle est la probabilité qu'une institution ordinaire en déclenche au moins une, ou trois ?
  **Calculer** (hypothèses d'indépendance et de corrélation explicites). Le rapport et le texte fixe
  le disent-ils au lecteur non technique ?
- **Faux positifs et taux de base.** Les « explications ordinaires » couvrent-elles les causes les
  plus fréquentes, ou les plus pittoresques ? Une règle dont presque tous les signaux s'expliquent
  ordinairement apporte-t-elle encore une information, et la priorité d'examen le reflète-t-elle ?
- **Strates et petits effectifs.** Percentiles sur des strates de petite taille, sur des communes à
  quelques marchés par an : `min_marches` et `min_marches_pour_agregat` suffisent-ils ? Un HHI ou une
  part calculés sur trois marchés ont-ils un sens ? Une strate fondée sur la seule population
  regroupe-t-elle des institutions comparables (commune centre, commune rurale, commune touristique) ?
- **Seuils et bandes.** `B1.bande`, `B2.fenetre_mois`, `C2.part` : d'où viennent-ils, et un autre
  choix raisonnable changerait-il beaucoup de signaux ? Une analyse de sensibilité est-elle prévue
  ou publiée ?
- **Sévérité, confiance, priorité.** Les deux axes sont-ils définis de façon qu'un même signal ne
  puisse pas recevoir deux confiances selon le lecteur ? La priorité d'examen (sévérité × confiance,
  puis récence) peut-elle se lire comme un score, alors que le projet s'interdit tout score ?
- **Combinaisons et double compte.** Un même fait peut-il déclencher plusieurs règles (B1 et B2, C1
  et C2) et peser plusieurs fois dans la synthèse ?
- **Deltas.** Entre deux audits, un signal qui apparaît ou disparaît peut-il venir d'une correction
  des données, d'un changement de strate, de seuil ou de version de règle plutôt que du comportement
  de l'institution ? Le delta le distingue-t-il ?
- **Calibration par les retours.** Les retours volontaires (`docs/02` §4.5) sont-ils un échantillon
  biaisé (on signale surtout les faux positifs) ? Qu'est-ce qui empêche une recalibration de suivre
  ce biais ?

### A2. Qualité des données publiques

- **Champs absents ou faux.** `nombre_offres_recues` souvent absent, procédures mal codées, montants
  HT ou TTC confondus, maximum d'accord-cadre pris pour un montant attribué, marchés subséquents
  codés comme marchés distincts : chaque règle touchée le dit-elle, et la confiance baisse-t-elle
  vraiment ?
- **Doublons et rapprochements.** Doublons entre profils d'acheteurs (G4), groupements de commandes,
  SIRET d'établissement contre SIREN d'entité : un mauvais rapprochement peut-il créer un fournisseur
  dominant (C2) ou un fractionnement (B2) qui n'existe pas ?
- **L'absence comme signal.** Une source vide ou lacunaire produit un signal de famille G : qu'est-ce
  qui distingue, dans le rapport, une institution qui ne publie pas d'un connecteur qui n'a pas
  trouvé, d'une source nationale incomplète, ou d'une période hors couverture ?
- **Couverture des règles.** Une règle qui exige une source optionnelle (A2 et le BOAMP) : quand la
  source manque, le rapport dit-il « non examiné » plutôt que de laisser croire à l'absence de
  signal ?
- **Statistiques nationales.** Les percentiles de strate se calculent sur des données nationales
  elles-mêmes lacunaires et inégalement renseignées selon les territoires : le biais déplace-t-il
  les signaux vers certaines strates ou régions ?

---

## Groupe B

### B1. Modèles de langage, ancrage et reproductibilité

- **Ce que l'ancrage laisse passer.** Le linter vérifie références, nombres, dates, montants, noms
  d'organisation et vocabulaire (`docs/06` §2). Un texte peut-il tromper sans enfreindre une seule de
  ces règles : sélection et ordre des faits, insinuation, juxtaposition de deux signaux, adverbes,
  « phrases de liaison non factuelles » qui orientent la lecture ?
- **Extraction contrainte.** Relocalisation des extraits par recherche exacte puis floue
  (`docs/07` §9) : un extrait relocalisé au mauvais endroit, ou proche mais différent, peut-il
  devenir une preuve ? Le seuil de la recherche floue est-il fixé, publié, et rejoué par `verify` ?
- **Résolution d'entités.** Le modèle propose, un score déterministe classe, l'humain confirme si le
  signal est fort : un rapprochement « probable » peut-il soutenir un signal moyen ou faible sans
  contrôle humain, et le rapport le montre-t-il assez pour un lecteur non technique ?
- **Agnosticisme réel.** Le même playbook avec un modèle local de petite taille et avec un modèle
  commercial : qu'est-ce qui diffère dans le rapport (textes, extractions, rapprochements), et
  l'en-tête du rapport suffit-il au lecteur pour en tenir compte ?
- **Injection par les documents.** Les PV, délibérations et pages web lus par le modèle sont des
  entrées non maîtrisées : un document contenant des instructions peut-il modifier une extraction,
  un rapprochement ou un texte ? Qu'est-ce qui, dans la spécification, le rend sans effet sur les
  faits ?
- **Rejouer.** `verify` rejoue les règles, pas les sorties du modèle (`docs/06` §4) : tout ce qui
  dépend d'une sortie de modèle (extraction, rapprochement) est-il soit confirmé par un humain, soit
  marqué, soit rejouable à partir du journal ?
- **Promesse de compatibilité.** « `verify` sait lire toutes les versions passées » (`docs/06` §7) :
  est-ce tenable quand une règle corrigée (bug de sécurité, `docs/09` §7) change ses signaux ? Que dit
  `verify` d'un ancien rapport produit par une règle depuis reconnue fausse ?

### B2. Pérennité face aux modèles et aux sources futurs

La question de cette lentille est : **qu'est-ce qui casse, ou perd son sens, quand un modèle bien
meilleur sort, quand un agent généraliste exécute les playbooks, ou quand une source change ?**

- **Agents qui exécutent les playbooks.** Les playbooks sont portables vers « n'importe quel autre
  agent » (`docs/04`). Un agent qui n'applique ni le linter ni `verify` produit-il un document qui
  peut passer pour un « rapport Vigie » ? Qu'est-ce qui distingue un rapport vérifié d'une imitation,
  pour un journaliste qui en reçoit un ?
- **Modèles plus capables.** Si un modèle sait lire des PV mieux qu'un humain pressé, la frontière
  « déterministe d'abord » (règle 4, ADR 0002) reste-t-elle juste, ou fige-t-elle une prudence qui
  deviendra une limite ? La spécification dit-elle comment cette frontière se déplacerait (ADR,
  version du protocole), sans affaiblir la règle 4 ?
- **Sources qui changent.** Nouveau schéma des DECP, seuils réglementaires modifiés, source fermée ou
  déplacée : les règles prennent-elles le seuil en vigueur à la date du marché, les connecteurs
  détectent-ils un changement de format au lieu de produire des champs vides ?
- **Lisibilité dans dix ans.** Un rapport porte-t-il de quoi être compris et vérifié hors de son
  contexte : versions du protocole, du catalogue, de la configuration, des règles, du modèle, et
  manifeste téléchargeable ?

---

## Groupe C

### C1. Usages, neutralité et effets

- **Lecture par un non-spécialiste.** Le citoyen curieux est le premier utilisateur (`docs/01`) :
  « sévérité forte » ou un percentile élevé se lisent-ils comme une accusation, malgré le
  vocabulaire neutre ? Le texte fixe et les « explications ordinaires » suffisent-ils à l'en
  empêcher ?
- **Comparaison et classement.** La synthèse comparative par strate pour les associations (`docs/02`
  §2.3, §4.3) peut-elle être retournée en classement d'institutions, que le projet exclut ?
- **Campagnes électorales.** Pendant une campagne locale, un rapport produit par un camp contre une
  équipe sortante : la symétrie (« même rapport pour toutes les institutions », `docs/09` §6)
  suffit-elle, ou faut-il un garde-fou (date, période examinée, delta vérifiable par l'adversaire) ?
- **Dissuasion.** La mesure de succès « dissuasion » (`docs/01`) suppose des territoires couverts et
  non couverts comparables : le projet peut-il, sans le vouloir, rendre cette mesure impossible ou
  biaisée ?
- **Effets sur les acheteurs.** Une règle peut-elle pousser un acheteur à un comportement pire pour
  éviter un signal (fractionner autrement, éviter un fournisseur local compétent) ?

### C2. Lecteur hostile

Lire trois fois la spécification, en se mettant chaque fois à la place de quelqu'un qui veut
contester un rapport **défavorable pour lui**, de bonne foi mais sans indulgence :

- **l'exécutif d'une institution signalée** : « vos seuils sont arbitraires », « vos données sont
  fausses et vous le savez », « vous comparez l'incomparable », « ce signal vise notre équipe » ;
- **une entreprise attributaire** : « vous nous désignez sans rien savoir de notre activité » (E1,
  E2, C2) — et si l'attributaire est un entrepreneur individuel, son identifiant désigne-t-il une
  personne physique au sens de `docs/09` §2 ?
- **un journaliste ou un chercheur sceptique** : « qu'est-ce qui prouve que l'outil ne trouve pas
  toujours quelque chose ? », « refaites l'audit avec une autre implémentation : trouve-t-on la même
  chose ? » ;
- **un contributeur malveillant** : une PR qui modifie un seuil, une fiche de commune pointant une
  mauvaise URL, un connecteur qui écarte silencieusement une partie des données, un fork qui retire
  le linter et garde le nom.

Pour chaque attaque, la spécification fournit-elle **une réponse publique, écrite avant tout
rapport** ? Une attaque sans réponse écrite est un constat ; une attaque déjà couverte ne produit
rien.

---

## Groupe D

### D1. Droit et éthique

Cette lentille formule des **questions pour un professionnel du droit**, jamais des avis
juridiques. Elle ne cite un texte de loi ou une décision qu'après vérification en ligne, avec l'URL.

- Les quatre critères de la bonne foi que `docs/09` §3 dit « encodés dans le produit » le sont-ils
  vraiment par une exigence vérifiable, ou seulement par une intention ?
- Données personnelles : élus, dirigeants inscrits, entrepreneurs individuels, personnes citées
  incidemment dans un PV. Chaque catégorie a-t-elle une règle de collecte, d'affichage et de
  conservation, et le store qui garde la version publiée d'un document (la preuve) est-il compatible
  avec la minimisation ?
- Réutilisation des sources : licences et conditions de chaque connecteur, `robots.txt`, débit ;
  registres dont la réutilisation est encadrée. Chaque connecteur du commons porte-t-il sa licence
  vérifiée ?
- Qui est responsable de quoi : l'utilisateur qui publie, le contributeur d'une règle fausse, le
  mainteneur. La procédure de `docs/09` §7 (erreur de règle traitée comme un bug de sécurité) a-t-elle
  les moyens de prévenir les auteurs de rapports déjà publiés ?
- Modèles de demande d'accès aux documents et de question en assemblée : restent-ils dans le
  vocabulaire autorisé et dans la présomption d'innocence ?

### D2. Ouverture, gouvernance et adoption du protocole

- **Réimplémentable ?** Une autre équipe peut-elle, avec les seuls `docs/04`, `05` et `06`, écrire une
  implémentation dont `verify` accepte les rapports, et qui produit les mêmes signaux ? Lister ce qui
  ne vit que dans le code de référence.
- **Licences.** Code Apache-2.0, commons CC-BY-SA-4.0, protocole CC-BY-4.0, résumés de strates sous
  licence à déterminer (`docs/10` §1) : un playbook (« proche d'un skill d'agent ») repris dans un
  produit fermé, une fiche de règle copiée dans le code : les licences le permettent-elles, et est-ce
  voulu ?
- **Contrôle des contributions.** Avec un seul mainteneur (`docs/10` §2), qu'est-ce qui empêche une
  contribution fautive (seuil, connecteur, fiche) d'entrer, et une fois entrée, d'être détectée ?
- **Versions.** Protocole, catalogue, configuration, règles, playbooks ont chacun leur version : la
  règle de montée de version est-elle écrite pour chacun, et un rapport les porte-t-il toutes ?
- **Capture et fédération.** Les forks territoriaux peuvent-ils diverger au point que deux « rapports
  Vigie » sur la même institution se contredisent ? Est-ce un problème, et la spécification le dit-elle ?

### D3. Faisabilité

- **Une règle qu'on ne peut pas exécuter est pire qu'une règle absente** : elle sera tournée en
  silence. Chaque exigence est-elle tenable par un mainteneur seul, aux durées de `docs/08` ?
- Les cibles de `docs/02` §9 et `docs/07` §15 (15 minutes, coût « d'un café », modèle local sur
  processeur) sont-elles compatibles avec le téléchargement et le traitement des données nationales
  nécessaires aux strates ? **Calculer** ce qui peut l'être.
- Le citoyen curieux installe-t-il réellement l'outil sans terminal avant la v1.1 (`docs/07` §13) ?
  Sinon, qui sont les vrais premiers utilisateurs, et la spécification est-elle écrite pour eux ?
- L'entretien du commons (fiches de communes à revérifier, `verifie_le` de moins de six mois,
  `docs/10` §5) est-il soutenable, et que fait l'outil d'une fiche périmée ?
- Quelles règles ou exigences pourraient être **retirées** sans perte de justesse, pour libérer du
  temps vers ce qui en a le plus besoin ?
