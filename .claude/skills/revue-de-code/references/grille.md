# Grille de lecture

Cinq axes. Pour chaque module, se poser les questions de chaque axe ; chaque « non » est un constat
candidat, à trier ensuite (`tri.md`).

## Signaux mécaniques

À relever avant de lire, et à copier en comptes dans le rapport. Seuls les fichiers suivis par Git
comptent : jamais `.claude/worktrees/` (copies complètes du dépôt), ni `.vigie/`, ni une sortie de
build.

```bash
uv run ruff check --statistics                      # par code de règle
uv run mypy --strict vigie/ | tail -n 1             # total
git ls-files -z '*.py' | xargs -0 grep -nE 'except( Exception)?:|except Exception as' | wc -l   # captures larges
git ls-files -z '*.py' | xargs -0 grep -nE '# (noqa|type: ignore)' | wc -l                       # désactivations
git ls-files -z '*.py' | xargs -0 grep -nE '\.get\([^,()]+, *[^)]+\)| or (0|""|\[\]|\{\})\b' | wc -l  # valeurs par défaut
git ls-files -z 'vigie/rules/*.py' | xargs -0 grep -nE 'httpx|open\(|Path\(|requests|duckdb' | wc -l  # I/O dans une règle
git ls-files -z 'vigie/*.py' ':!vigie/connectors/*' ':!vigie/llm/*' | xargs -0 grep -nE '^\s*(import|from) httpx' | wc -l  # réseau hors connecteurs et fournisseur LLM
comm -23 <(git ls-files | sort) <(git grep -Il '' | sort)                                       # fichiers que Git traite comme binaires
```

La dernière commande liste ce que Git considère comme binaire : une image est normale, un `.py`, un
`.yaml` ou un `.md` dans la liste est une alarme (axe 1, dernière question). Elle tient sur macOS, où
`grep -P` n'existe pas.

Chaque compte est une piste, pas un constat : un `.get(clé, défaut)` sur un champ facultatif qui a
un sens par défaut est légitime (axe 1).

## 1. Robustesse

- **Une donnée absente reste-t-elle absente et visible ?** `or 0`, `or ""`, `.get(clé, défaut)`
  sont légitimes pour un compteur ou un champ facultatif qui a un sens par défaut ; ils sont un
  défaut quand ils remplacent une donnée qui aurait dû être là. L'absence est une information
  (`docs/02` §6) : elle fait baisser la confiance ou produit un signal de famille G, elle n'est
  jamais comblée. Question à poser : *si cette valeur manquait, le rapport le dirait-il ?*
- **Une erreur capturée est-elle traitée pour ce qu'elle est ?** Un `except` nu, ou qui attrape tout
  pour afficher un message spécifique (« source vide » alors que le réseau est tombé), transforme une
  panne en fausse information. Le rapport distingue source inaccessible, vide et lacunaire
  (`docs/02` §6) : le code aussi.
- **Les frontières sont-elles validées ?** Tout ce qui entre (fichier téléchargé, réponse HTTP,
  YAML de connecteur ou de playbook, argument de ligne de commande, sortie de modèle) est validé par
  un modèle pydantic avant usage. Un `cast()` ou un `# type: ignore` sur une donnée externe est une
  promesse, pas une vérification.
- **Le code est-il indépendant de la plateforme ?** Linux, macOS et Windows sont visés (`docs/02`
  §9) : séparateurs de chemin (`pathlib`), fins de ligne, `open()` sans `encoding=`, fuseau horaire
  (horodatages en UTC explicites), locale de tri.
- **Le résultat est-il déterministe ?** `vigie verify` rejoue les règles et compare les signaux
  (`docs/06` §3) : horloge injectée, parcours de répertoire et de requête triés, pas d'ordre qui
  dépende d'un `set`, identifiants de signal stables.
- **Les écritures du store sont-elles atomiques et non destructives ?** Un document est immuable
  (`docs/06` §1) ; relancer `vigie fetch` doit être idempotent (`docs/07` §11).
- **Les extraits de preuve sont-ils copiés tels quels ?** Une normalisation, un `strip()` ou un
  changement d'encodage appliqué à l'`extrait` avant son empreinte casse la relocalisation par
  `verify`. Les normalisations vivent dans la comparaison, jamais dans la donnée.
- **Le source ne contient-il que ce qu'on voit ?** Un caractère de contrôle écrit brut (NUL,
  échappement, espace insécable dans du code) se lit comme autre chose, et un NUL fait classer le
  fichier comme binaire par Git : ses diffs disparaissent des PR.

## 2. Structure

- **Chaque règle, constante ou calcul vit-il à un seul endroit ?** Un seuil ou un paramètre écrit
  dans le code au lieu d'être lu dans `config/` (le catalogue l'interdit) ; un calcul de strate
  recopié dans une règle au lieu d'être pris dans `vigie/aggregate/` ; une liste de mots interdits
  ailleurs que dans `vigie/lint/vocabulaire.fr.yaml` ; une fiche de règle recopiée au lieu d'être lue
  dans `catalogue/`.
- **Les frontières d'architecture tiennent-elles ?** Une règle ne fait pas d'I/O (`docs/07` §5) ;
  aucun appel réseau hors des connecteurs et du fournisseur de modèle (`CLAUDE.md`, `docs/07` §14) ;
  tout appel de modèle passe par `vigie.llm.Provider` ; tout téléchargement passe par `vigie.store`.
- **Le LLM reste-t-il dans ses trois rôles ?** Extraire de la structure, aider la résolution
  d'entités, rédiger un texte ancré (`CLAUDE.md`, règle 4). Une sortie de modèle qui décide qu'un
  signal existe, ou qui écrit un chiffre non copié d'une preuve, est un constat haut.
- **Y a-t-il du code mort ?** Export jamais importé, branche inatteignable, paramètre toujours passé
  avec la même valeur.
- **Un module a-t-il une seule raison de changer ?** Un fichier qui mêle téléchargement, parsing et
  rendu en a plusieurs.
- **Un cas particulier est-il écrit en code au lieu d'être une donnée ?** (`if` sur une commune, un
  acheteur, une juridiction, une procédure qui devrait être une table de `config/`.)

## 3. Lisibilité

- **Les noms disent-ils ce que la chose est ?** Identifiants en anglais, textes destinés à
  l'utilisateur en français et passés par `gettext` (`CLAUDE.md`, « Stack et conventions »). Un même
  concept a-t-il un seul nom partout, cohérent avec le glossaire de `docs/02` §1 ?
- **Une fonction se lit-elle sans remonter ailleurs ?** Longueur (signal au-delà de 50 lignes),
  niveaux d'imbrication, paramètres booléens qui changent le sens de l'appel.
- **Les commentaires disent-ils pourquoi, et disent-ils vrai ?** Un compte écrit en dur (« les trois
  règles ») ment dès l'ajout suivant ; une référence à un fichier ou une fonction renommés aussi.
- **Les types portent-ils le sens ?** Une union discriminée plutôt que des champs optionnels
  corrélés ; `Literal` pour les énumérations du protocole ; modèles pydantic figés (`frozen`) là où
  rien ne doit muter (document, preuve).

## 4. Tests

- **Chaque règle a-t-elle au moins un vrai positif et un faux positif connu ?** (`CLAUDE.md`,
  catalogue « Ajouter une règle ».) Le faux positif doit ne **pas** déclencher, ou déclencher à
  confiance basse, comme la fiche le dit.
- **Chaque assertion vise-t-elle la bonne raison ?** Une entrée invalide doit échouer sur la règle
  visée seulement.
- **Les fixtures sont-elles synthétiques ?** Aucune donnée réelle nominative, aucun SIREN réel hors
  liste blanche d'institutions publiques (`docs/07` §12).
- **Le test est-il indépendant de la plateforme, du réseau et de l'ordre d'exécution ?** Aucun appel
  réseau en CI hors job `--online` (`docs/07` §12).
- **Teste-t-il un comportement, pas une implémentation ?** Un test qui casse à chaque refactoring
  sans changement de comportement coûte plus qu'il ne protège.
- **Les invariants ont-ils un test de propriété** quand le catalogue en énonce un (exemple de
  `docs/07` §12 : B2 ne se déclenche jamais si un marché individuel dépasse le seuil) ?

## 5. Cohérence documentaire

- **Ce que promettent `CLAUDE.md`, le README, `CONTRIBUTING.md` et les docstrings existe-t-il ?**
  Commandes, fichiers, fonctions nommées.
- **Les fichiers de doc sont-ils lisibles par tous les outils ?** Encodage UTF-8, fins de ligne LF.
- **`HANDOFF.md` (section « État ») et `docs/DETTE.md` disent-ils l'état réel ?**
