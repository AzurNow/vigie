# 01 — Vision et principes

## La thèse

L'impact du journalisme d'investigation ne réside pas dans les articles publiés mais dans **ce qui n'arrive pas** parce que quelqu'un regarde. La littérature économique mesure ce qui se passe quand un journal local ferme : le coût des emprunts de la collectivité augmente, les dépenses de personnel gonflent, la participation civique baisse, la corruption monte.

Aujourd'hui, une fraction infime de l'activité des institutions publiques est jamais examinée par un regard compétent et indépendant. L'immense majorité du pouvoir s'exerce sans témoin, non par secret, mais par **manque de bande passante** : les données sont publiques, personne n'a le temps de les lire.

Les grands modèles de langage sont la première technologie qui rend la lecture compétente de ces données quasi gratuite. Si l'observation devient universelle, permanente et crédible, l'équilibre change : chaque acheteur public, chaque conseil, chaque établissement se comporte comme s'il avait un reporter compétent assigné à vie. C'est de la dissuasion, et elle passe à l'échelle.

## Pourquoi un protocole open source et pas une plateforme

Une plateforme centralisée finit capturée (vendue aux fournisseurs ou aux lobbyistes, comme les outils de veille commerciale existants), fermée (comme le code d'Aleph en 2025) ou éteinte (perte de financement). Elle concentre aussi le risque juridique et les coûts sur une seule structure.

Un protocole plus une implémentation de référence se disperse par copie. Le modèle est OpenStreetMap : personne ne cartographie le monde, chacun cartographie son quartier. Ici, chacun audite sa commune, son hôpital, son département. Le commons (playbooks, connecteurs, catalogue de signaux) grandit avec les contributeurs ; le cœur reste petit.

## Les quatre engagements de conception

1. **Bien commun ouvert et fédéré.** Code sous licence permissive, commons sous licence copyleft. Aucun nœud central. Chaque territoire peut héberger, forker, adapter.
2. **Institutions, jamais individus.** La cible est l'exercice du pouvoir, pas la personne. Un outil qui surveille des agents publics nommément est une machine à harcèlement, pas du journalisme.
3. **Chaîne de preuve native.** Chaque signal est rejouable jusqu'au document source. Une fausse accusation publique détruit le capital de confiance du projet entier. La vérifiabilité *est* le produit ; l'IA n'est que le moteur.
4. **Boucle fermée vers l'action.** Les rapports sont conçus pour être repris par quelqu'un qui peut agir : journaliste local, élu d'opposition, association, auditeur, parquet. Un signal non transmis est un signal perdu.

## Ce que Vigie est

- Une CLI qui, en une commande, télécharge les données publiques d'une institution, applique un catalogue de règles éprouvées, et produit un rapport sourcé.
- Trois formats ouverts (playbook, connecteur, preuve) que d'autres outils peuvent adopter.
- Un commons de playbooks et de connecteurs, un territoire à la fois.
- Un outil pour non-experts : un citoyen curieux doit pouvoir auditer sa commune en une soirée.

## Ce que Vigie n'est pas

- Un média. Vigie ne publie rien. Les utilisateurs publient ce qu'ils veulent, sous leur responsabilité, après vérification humaine.
- Un détecteur de corruption. Vigie détecte des *configurations de données* qui, selon la littérature anticorruption, méritent un examen humain. La plupart ont une explication innocente.
- Un service. Pas de compte, pas de serveur, pas d'API hébergée par le projet.
- Un outil de surveillance de personnes. Voir `09-ethique-et-juridique.md`.
- Un agent autonome qui enquête seul. Le playbook contient des points de contrôle humains obligatoires.

## Utilisateurs cibles, par ordre de priorité

1. **Le citoyen curieux** (contribuable, membre d'association, conseiller municipal d'opposition). Pas technique. Veut savoir si quelque chose mérite une question en conseil. C'est lui qui disperse le projet.
2. **Le journaliste local** (PQR, média indépendant, pigiste). Veut des pistes sourcées qu'il peut vérifier et exploiter en quelques heures.
3. **L'association de transparence** (Anticor, Transparency, collectifs locaux). Veut auditer en série et suivre dans le temps.
4. **Le contributeur technique** (développeur, data scientist, étudiant). Veut ajouter sa ville, une source, une règle.
5. **L'auditeur ou l'institution elle-même** (CRC, service de contrôle interne, acheteur public). Veut s'auto-auditer avant qu'un autre ne le fasse. Public secondaire mais stratégique : c'est la dissuasion qui opère.

## Mesures de succès

Le succès n'est pas le nombre de signaux. C'est :

- Nombre d'institutions ayant fait l'objet d'au moins un audit (par territoire, par type).
- Nombre de territoires couverts par au moins un connecteur maintenu.
- Nombre de contributeurs actifs (connecteur, playbook, règle) sur 90 jours.
- Nombre de rapports partagés publiquement par leurs auteurs.
- Nombre de pistes confirmées par un humain comme méritant suite (retour volontaire).
- Taux de faux positifs déclarés (retour volontaire) par règle, utilisé pour calibrer.
- Reprises par des médias, questions posées en assemblée délibérante, saisines, citant un rapport Vigie.
- Indicateur de dissuasion à long terme (recherche externe) : évolution des indicateurs de concurrence des marchés dans les territoires couverts vs non couverts.

## Tête de pont

**Marchés publics, France, communes.** Parce que : le plus gros flux d'argent public ; données déjà structurées et ouvertes (DECP, BOAMP, TED) ; catalogue de red flags validé par quinze ans de recherche anticorruption (Open Contracting Partnership, Banque mondiale, OCDE, ANAC italienne, Prozorro ukrainienne) ; échelon communal = 35 000 institutions, le plus proche des citoyens et le moins couvert par la presse.

Ensuite, dans l'ordre : procès-verbaux des assemblées délibérantes (là où le LLM apporte le plus, car non structuré) ; budgets et subventions ; intercommunalités, départements, régions, hôpitaux, bailleurs sociaux, SEM ; puis un connecteur OCDS générique pour l'international.
