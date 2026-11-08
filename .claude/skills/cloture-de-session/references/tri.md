# Trier : risque réel ou précaution ?

Trois questions. Un point qui ne passe pas les trois n'entre pas dans `docs/DETTE.md`.

## 1. Quel est le scénario de casse, en une phrase concrète ?

Il faut pouvoir écrire *quand X arrive, Y devient faux*. Si la réponse est « ça pourrait poser
problème », ce n'est pas un risque identifié, c'est un malaise. Le garder pour soi ou aller le
vérifier.

- Non : « la validation des signaux pourrait être insuffisante ».
- Oui : « si un parseur produit une preuve dont l'`extrait` est normalisé avant d'être empreint,
  `verify` ne peut plus le relocaliser dans le document source et rejette tous les rapports de cette
  source ».

## 2. Est-ce que ça vient de **cette** session ?

La dette recense ce que *ce travail* a introduit ou aggravé. Une faiblesse préexistante qu'on n'a
pas touchée appartient à l'entrée de la session qui l'a créée, ou à personne.

Cas limite : une décision prise cette session qui rend visible un problème plus ancien. Elle entre,
en le disant — c'est cette session qui a rendu la facture exigible.

## 3. Comment saurait-on que c'est arrivé ?

C'est la question qui donne la gravité.

- Aucun test, aucune alerte de `verify` ni de `lint`, aucun symptôme visible → **haute**. Le projet
  produit des rapports qu'on peut publier ; une affirmation fausse ou non prouvée qui passe tous les
  contrôles est le pire cas possible.
- Un test échoue, `verify` ou `lint` refuse, un build casse, quelqu'un s'en aperçoit à la lecture →
  **moyenne**.
- On s'en aperçoit tout de suite, ça coûte du temps → **basse**.

## Les faux positifs fréquents

| Ce qu'on est tenté d'écrire | Ce que c'est vraiment |
| --- | --- |
| « Il faudrait plus de tests » | Un vœu. Nommer le test manquant et l'invariant qu'il garde, sinon rien. |
| « Cette dépendance pourrait devenir obsolète » | Une généralité vraie de toute dépendance. |
| « Le code n'est pas optimisé » | Pas de la dette tant qu'aucune cible de `docs/07` §15 n'est manquée. |
| « La spécification est ambiguë sur ce point » | Un trou de spécification : ADR « proposé », pas de la dette. |
| « Il reste la règle Z ou le connecteur Z à écrire » | Un reste à faire de la roadmap, pas une facture différée. |
| « Un seuil est encore marqué [À VÉRIFIER] » | Une tâche de vérification connue (convention de `docs/00-INDEX.md`), sauf si du code l'a codé en dur. |

## Le cas du silence

Si aucun point ne passe les trois questions, écrire l'entrée quand même, d'une ligne :

```markdown
## AAAA-MM-JJ — <ce qui a été fait> (`<chemin/>`)

Aucun risque identifié : <pourquoi, en une phrase>.
```

Un journal qui saute les sessions calmes ne se distingue plus d'un journal oublié.
