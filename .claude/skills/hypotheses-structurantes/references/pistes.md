# Pistes de départ

Des **questions**, pas des constats : chacune nomme la phrase de la spécification qu'elle vise, le
mécanisme que la phrase suppose, et ce qu'il faudrait savoir du monde pour statuer. La première
passe part d'ici ; une hypothèse qu'aucune piste ne prévoit reste une hypothèse. Les sous-agents de
l'étape 3 reçoivent les questions, jamais les réponses attendues.

Hors champ, rappel : les règles 1, 2 et 3 de `CLAUDE.md`.

## P1. La dissuasion sans visibilité

- **La spécification dit** : « Si l'observation devient universelle, permanente et crédible,
  l'équilibre change : chaque acheteur public, chaque conseil, chaque établissement se comporte
  comme s'il avait un reporter compétent assigné à vie. » (`docs/01`, « La thèse ») ; et « Vigie ne
  publie rien. » (`docs/01`, « Ce que Vigie n'est pas ») ; et « on ne discute pas d'institutions
  nommées dans les espaces du projet » (`docs/10` §3).
- **Mécanisme supposé** : un acheteur qui se sait observé change de comportement. La littérature
  invoquée (« ce qui se passe quand un journal local ferme ») porte sur une observation *publiée et
  visible*.
- **À savoir** : par quel chemin un acheteur apprend-il qu'un rapport ou que l'outil existe, et
  qu'établit la littérature sur l'effet d'une observation *non publiée* ? Quels dispositifs
  comparables (publication de données, outils de veille, rapports d'institutions de contrôle) ont
  mesuré un effet, et lequel ? Ce que tomberait avec l'hypothèse : l'engagement 1 (aucun nœud
  central) tel qu'il est écrit, ou la thèse elle-même.

## P2. Le premier utilisateur disperse le projet

- **La spécification dit** : « Le citoyen curieux […] Pas technique. […] C'est lui qui disperse le
  projet. » (`docs/01`, « Utilisateurs cibles ») ; et « Objectif v1.1 : installateur graphique
  minimal […], parce que le citoyen curieux n'ouvre pas de terminal. » (`docs/07` §13).
- **Mécanisme supposé** : des non-techniciens installent une CLI, l'exécutent, partagent, et c'est
  ce partage qui fait croître le projet avant la v1.1.
- **À savoir** : quels outils civiques comparables ont été adoptés par des non-techniciens, sous
  quelle forme (site, extension, fichier, service d'un tiers), et lesquels sont restés aux
  techniciens ? Qui exploite déjà les DECP aujourd'hui, et comment ? Si les premiers utilisateurs
  réels sont techniques, l'ordre des livrables de la phase 1 change (`docs/08`).

## P3. La tête de pont : communes et DECP

- **La spécification dit** : « Marchés publics, France, communes. Parce que : […] échelon communal =
  35 000 institutions, le plus proche des citoyens et le moins couvert par la presse. » (`docs/01`,
  « Tête de pont ») ; et, sur la qualité des DECP, « hétérogène. Nombre d'offres reçues souvent
  absent ; modifications mal renseignées ; doublons entre profils d'acheteurs » (`docs/11`, DECP).
- **Mécanisme supposé** : pour la commune que l'utilisateur choisit — la sienne —, les DECP
  contiennent assez de marchés, assez renseignés, pour qu'au moins une règle produise un signal
  examinable.
- **À calculer** : par strate, la part des communes ayant au moins N marchés publiés sur la période
  par défaut du playbook, et la part dont les champs requis par les règles de phase 1 sont
  renseignés ; la même chose pour les EPCI, les départements, les hôpitaux. L'argument de la
  spécification est le besoin ; le chiffre manquant est la **détectabilité**. Si elle est basse pour
  la commune médiane, la tête de pont de l'impact est peut-être une autre institution, et le premier
  persona un autre.

## P4. Le modèle OpenStreetMap

- **La spécification dit** : « Le modèle est OpenStreetMap : personne ne cartographie le monde,
  chacun cartographie son quartier. » (`docs/01`) ; « pas de vue agrégée nationale centralisée (les
  résumés de strates publiés dans le dépôt en tiennent lieu) » (ADR 0001) ; « Terminé quand : […] un
  contributeur externe a fait accepter une règle ou un connecteur. » (`docs/08`, phase 1).
- **Mécanisme supposé** : des contributeurs viennent et restent parce que leur contribution a une
  valeur visible pour d'autres, comme un tracé sur la carte commune.
- **À savoir** : ce qui, chez OpenStreetMap et dans les communs de données comparables, a produit
  les contributeurs (artefact partagé visible, effet de réseau, usage immédiat de sa propre
  contribution), et ce qui, dans Vigie, joue ce rôle quand chaque rapport reste local. Quels communs
  de « recettes » ou de connecteurs (sans carte partagée) ont atteint plusieurs dizaines de
  contributeurs, et lesquels sont restés à un ?

## P5. La mesure qui compte

- **La spécification dit** : « Le succès n'est pas le nombre de signaux. C'est : » suivi de huit
  mesures (`docs/01`, « Mesures de succès »).
- **Mécanisme supposé** : ces huit mesures tirent dans le même sens ; optimiser l'une ne dégrade pas
  les autres.
- **À savoir** : laquelle est la mesure de résultat (une question posée, une piste confirmée, un
  comportement d'acheteur qui change) et lesquelles sont des mesures d'activité (institutions
  auditées, territoires, contributeurs, rapports partagés) ? Quelle mesure, choisie comme étoile
  polaire, changerait la phase 1 ? Aucune mesure ne compte le dommage évité ni le dommage causé (une
  publication fausse par un utilisateur) : faut-il l'une des deux ?

## P6. Seul, ou adossé dès le début

- **La spécification dit** : « Un particulier, sans organisation, veut l'impact le plus large
  possible. » (ADR 0001, contexte) ; « Phase solo (phases 0-1) : un mainteneur. » et « Hébergement
  institutionnel (phase 3+) : adossement à une structure à but non lucratif existante » (`docs/10`
  §2).
- **Mécanisme supposé** : le projet atteint ses premiers utilisateurs, ses premiers contributeurs et
  sa première visibilité seul, puis s'adosse.
- **À savoir** : quelles structures existantes (associations de transparence, médias d'enquête,
  réseaux de journalistes, communautés de données) font déjà ce que la phase 1 promet, avec quels
  moyens et quels manques déclarés publiquement ? Un adossement en phase 1 règle-t-il P1 (chemin de
  visibilité), P2 (premiers utilisateurs) et P4 (contributeurs) d'un coup, et que coûte-t-il
  (gouvernance, anti-capture, `docs/10` §2 et §4) ?

## P7. L'opérateur est un agent

- **La spécification dit** : « Les grands modèles de langage sont la première technologie qui rend
  la lecture compétente de ces données quasi gratuite. » (`docs/01`) ; « N'importe quel agent capable
  d'appeler des outils et de produire du JSON validé peut exécuter un playbook » (`docs/04`,
  « Portabilité ») ; « Il n'y a pas de mode « sans humain » pour la livraison d'un rapport ; il y en
  a un pour la collecte et le calcul (`--batch`) » (`docs/02` §5).
- **Mécanisme supposé** (implicite, jamais écrit) : l'opérateur de l'outil est un humain qui choisit
  une institution, lance, lit, agit.
- **À savoir** : si l'opérateur principal est un agent qui déroule le playbook sur toutes les
  institutions d'un territoire, périodiquement, en `--batch`, et que l'humain n'intervient qu'aux
  points de contrôle et à la signature (règles 1 à 4 inchangées), qu'est-ce qui change — l'ordre des
  utilisateurs (association et journaliste d'abord), le coût de la couverture universelle que P1
  suppose, la forme de la v1 (un lot et un delta plutôt qu'« une commande » pour une commune), les
  points de contrôle de `docs/02` §5 ? Ce que le monde sait : quels dispositifs de veille agentique ou
  automatisée sur des données publiques existent, et ce qu'ils ont produit comme actions humaines.

## P8. « Les plateformes, ce sont les autres qui les construisent »

- **La spécification dit** : « Local-first, zéro serveur. […] Le projet n'héberge rien. »
  (`CLAUDE.md`, règle 5) ; « Les plateformes, ce sont les autres qui les construisent, mille fois. »
  (`CLAUDE.md`, « Ce qu'est ce projet ») ; « Un service hébergé. Une API publique. […] Tout ce qui
  ferait du projet une cible plutôt qu'un protocole. » (`docs/08`, « Ce qui n'est pas prévu »).
- **Mécanisme supposé** : un protocole ouvert suffit pour que des tiers construisent les
  plateformes qui portent l'impact ; le projet n'a pas besoin d'en être une.
- **À savoir** : pour quels protocoles ouverts des tiers ont-ils effectivement construit les
  plateformes, et à quelles conditions (implémentation de référence utilisable seule, demande déjà
  existante, acteur économique intéressé) ? Lesquels sont restés un protocole sans plateforme ? La
  règle 5 est dans le champ : la question n'est pas de l'abandonner mais de savoir si « n'héberge
  rien » et « d'autres hébergeront » tiennent ensemble sans un acteur nommé (P6).

## P9. Le verrou des sept règles avant la réalité

- **La spécification dit** : « les sept règles non négociables du `CLAUDE.md` ne peuvent être
  modifiées qu'à l'unanimité et après consultation publique. » (`docs/10` §2).
- **Mécanisme supposé** : rendre les règles très coûteuses à changer protège le projet de la dérive
  et de la capture.
- **À savoir** : quels projets libres comparables ont écrit des principes intangibles avant leurs
  premiers utilisateurs, et ce qui s'est passé quand la réalité les a contredits (règle contournée en
  silence, fork, révision tardive) ? Un verrou effectif au lancement de la phase 1, après la première
  passe de ce registre, protégerait-il autant pour moins de rigidité ? Les règles 1 à 3 restent
  hors champ ; la question porte sur le *moment* du verrou, et sur les règles 4 à 7.

---

## Pilotes de départ

Ce que le web ne tranchera pas, et que seul l'auteur peut exécuter, hors du dépôt, sans nommer
l'institution ni la personne dans ce qui revient au registre.

- **L'audit à la main.** Hypothèses : P2, P3, et la promesse « en moins de 15 minutes […] lisible sans
  formation » (`docs/02` §2.1). Dérouler le playbook `fr/audit-marches-publics-commune` à la main sur
  une institution réelle avec les données brutes des portails de `docs/11`, chronomètre en marche.
  Observer : le temps, le nombre de marchés trouvés, le nombre de règles applicables avec les champs
  présents, ce qui sort comme signal examinable, et ce que l'on ferait ensuite. Falsifie si : aucun
  signal examinable, ou aucune action évidente à la fin.
- **Un rapport, une personne.** Hypothèses : P1, P5, et l'engagement 4 de `docs/01`. Remettre le
  résultat de l'audit à la main, mis en forme comme le rapport de `docs/02` §4, à une seule personne
  qui peut agir (journaliste, élu d'opposition, association), et attendre. Observer : a-t-elle lu,
  posé une question, demandé un document, publié ; en combien de temps ; qu'a-t-elle demandé en plus.
  Falsifie si : rien ne se passe, ou ce qui se passe n'est pas une action que `docs/01` attend.
- **Une fiche, un contributeur.** Hypothèse : P4, et « ajouter une commune […] doit tenir en 20
  lignes de YAML » (`docs/02` §2.4). Demander à une personne extérieure d'écrire une fiche de commune
  à partir de `connectors/fr/communes/EXEMPLE.yaml` et de la documentation seule. Observer : le
  temps, les questions posées, ce qui a bloqué, et si la personne reviendrait. Falsifie si : la fiche
  n'aboutit pas sans aide, ou la personne ne voit pas à quoi elle a servi.
