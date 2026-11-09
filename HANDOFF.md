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

## État au 2026-11-09

- **Phase 0 (fondations), rien n'est implémenté** : pas de `pyproject.toml`, pas de paquet `vigie/`,
  pas de CI. Le dépôt contient la spécification (`docs/`), la configuration de départ (`config/`), le
  connecteur `fr/decp` et la fiche d'exemple en YAML, le playbook `fr/audit-marches-publics-commune`.
- **Contexte agentique** (importé du Banc d'essai 2027, projet voisin du même auteur, PR #1 ; révisé
  PR #2 et #3, fusionnées) : sept compétences — `cloture-de-session`, `complexite-maitrisee`,
  `revue-de-code`, `conformite-specification`, `critique-specification` (mesure la spécification à
  l'impact : crédibilité **et** utilité), `hypotheses-structurantes` (les paris de `docs/01`, de la
  forme et des règles 4 à 7 traités comme hypothèses falsifiables ; règles 1 à 3 hors champ par
  décision de l'auteur du 2026-11-09), `revue-contexte-agentique` — deux agents (`codeur-opus`,
  `codeur-sonnet`), le gabarit de brief, ce fichier, `LESSONS.md`, `docs/DETTE.md`, `.gitignore`, et
  dans `CLAUDE.md` la règle de complexité, « Où l'agent s'arrête » et la clôture de session.
- **Première passe `hypotheses-structurantes` faite ; PR #4 ouverte, branche
  `docs/fondations-2026-11-09`** : `docs/fondations/registre.md` (dix hypothèses, statuts et faits
  sourcés), `docs/fondations/2026-11-09.md` (biais, écartés, trois pilotes, test T1, scripts), et
  quatre ADR proposés : 0004 (mécanisme de dissuasion et mesure de résultat), 0005 (premiers
  utilisateurs, opérateur et tête de pont de la v1), 0006 (adossement dès la phase 1, artefact
  partagé), 0007 (moment du verrou des règles 4 à 7). Ordre voulu : fondations → critique → code ;
  `critique-specification` ne se lance qu'après les décisions sur ces ADR.
- **En attente de l'auteur** : les ADR 0003 à 0007 ; les pilotes A (audit à la main), B (un rapport,
  une personne), C (une fiche, un contributeur) et le test T1 (détectabilité par strate : une
  journée agent **avec réseau**, scripts en annexe de la passe) ; `.claude/settings.json` (refus du
  mode auto, auto-modification) ; le ménage des branches distantes fusionnées
  (`chore/contexte-agentique`, `chore/critique-specification-impact`, `chore/hypotheses-structurantes`
  : le proxy refuse la suppression à l'agent) ; la règle de « Fusion » ci-dessus (les PR #2 et #3
  ont été fusionnées par l'agent sur demande explicite de l'auteur en conversation ; écrire si c'est
  la règle).

## Prochaines tâches, par priorité

1. L'auteur tranche les ADR 0004 à 0007 (la passe recommande) ; T1 et le pilote A peuvent précéder
   la décision sur 0005.
2. PR de révision de la spécification selon les ADR acceptés (`docs/01`, `docs/02` §2 et §9,
   `docs/08`, `docs/10` §2, en-tête du playbook, `config/strates.fr.yaml`), relue par l'auteur.
3. `critique-specification` sur le nouvel étalon (première passe ; verdict « protocole 0.1 prêt à
   être codé »).
4. Puis les livrables de la phase 0 (`docs/08-roadmap.md`), dans l'ordre que donne `CLAUDE.md`
   (« État actuel »), réordonnés si l'ADR 0005 est accepté (lot et delta remontent) :

   a. Dépôt, licence, CI, `pyproject.toml` (avec `C901` à 9, voir `complexite-maitrisee`), structure
      de `docs/07` §3.
   b. `vigie/protocol` : schémas pydantic Document, Evidence, Signal, Report ; `PROTOCOL_VERSION =
      "0.1"`.
   c. `vigie/store` : store adressé par contenu, provenance, index duckdb.
   d. Connecteur `fr/decp` (téléchargement, parsing vers OCDS, preuves déterministes, dédoublonnage
      G4). À noter d'après la passe : le « nombre d'offres reçues » n'existe que pour les marchés
      notifiés depuis le 1er janvier 2024 (arrêté du 22 décembre 2022) ; la consolidation
      ministérielle est quasi vide pour 2024-2026 ; le consolidé communautaire (Parquet, ~262 Mo)
      couvre 2018-2026.
   e. Règles A1 (sur données 2024+ seulement), B1, C2, puis G2, G4, avec vrais positifs et faux
      positifs connus.
   f. La suite de la phase 0 : `fr/sirene`, `vigie/llm`, `vigie/lint`, runner de playbooks, rendu
      HTML, `verify`.

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
