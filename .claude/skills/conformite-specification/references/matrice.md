# Matrice des exigences — `docs/conformite/exigences.json`

Un seul fichier, versionné, mis à jour à chaque passe. Il est la mémoire de la conformité : la passe
suivante part de lui au lieu de tout relire. L'historique Git en garde les états successifs.

## Forme

```json
{
  "$commentaire": "Matrice de conformité code ↔ spécification. Tenue par la compétence conformite-specification ; ne pas éditer hors d'une passe.",
  "specification": {
    "protocol_version": "0.1",
    "catalogue_version": "0.1.0",
    "sha256": {
      "CLAUDE.md": "<empreinte>",
      "docs/06-chaine-de-preuve.md": "<empreinte>"
    }
  },
  "passe": {
    "date": "AAAA-MM-JJ",
    "commit": "<sha court de départ>",
    "rapport": "docs/conformite/AAAA-MM-JJ.md"
  },
  "exigences": [
    {
      "id": "D06§1.04",
      "source": "docs/06-chaine-de-preuve.md",
      "section": "1",
      "citation": "Invariants vérifiés à la création : `preuves` non vide",
      "nature": "code",
      "statut": "testee",
      "code": ["vigie/protocol/signal.py:Signal"],
      "tests": ["tests/protocol/test_signal.py::test_signal_sans_preuve_refuse"],
      "lot": null,
      "constat": null,
      "note": null
    }
  ]
}
```

Les chemins et noms de l'exemple sont **illustratifs** : la matrice ne cite que ce qui existe.

## Champs

| Champ | Contenu |
| --- | --- |
| `id` | Identifiant stable (`references/extraction.md`). Jamais réattribué. |
| `source` | Fichier de la spécification. |
| `section` | Numéro de section, ou code de règle pour le catalogue. |
| `citation` | Phrase exacte, sans reformulation. Une ellipse `[…]` est permise pour couper, jamais pour changer le sens. |
| `nature` | `code`, `donnee`, `processus`, `hors-code`. |
| `statut` | Voir ci-dessous. |
| `code` | Emplacements qui appliquent l'exigence : `chemin:fonction` de préférence à `chemin:ligne`, qui se périme au premier ajout. Liste vide si non implémentée. |
| `tests` | Tests qui échoueraient si l'exigence était violée : identifiant pytest `chemin::test`. |
| `lot` | Phase et livrable de `docs/08` qui l'implémentera, si `a-implementer`. |
| `constat` | Numéro du constat dans le rapport de la passe, si l'exigence en porte un. |
| `note` | Une phrase, seulement quand le statut ne se comprend pas seul. |

## Statuts

| Statut | Sens | Constat ? |
| --- | --- | --- |
| `testee` | Implémentée, et un test nommé échouerait si elle était violée. | non |
| `sans-test` | Implémentée, aucun test ne la garde. | oui, moyenne si elle touche un signal ou une preuve |
| `contredite` | Le code fait autre chose que le texte. | oui, haute si elle touche un signal ou une preuve |
| `partielle` | Une partie de l'exigence est appliquée, pas le reste ; `note` dit laquelle. | oui |
| `a-implementer` | Le lot qui la porte n'existe pas encore ; `lot` le nomme. | non |
| `hors-code` | Nature `hors-code`. | jamais |
| `a-verifier` | Texte modifié depuis la dernière passe, pas encore retracé. N'existe qu'en cours de passe. | — |
| `retiree` | Supprimée de la spécification ; gardée pour la stabilité des identifiants. | non |

Les **règles non écrites** (sens inverse) ne sont pas dans la matrice, puisqu'aucune phrase ne les
fonde : elles vivent dans le rapport. Quand l'auteur en écrit une dans la spécification, elle devient
une exigence de la matrice à la passe suivante.

## Contrôles avant de rendre

- Chaque `id` est unique ; aucun `a-verifier` ne reste.
- Chaque exigence `testee`, `sans-test`, `contredite` ou `partielle` a au moins un emplacement de
  `code`, et chaque `testee` au moins un test.
- Chaque fichier cité existe (`ls`), chaque test cité se retrouve (`uv run pytest --collect-only -q`
  puis `grep` de son identifiant).
- Les empreintes de `specification.sha256` sont celles des fichiers au commit de départ de la passe.
