# Passation entre sessions

Point de reprise d'une session à l'autre, sur cette machine, une autre, ou dans le cloud : la mémoire
locale de Claude Code n'y est pas visible, ce fichier l'est. La section « État » est réécrite à chaque
clôture (compétence `cloture-de-session`), jamais empilée ; les passations précédentes sont dans
l'historique Git de ce fichier. Les consignes durables de l'auteur sont recopiées dans « Façon de
travailler », en bas : elles font foi.

## Fusion

À décider par l'auteur pour ce dépôt. Tant que ce n'est pas écrit ici : l'agent ouvre la PR, l'auteur
fusionne. Ne jamais pousser directement sur `main`. Si un contrôle de l'outil refuse une commande
(fusion, permission), le dire sans le contourner : une autorisation donnée en conversation ou en
mémoire ne lève pas une garde de l'outil.

## État au 2026-11-08

- **Phase 0 (fondations), rien n'est implémenté** : pas de `pyproject.toml`, pas de paquet `vigie/`,
  pas de CI. Le dépôt contient la spécification (`docs/`), la configuration de départ (`config/`), le
  connecteur `fr/decp` et la fiche d'exemple en YAML, le playbook `fr/audit-marches-publics-commune`.
- **Contexte agentique importé du Banc d'essai 2027** (projet voisin du même auteur), adapté à Vigie,
  sur la branche `chore/contexte-agentique`, non commité : six compétences (`cloture-de-session`,
  `complexite-maitrisee`, `revue-de-code`, `conformite-specification`, `critique-specification`,
  `revue-contexte-agentique`), deux agents (`codeur-opus`, `codeur-sonnet`), le gabarit de brief,
  ce fichier, `LESSONS.md`, `docs/DETTE.md`, `.gitignore`, et dans `CLAUDE.md` la règle de complexité,
  la section « Où l'agent s'arrête » et la clôture de session.
- **En attente de l'auteur** : `.claude/settings.json` (permissions de ménage des branches) n'a pas pu
  être écrit par l'agent (refus du mode auto, auto-modification) ; l'ADR 0003 (point d'entrée pour les
  autres agents) est proposé.

## Prochaines tâches, par priorité

Livrables de la phase 0 (`docs/08-roadmap.md`), dans l'ordre que donne `CLAUDE.md` (« État actuel ») :

1. Dépôt, licence, CI, `pyproject.toml` (avec `C901` à 9, voir `complexite-maitrisee`), structure de
   `docs/07` §3.
2. `vigie/protocol` : schémas pydantic Document, Evidence, Signal, Report ; `PROTOCOL_VERSION = "0.1"`.
3. `vigie/store` : store adressé par contenu, provenance, index duckdb.
4. Connecteur `fr/decp` (téléchargement, parsing vers OCDS, preuves déterministes, dédoublonnage G4).
5. Règles A1, B1, C2, puis G2, G4, avec vrais positifs et faux positifs connus.
6. La suite de la phase 0 : `fr/sirene`, `vigie/llm`, `vigie/lint`, runner de playbooks, rendu HTML,
   `verify`.

## Quand il faut l'auteur

Tout ce que liste « Où l'agent s'arrête » dans `CLAUDE.md`, les ADR au statut « proposé » de
`docs/decisions/`, et la levée de toute mention **[À VÉRIFIER]** dont dépend un livrable en cours.

## Façon de travailler (consignes durables de l'auteur)

- Une décision qui revient à l'auteur se pose **complètement** : contexte, options réelles, coût de
  chacune, recommandation en premier. Jamais une demi-ligne.
- Sur une décision d'outillage ou de code qui ne touche pas une zone de « Où l'agent s'arrête » :
  retenir l'option recommandée sans demander, et le dire. Paralléliser les lots indépendants.
- La session principale orchestre ; le code qui décide d'un signal ou d'une preuve part à l'agent
  `codeur-opus`, le code de routine à `codeur-sonnet`, avec un brief autonome d'après
  `.claude/briefs/GABARIT.md`. Ces agents ne commitent pas : la session principale relit leur diff
  (dont les lignes d'assertion des tests), rejoue les vérifications, commite, pousse et ouvre la PR.
  Deux agents en parallèle travaillent dans des worktrees séparés.
- Qualité avant dates : un code propre et complet vaut mieux qu'une durée de phase tenue.
- Ménage : branches fusionnées et worktrees d'agents supprimés avec `-d`, jamais `-D`, et seulement
  après avoir lu `state = MERGED`.
- Chaque PR dit ce qui pourrait casser ; une question qu'elle laisse ouverte devient le jour même un
  ADR « proposé ».
- Fin de session : compétence `cloture-de-session`. Près de la limite d'usage ou de contexte :
  s'arrêter, réécrire « État », commiter ce qui peut l'être, ne rien lancer de nouveau.
