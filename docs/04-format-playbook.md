# 04 — Format de playbook (protocole, v0.1)

Un playbook est une **recette d'audit** : un fichier Markdown avec un en-tête YAML, lisible par un humain sans outil, exécutable par le runner de Vigie, et portable vers n'importe quel autre agent ou framework (c'est volontairement proche d'un « skill » d'agent). Le playbook encode le savoir métier ; le code ne fait que l'exécuter.

## Emplacement et nommage

```
playbooks/<juridiction>/<slug>/PLAYBOOK.md
playbooks/<juridiction>/<slug>/templates/        # modèles de questions, de demandes CADA…
playbooks/<juridiction>/<slug>/tests/            # scénarios d'exécution (fixtures synthétiques)
```

Identifiant : `<juridiction>/<slug>`, ex. `fr/audit-marches-publics-commune`. Juridiction = code pays ISO en minuscules, ou `ocds`/`generic` pour l'international.

## En-tête YAML (obligatoire)

```yaml
---
id: fr/audit-marches-publics-commune
version: 0.1.0
protocol: 0.1
titre: Audit des marchés publics d'une commune
resume: Une phrase.
juridiction: FR
cible:
  type: commune              # type d'institution (voir modèle de données)
  identification: [nom, siren, code_insee]
entrees:
  - {nom: institution, type: institution, obligatoire: true}
  - {nom: periode, type: periode, defaut: "36 mois glissants"}
sources:                     # connecteurs requis / optionnels
  - {id: fr/decp, obligatoire: true}
  - {id: fr/sirene, obligatoire: true}
  - {id: fr/comptes-collectivites, obligatoire: false}
  - {id: fr/boamp, obligatoire: false}
regles: [A1, A2, A3, A4, B1, B2, C1, C2, C3, D1, D2, E1, E2, G1, G2, G4]
catalogue: ">=0.1.0"
niveau: debutant             # debutant | intermediaire | expert
duree_estimee: 15 min
necessite_llm: false         # false = rapport complet possible sans modèle (sans texte rédigé)
points_de_controle: [identification, rapprochements_forts, export]
licence: CC-BY-SA-4.0
auteurs: [...]
---
```

Le runner refuse un playbook dont l'en-tête est invalide, dont une règle n'existe pas dans le catalogue, ou qui ne déclare pas les trois points de contrôle obligatoires (voir doc 02 §5).

## Corps Markdown (sections, dans cet ordre)

### `## Objectif`
Ce que l'utilisateur saura à la fin. Deux ou trois phrases, sans jargon.

### `## À qui s'adresse ce playbook`
Et ce qu'il faut savoir avant (rien, idéalement).

### `## Étapes`
Liste numérotée. Chaque étape est l'un de trois types, indiqué par un badge en début de ligne :

- `[collecte]` : appel d'un connecteur. Syntaxe : `[collecte] fr/decp acheteur={institution.siren} periode={periode}`.
- `[regles]` : application d'un sous-ensemble de règles. Syntaxe : `[regles] A1 A3 B1 B2`.
- `[agent]` : tâche confiée au LLM **avec outils** et **sortie contrainte** par un schéma. Syntaxe : `[agent] schema=Deliberation sources=pv-conseil` suivie de l'instruction en prose. Toute sortie d'une étape agent doit référencer les documents (preuves) ; le runner rejette les sorties non ancrées.
- `[controle]` : point de contrôle humain. Syntaxe : `[controle] identification` suivi du texte affiché à l'utilisateur et des options.
- `[calcul]` : agrégat déterministe nommé (percentiles, deltas), `[calcul] strate_percentiles`.

Une étape peut être conditionnelle : `(si fr/boamp disponible)`.

### `## Points de contrôle humain`
Pour chaque point : quand il se déclenche, ce qui est montré, les choix possibles, ce qui se passe selon le choix.

### `## Sortie attendue`
Quelles sections du rapport sont produites, quels tableaux, quels exports. Doit être conforme au contrat de sortie (doc 02 §4).

### `## Pièges connus et faux positifs`
Spécifiques à ce playbook, en plus de ceux des fiches de règles. Ex. : « les communes membres d'un groupement de commandes apparaissent avec très peu de marchés ».

### `## Suites d'enquête suggérées`
Liste concrète : documents à demander, questions à poser, autres playbooks à enchaîner. Les modèles sont dans `templates/`.

### `## Limites`
Ce que ce playbook ne regarde pas, et pourquoi.

### `## Historique`
Changements par version.

## Exécution

```
vigie audit --playbook fr/audit-marches-publics-commune --commune "Nom" [--periode 2023-01..2025-12] [--batch]
```

- `--batch` exécute collecte, règles et calculs, puis s'arrête avant les points de contrôle et la rédaction ; produit le rapport JSON brut. Utile pour les associations (lots), puis reprise interactive.
- Le runner journalise chaque étape (entrées, sorties, durée, modèle utilisé) dans `.vigie/audits/<id>/journal.jsonl`. Le journal fait partie du manifeste de preuve.

## Portabilité

Un playbook ne contient aucun code. Les étapes `[agent]` sont des instructions en prose plus un schéma de sortie nommé (défini dans `vigie/protocol/schemas/`). N'importe quel agent capable d'appeler des outils et de produire du JSON validé peut exécuter un playbook ; c'est l'objectif : que d'autres implémentations existent.

## Validation

`vigie playbook validate <chemin>` vérifie : en-tête, existence des règles et connecteurs, présence des sections, présence des points de contrôle obligatoires, absence de vocabulaire interdit, licence, existence d'au moins un scénario de test.

## Contribution d'un playbook au commons

Un playbook est accepté s'il : passe la validation ; a été exécuté sur au moins trois institutions réelles par son auteur (sans que les rapports soient fournis — vie privée) ; a une section « pièges connus » non vide ; est sous CC-BY-SA-4.0.
