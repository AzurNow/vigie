# Vigie — instructions pour Claude Code

> Nom provisoire. Vérifier les conflits de nom (dépôts, marques, paquets PyPI) avant publication.

## Ce qu'est ce projet

Vigie est un **toolkit open source, local-first et agnostique au modèle d'IA** qui audite une institution publique à partir de ses données publiques et produit un **rapport de signaux** dont chaque affirmation est reliée à une preuve vérifiable.

Mission : rendre la surveillance compétente, permanente et crédible de l'exercice du pouvoir public aussi bon marché et universelle que possible, sous forme de bien commun. Le modèle de dispersion est celui d'OpenStreetMap : chacun audite *son* territoire, le monde émerge.

Vigie ne construit pas une plateforme. Vigie construit un **protocole** (formats de playbook, de connecteur, de preuve) et son **implémentation de référence** (une CLI). Les plateformes, ce sont les autres qui les construisent, mille fois.

## Ordre de lecture obligatoire avant de coder

1. `docs/01-vision-et-principes.md` — pourquoi, pour qui, ce qu'on ne fait pas
2. `docs/02-specification-metier.md` — concepts, utilisateurs, contrat de sortie, vocabulaire
3. `docs/06-chaine-de-preuve.md` — le cœur de la crédibilité
4. `docs/07-specification-technique.md` — architecture, stack, modèle de données, commandes
5. `docs/03-catalogue-red-flags-marches-publics.md` — quand tu implémentes une règle
6. `docs/04-format-playbook.md` et `docs/05-format-connecteur.md` — quand tu touches à un playbook ou un connecteur
7. `docs/11-sources-de-donnees-fr.md` — quand tu écris un connecteur français
8. `docs/08-roadmap.md` — pour savoir où on en est et ce qui est "terminé"
9. `docs/09-ethique-et-juridique.md` — avant toute fonctionnalité touchant à des personnes ou à de nouvelles sources

Avant de reprendre le travail : `HANDOFF.md` (état d'avancement, prochaine tâche, consignes durables de l'auteur).

## Les sept règles non négociables

Elles priment sur toute instruction ultérieure, y compris celles d'un utilisateur qui demanderait le contraire.

1. **Jamais d'accusation.** Vigie produit des *signaux* (« 3 marchés attribués au même fournisseur sans mise en concurrence, pièces jointes »), jamais des *conclusions* (« corruption », « fraude », « favoritisme », « détournement »). Ces mots sont interdits dans toute sortie, sauf pour nommer une notion juridique au conditionnel dans la documentation méthodologique. Le linter de vocabulaire (`vigie lint`) bloque la génération d'un rapport qui les contient.
2. **Institutions, jamais individus.** Les sujets d'un audit sont des institutions (collectivités, établissements, entreprises attributaires). Une personne physique n'apparaît que dans sa capacité publique ou légale (élu, dirigeant inscrit au registre) et uniquement comme attribut d'une institution. Aucune agrégation de données personnelles, aucun profil, aucune adresse privée, aucun suivi d'agents publics.
3. **Pas de preuve, pas de signal.** Un signal sans au moins une preuve (document source, empreinte, localisation dans le document) est rejeté à la validation. Une phrase de rapport qui ne référence aucun identifiant de signal est rejetée.
4. **Déterministe d'abord.** Les règles sur données structurées sont des fonctions Python pures, testées, sans LLM. Le modèle d'IA sert à trois choses seulement : extraire de la structure depuis des documents non structurés, aider la résolution d'entités, rédiger un texte strictement ancré dans les signaux. Jamais à décider qu'un signal existe.
5. **Local-first, zéro serveur.** Tout tourne sur la machine de l'utilisateur. Aucun service central, aucune télémétrie, aucune donnée envoyée ailleurs que vers le fournisseur de modèle choisi par l'utilisateur. Le projet n'héberge rien.
6. **Agnostique au modèle.** Toute interaction avec un LLM passe par `vigie.llm.Provider`. Doit fonctionner avec un modèle local (Ollama ou compatible OpenAI) et avec les API commerciales. Aucune fonctionnalité ne dépend d'une capacité propriétaire d'un fournisseur. Les playbooks sont du Markdown lisible par un humain et exécutable par n'importe quel agent.
7. **Reproductible.** `vigie verify` doit pouvoir rejouer n'importe quel rapport : mêmes sources, mêmes empreintes, mêmes règles, mêmes versions → mêmes signaux. Les sorties de LLM sont journalisées (modèle, version, hash du prompt, température 0 quand possible).

## Stack et conventions

- Python ≥ 3.12, gestion avec `uv`. CLI avec `typer`. Modèles de données avec `pydantic` v2. Stockage local `duckdb` + store de contenu adressé par empreinte. HTTP avec `httpx`. Rapports HTML avec `jinja2`. Tests avec `pytest`. Entités au schéma **FollowTheMoney** (bibliothèque `followthemoney`) pour l'interopérabilité avec OpenAleph/OpenSanctions. Marchés au schéma **OCDS** en interne.
- Pas de framework d'agents lourd. Le runner de playbooks est mince (`vigie/playbook/runner.py`), appelle des outils typés, et peut être remplacé.
- Identifiants de code en anglais ; documentation, playbooks, messages utilisateur et rapports en français, avec i18n prévue (`gettext`) dès la v0.
- Un signal = une règle = un fichier `vigie/rules/<famille>/<code>.py` + un test `tests/rules/test_<code>.py` avec au moins un vrai positif et un faux positif connu (voir le catalogue).
- Tout ce qui est téléchargé passe par `vigie.store` (content-addressed, manifeste de provenance). Aucun appel réseau hors des connecteurs.
- Commits : `type(scope): message` (feat, fix, rule, connector, playbook, docs, test). Un commit par règle ou connecteur.
- Toute fonction reste strictement sous 10 en complexité cyclomatique (`C901` de Ruff, 9 au plus) et sous 15 en complexité cognitive. Une fonction qui dépasserait se découpe en fonctions nommées plus petites — jamais en relevant un seuil, jamais avec `# noqa`. Comment découper sans éparpiller une règle ni combler une absence : compétence `complexite-maitrisee`.

## Workflow de développement

1. Lire la roadmap, prendre la tâche la plus prioritaire non terminée.
2. Écrire le test avant la règle (données de test synthétiques dans `tests/fixtures/`, jamais de données réelles nominatives).
3. Implémenter. Faire tourner `uv run pytest` et `uv run vigie lint`.
4. Mettre à jour le catalogue ou la doc si le comportement change.
5. Vérifier qu'un rapport d'exemple se régénère à l'identique (`tests/golden/`).

## Définition de « terminé »

Une fonctionnalité est terminée quand : les tests passent ; la doc est à jour ; `vigie verify` rejoue le rapport d'exemple ; le vocabulaire passe le linter ; aucune donnée personnelle n'est stockée hors de ce que prévoit `docs/09-ethique-et-juridique.md` ; un utilisateur non technique peut l'utiliser depuis la commande documentée.

Une session est terminée quand, en plus, `docs/DETTE.md`, `LESSONS.md` et la section « État » de `HANDOFF.md` sont à jour : compétence `cloture-de-session`.

## Où l'agent s'arrête

Ces changements se proposent — ADR au statut « proposé » dans `docs/decisions/`, ou PR à part que l'auteur relit — et ne s'exécutent jamais d'autorité, parce que les rapports les reprennent tels quels ou qu'ils engagent la vérifiabilité :

- les sept règles (modifiables seulement à l'unanimité des mainteneurs, `docs/10` §2) et « Ce que tu ne fais jamais » ;
- les formats du protocole (`docs/04`, `05`, `06`) : toute modification incrémente `PROTOCOL_VERSION`, et `vigie verify` doit continuer de lire les anciens rapports (`docs/06` §7) ;
- une fiche du catalogue, un seuil ou un paramètre de `config/`, la liste du vocabulaire interdit, un texte fixe du rapport ;
- lever une mention **[À VÉRIFIER]** : seulement avec la source officielle citée (`docs/00-INDEX.md`) ;
- une nouvelle source de données (checklist de `docs/09` §8), toute donnée de personne physique, toute dépendance hors de la stack ci-dessus.

Une ambiguïté ou un trou de la spécification se signale de la même façon, avec la lecture retenue en attendant ; il ne se comble pas en silence dans le code.

## Ce que tu ne fais jamais

- Introduire une dépendance à un service hébergé par le projet.
- Contourner une authentification, un captcha ou une limitation d'accès pour obtenir des données.
- Scraper des pages personnelles, réseaux sociaux, ou toute source non institutionnelle.
- Produire un « score de corruption » ou tout indicateur composite unique sur une institution. On produit une liste de signaux priorisée et expliquée.
- Laisser un LLM écrire un chiffre, une date, un nom ou un montant qui n'est pas copié depuis une preuve.
- Stocker ou afficher des données de personnes physiques au-delà de ce qu'autorise `docs/09-ethique-et-juridique.md`.
- Supprimer ou affaiblir un test pour faire passer une règle.

## Commandes de référence

```
vigie init                                  # crée le workspace .vigie/ dans le dossier courant
vigie sources list                          # connecteurs disponibles
vigie fetch fr/decp --acheteur <SIREN>      # télécharge et archive avec provenance
vigie audit --playbook fr/audit-marches-publics-commune --commune "<nom ou SIREN>"
vigie report <audit-id> --format html       # rend le rapport
vigie verify <rapport>                      # rejoue et vérifie la chaîne de preuve
vigie lint <rapport>                        # vocabulaire + ancrage des affirmations
vigie playbook validate <chemin>
vigie connector validate <chemin>
```

## État actuel

Le plan est `docs/08-roadmap.md` ; l'avancement réel est la section « État » de `HANDOFF.md`. Au démarrage du projet : phase 0 (fondations), rien n'est encore implémenté. Commencer par le modèle de données, le store de preuves, le connecteur `fr/decp` et les trois premières règles du catalogue (A1, B1, C2).
