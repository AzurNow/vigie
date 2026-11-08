# ADR 0002 — Déterministe avant LLM

Date : 2026-11-07 · Statut : accepté

## Contexte
La crédibilité du projet repose sur l'absence de fausses accusations. Les LLM sont non déterministes et peuvent halluciner.

## Décision
Toute règle sur données structurées est une fonction pure testée. Le LLM n'intervient que pour extraire de la structure (avec preuves relocalisées), aider la résolution d'entités (confirmée par un humain si signal fort) et rédiger un texte strictement ancré (linter). Le rapport doit exister sans LLM.

## Conséquences
Durabilité face aux évolutions de modèles ; vérifiabilité ; coût nul possible ; mais les règles sur non-structuré (PV) restent à confiance moyenne et nécessitent une vérification humaine explicite.
