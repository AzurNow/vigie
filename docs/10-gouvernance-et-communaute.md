# 10 — Gouvernance, licences, communauté, financement

## 1. Licences

- **Code** (`vigie/`, `scripts/`, `tests/`) : **Apache-2.0**. Permissive, avec clause de brevet : adoption maximale, y compris par des médias et des institutions.
- **Commons** (`playbooks/`, `connectors/`, `catalogue/`, `config/`, `docs/`, `templates/`) : **CC-BY-SA-4.0**. Le savoir métier reste ouvert : quiconque l'améliore doit partager.
- **Résumés de strates** (données agrégées publiées) : **ODbL** ou **Licence Ouverte 2.0** selon la source **[À VÉRIFIER : compatibilité avec les licences des données d'origine]**.
- **Protocole** (docs 04, 05, 06, schémas) : CC-BY-4.0 + engagement écrit de non-revendication de brevet, pour que d'autres implémentations existent sans crainte.
- Contribution : DCO (Developer Certificate of Origin), pas de CLA. Pas de transfert de droits au projet.

## 2. Gouvernance

- **Phase solo** (phases 0-1) : un mainteneur. Décisions documentées dans `docs/decisions/` (ADR courts). Le but est de rendre le projet reprenable à tout moment : si le mainteneur disparaît, tout est là.
- **Trois mainteneurs** avant la phase 3 : au moins un profil journalisme/transparence, un profil données/code, un profil juridique ou éthique. Décisions à la majorité ; les sept règles non négociables du `CLAUDE.md` ne peuvent être modifiées qu'à l'unanimité et après consultation publique.
- **Fédération** : toute organisation peut forker, héberger, adapter. Le dépôt principal ne détient que le protocole, le cœur et le commons. Les forks territoriaux sont encouragés et listés dans `ECOSYSTEM.md`.
- **Hébergement institutionnel** (phase 3+) : adossement à une structure à but non lucratif existante (fondation du libre, organisation de journalisme d'investigation) plutôt que création d'une association propre, pour limiter la charge administrative et le risque de capture. Conditions : licence inchangée, gouvernance ouverte, pas d'exclusivité.
- **Anti-capture** : aucun financeur ne siège à la gouvernance ; aucun financement ne peut être conditionné à une orientation des règles ; les financements sont listés publiquement.

## 3. Communauté

- **Lieux** : dépôt (issues, discussions), un canal ouvert (Matrix ou Discourse — éviter les plateformes fermées), et participation aux communautés existantes plutôt que création d'une bulle : GIJN, Data for Good, Hacks/Hackers, OpenAleph Discourse, Open Contracting Partnership, forums open data des collectivités.
- **Contributions attendues, par ordre de facilité** : fiche de commune (YAML) → retour de faux positif → traduction → connecteur → règle → playbook.
- **Reconnaissance** : `CONTRIBUTORS.md` ; chaque fiche, connecteur, règle porte ses auteurs.
- **Code de conduite** : Contributor Covenant 2.1, plus une clause spécifique : toute contribution visant une personne nommément (fixture, issue, discussion) est refusée et retirée.
- **Ce qu'on ne fait pas en communauté** : on ne discute pas d'institutions nommées dans les espaces du projet. Les issues portent sur des règles, des sources, du code. Les rapports réels se discutent ailleurs, sous la responsabilité de leurs auteurs.

## 4. Financement

Principe : le projet doit **fonctionner sans argent** (local-first, rien à héberger). L'argent sert à accélérer, pas à survivre.

Sources compatibles avec l'anti-capture, dans l'ordre :
1. **NLnet / NGI Zero** : finance des particuliers et de petits projets libres, a déjà financé des composants d'Aleph. Dossier à préparer dès la phase 1 **[À VÉRIFIER : appels en cours et critères]**.
2. **Sovereign Tech Fund / Agency** : infrastructure numérique ouverte, une fois qu'il y a des utilisateurs.
3. **Fondations journalisme et transparence** : Open Society, Luminate, Fonds pour une presse libre / Fondation pour le journalisme (France), Pulitzer Center (bourses de reportage *utilisant* l'outil).
4. **Programmes publics open data / civic tech** (France, UE) : acceptables si sans conditionnalité sur les règles.
5. **Dons individuels** (Open Collective) : transparents par construction.

Sources exclues : fournisseurs de solutions aux acheteurs publics ou aux entreprises candidates ; partis ; toute entité qui serait sujet d'audit autrement que par un programme public ouvert à tous.

## 5. Indicateurs de santé du projet

- Contributeurs actifs sur 90 jours (cible phase 2 : 10).
- Délai médian de réponse aux issues (< 7 jours).
- Part des connecteurs avec `verifie_le` < 6 mois (> 80 %).
- Forks territoriaux actifs.
- Couverture de tests du cœur (> 90 %).
- Zéro incident « donnée personnelle indue » non traité en 48 h.
