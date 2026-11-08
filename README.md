# Vigie

> Nom provisoire.

**Auditez l'exercice du pouvoir public à partir de ses données publiques, chez vous, en une commande, avec des preuves.**

Vigie est un toolkit open source, local-first et agnostique au modèle d'IA. Il télécharge les données publiques d'une institution (marchés publics, comptes, délibérations…), applique un catalogue de règles issues de la recherche anticorruption, et produit un rapport de **signaux** — jamais d'accusations — dont chaque affirmation est reliée à un document source vérifiable.

```
pipx install vigie
vigie init
vigie audit --commune "Votre commune"
```

## Pourquoi

L'essentiel de l'activité des institutions publiques n'est jamais examiné, non par secret, mais par manque de temps. Les données sont publiques ; personne ne les lit. Vigie rend cette lecture quasi gratuite, pour que chacun puisse auditer *son* territoire. Le modèle est OpenStreetMap : chacun cartographie son quartier, le monde émerge.

## Ce que Vigie n'est pas

Pas un média, pas un détecteur de corruption, pas un service, pas un outil de surveillance de personnes. Lire `docs/01-vision-et-principes.md`.

## Contribuer

La contribution la plus utile tient en 20 lignes de YAML : ajouter votre commune (`connectors/fr/communes/EXEMPLE.yaml`). Voir `CONTRIBUTING.md`.

## Documentation

`docs/00-INDEX.md`. Pour développer avec un agent de code : `CLAUDE.md`.

## Licences

Code : Apache-2.0. Playbooks, connecteurs, catalogue, documentation : CC-BY-SA-4.0.
