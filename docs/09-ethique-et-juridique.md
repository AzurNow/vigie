# 09 — Éthique et cadre juridique

Ce document n'est pas un avis juridique. Il fixe les règles de conception du projet et liste les points à faire relire par un professionnel **[À VÉRIFIER]** avant la phase 2. Les règles de conception priment : en cas de doute, on fait moins.

## 1. Les trois principes

1. **Le pouvoir, pas les personnes.** Vigie examine l'exercice du pouvoir public par des institutions. Les personnes n'apparaissent que comme attributs d'une institution, dans leur capacité publique ou légale, et uniquement quand la source est un registre public institutionnel.
2. **Le signal, pas le jugement.** Vigie décrit des configurations de données publiques. Le jugement appartient à l'humain qui vérifie, publie et signe.
3. **La preuve, pas la confiance.** Tout est retraçable. Personne n'a à croire Vigie.

## 2. Données à caractère personnel

### Ce qui est autorisé
- **Élus et responsables publics** : nom et fonction, uniquement quand ils figurent dans un acte public de l'institution (PV, délibération, arrêté) ou dans une déclaration publiée par la HATVP, et uniquement pour le rapprochement prévu par les règles H1/H3 et l'extraction des déports (I). Le rapport par défaut **n'affiche pas le nom** ; il indique la fonction et renvoie au document (la preuve).
- **Dirigeants d'entreprises** inscrits au registre public (RNE, BODACC, JO associations) : nom et fonction, uniquement pour E4/H3, mêmes règles d'affichage.

### Ce qui est interdit
- Toute donnée sur une personne hors capacité publique ou légale : adresse personnelle, patrimoine, famille, opinions, activités privées, réseaux sociaux, photos.
- Toute **agrégation** de données personnelles (profil, historique, graphe centré sur une personne).
- Tout suivi d'**agents publics** (fonctionnaires, employés) : le projet ne lit pas les communications, ne surveille pas les horaires, ne nomme pas les agents.
- Toute donnée sur des **personnes privées** mentionnées incidemment dans un document (demandeurs d'aides, administrés) : les extractions LLM doivent les ignorer ; les fixtures de test n'en contiennent jamais.
- La conservation de documents contenant des données personnelles au-delà de ce que le store exige pour la preuve : si un document public contient des données personnelles non nécessaires, le connecteur en archive la version publiée (c'est la preuve) mais n'en extrait rien.

### RGPD (France / UE) **[À VÉRIFIER]**
- L'utilisateur qui exécute Vigie traite des données ; il en est responsable. L'outil doit rendre ce traitement minimal par construction (minimisation, finalité, pas de diffusion par défaut).
- Le projet n'est pas responsable de traitement : il ne collecte rien.
- Points à faire relire : qualification du traitement par un particulier (exemption « activité strictement personnelle » vs publication) ; réutilisation des données HATVP et des registres (conditions de réutilisation) ; jurisprudence sur la réutilisation des données de dirigeants (RNE) ; position de la CNIL sur la collecte automatisée de données publiques.

## 3. Diffamation et présomption d'innocence (France) **[À VÉRIFIER]**

- Loi du 29 juillet 1881 : l'imputation d'un fait précis portant atteinte à l'honneur est diffamatoire même si le fait est vrai, sauf preuve de la vérité ou bonne foi. La bonne foi suppose : but légitime, absence d'animosité, prudence dans l'expression, enquête sérieuse. **Les quatre critères sont encodés dans le produit** : but (contrôle de l'action publique), absence d'animosité (vocabulaire neutre imposé), prudence (signal, pas conclusion ; explications ordinaires systématiques), enquête sérieuse (chaîne de preuve, suites à vérifier).
- Présomption d'innocence : aucun rapport ne qualifie pénalement quoi que ce soit.
- Droit de réponse : les suites suggérées incluent toujours « demander une explication à l'institution » avant toute publication. Le modèle de question en conseil en est l'incarnation.
- Le projet ne publie rien. L'utilisateur qui publie engage sa responsabilité ; le texte fixe d'avertissement le dit, et le point de contrôle « export » le rappelle à chaque fois.

## 4. Accès aux données

- Seules les sources listées dans la doc 11 (institutionnelles, ouvertes ou publiées par obligation légale) sont admises dans le commons.
- Respect des licences et conditions d'utilisation des API. Respect de `robots.txt`. Identification du client. Débit limité.
- Jamais de contournement d'authentification, de captcha, de blocage. Si un site institutionnel bloque l'accès automatisé à un document que la loi oblige à publier, la réponse est l'import manuel par l'utilisateur et, le cas échéant, une demande CADA — pas un contournement.
- Jamais de données issues de fuites.
- Droit d'accès aux documents administratifs (CRPA) : les modèles de demande sont fournis ; l'utilisateur les envoie lui-même.

## 5. Protection des utilisateurs

- Un utilisateur peut auditer sa propre commune et s'exposer localement. L'outil n'y peut rien, mais : il ne stocke aucune information d'identité de l'utilisateur ; les rapports ne contiennent ni chemin local ni nom de machine ; la documentation recommande, pour la publication, de passer par un média ou une association plutôt que de publier seul, et renvoie vers les ressources de protection des lanceurs d'alerte et des journalistes **[À VÉRIFIER : Défenseur des droits, Maison des lanceurs d'alerte, Reporters Shield]**.
- Poursuites-bâillons : la meilleure protection est la qualité du rapport (preuve, prudence). Le projet documente les recours (directive européenne anti-SLAPP et sa transposition **[À VÉRIFIER]**).

## 6. Contre-usages et mesures

| Risque | Mesure |
|--------|--------|
| Harcèlement d'élus ou d'agents | Pas de noms par défaut ; pas d'agents ; vocabulaire neutre ; pas de score ; pas de classement |
| Campagnes politiques déguisées | Même rapport pour toutes les institutions ; méthodologie publique ; delta vérifiable par l'adversaire |
| Génération en masse de « rapports » pour discréditer | Chaque rapport est vérifiable par `verify` ; un rapport non vérifiable n'est pas un rapport Vigie et l'outil refuse de l'exporter |
| Usage par des acteurs malveillants pour cibler des fournisseurs | Données déjà publiques ; aucune information nouvelle n'est créée sur les entreprises, seulement des rapprochements de sources publiques |
| Capture par un financeur ou un État | Gouvernance (doc 10) ; licence ; fédération ; pas de serveur à capturer |
| Fausses accusations par erreur de l'outil | Déterministe d'abord ; tests ; explications ordinaires ; confiance explicite ; retour de faux positifs |

## 7. Procédure en cas de problème

- Un signalement d'erreur dans une règle est traité comme un bug de sécurité : issue prioritaire, correctif, note de version, et recommandation aux utilisateurs de régénérer les rapports concernés (`verify` détectera le changement de version de règle).
- Un signalement d'atteinte à une personne (donnée personnelle indue) : retrait de la fixture ou du connecteur fautif dans les 48 h.
- Contact : adresse dédiée dans le README.

## 8. Checklist avant d'ajouter une source ou une règle

- [ ] La source est institutionnelle, ouverte ou publiée par obligation légale, licence compatible.
- [ ] Aucune donnée de personne hors §2, ou alors uniquement par rôle, avec affichage masqué par défaut.
- [ ] La règle a des explications ordinaires non vides et des vérifications concrètes.
- [ ] La règle ne peut pas être lue comme une accusation sans vérification humaine.
- [ ] Les fixtures de test sont synthétiques ou anonymisées.
- [ ] Relecture par un second mainteneur.
