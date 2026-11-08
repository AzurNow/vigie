# Contribuer

Merci. Par ordre de facilité :

1. **Ajouter votre commune** : copiez `connectors/fr/communes/EXEMPLE.yaml`, renommez avec le code INSEE, renseignez les URL officielles des PV et listes de délibérations, lancez `vigie connector validate`, ouvrez une pull request.
2. **Signaler un faux positif** : `vigie mark <signal> faux-positif -m "raison"` puis `vigie feedback export` et joignez le fichier (anonymisé) à une issue « calibration ».
3. **Traduire** : fichiers `.po` dans `vigie/i18n/`.
4. **Écrire un connecteur** : lire `docs/05-format-connecteur.md` et `docs/11-sources-de-donnees-fr.md`.
5. **Proposer une règle** : lire `docs/03-catalogue-red-flags-marches-publics.md`, section « Ajouter une règle ». Une règle sans tests et sans « explications ordinaires » n'est pas examinée.
6. **Écrire un playbook** : lire `docs/04-format-playbook.md`.

## Règles de la maison

- Les sept règles non négociables de `CLAUDE.md` s'appliquent à toute contribution.
- Aucune donnée réelle nominative dans les fixtures de test. Aucune institution nommée dans les issues.
- Lire `docs/09-ethique-et-juridique.md` avant toute contribution touchant à des personnes ou à une nouvelle source.
- DCO : signez vos commits (`git commit -s`).
- Code : `ruff`, `mypy --strict`, `pytest` doivent passer.
