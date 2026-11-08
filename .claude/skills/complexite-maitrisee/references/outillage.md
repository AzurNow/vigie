# Outillage

`ruff` fait partie de la stack (`docs/07` §2) : la règle `C901` ne demande aucune dépendance de
plus. La complexité cognitive, elle, n'est pas couverte par Ruff : la mesurer demande un outil
supplémentaire, donc une décision de l'auteur (`CLAUDE.md`, « Où l'agent s'arrête »).

## Python — `C901` de Ruff

Ruff signale quand la complexité est **strictement supérieure** à `max-complexity` : la valeur 9
refuse 10 et laisse passer 9, ce qui est exactement « strictement sous 10 ».

```toml
# pyproject.toml
[tool.ruff.lint]
extend-select = ["C90"]

[tool.ruff.lint.mccabe]
max-complexity = 9
```

`extend-select` ajoute la règle aux règles déjà choisies au lieu de les remplacer. La règle doit
être une erreur de `ruff check` dans la CI : un avertissement qui ne casse pas le build n'est pas une
règle, c'est un souhait.

## Python — complexité cognitive

McCabe compte les chemins, la complexité cognitive compte ce qu'il faut tenir en tête : une
répartition plate et exhaustive sur dix cas marque peu en cognitive et beaucoup en McCabe ; trois
`if` imbriqués marquent l'inverse. Prises ensemble, elles attrapent des défauts disjoints — et la
seconde protège l'idiome voulu ici, le cas particulier traité en donnée plutôt qu'en branche.

Ruff n'a pas d'équivalent. Les outils existants sont extérieurs à la stack ; en choisir un (et
vérifier sur sa documentation officielle son nom, son code de règle et son seuil par défaut) est une
décision de l'auteur. Tant qu'aucun n'est installé, la limite de 15 se tient à la relecture, et la
découpe de la compétence vaut pour les deux mesures.

## Mesurer avant que `pyproject.toml` existe

Sans rien installer dans le dépôt :

```bash
uvx ruff check --isolated --select C901 --config 'lint.mccabe.max-complexity=9' <fichiers>
```

À la main, compter comme Ruff (vérifié avec ruff 0.16.8 sur des cas témoins) : `1` de base, puis

- `+1` par `if` et par `elif` (pas pour `else` seul) ;
- `+1` par `for` et par `while` ;
- `+1` par `except` ;
- `+(n − 1)` pour un `match` à `n` branches.

Ne comptent **pas** pour Ruff : `and`, `or`, `a if c else b`, les compréhensions (même avec `if`).
Une condition à quatre `and` vaut donc 2 en McCabe : c'est la complexité cognitive, et la relecture,
qui doivent l'attraper.

Exemple : une fonction à `1 + 3 if + 2 elif + 1 for` vaut 7 : conforme, mais proche. Au-delà de 7, ne
pas attendre le linter — c'est le moment de regarder si un cas particulier ne devrait pas être une
donnée de `config/`.
