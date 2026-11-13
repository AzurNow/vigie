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

## État au 2026-11-13

- **Phase 0 (fondations), rien n'est implémenté** : pas de `pyproject.toml`, pas de paquet `vigie/`,
  pas de CI. Ordre voulu inchangé : fondations → critique → code.
- **Contexte agentique** en place (PR #1 à #3 fusionnées) : sept compétences, deux agents, gabarit
  de brief, `LESSONS.md`, `docs/DETTE.md`.
- **Test T1 exécuté le 2026-11-13** (PR #6) : `docs/fondations/2026-11-13-t1.md`, H3 mesurée par
  strate. 8 à 11 % des communes de 500 à 3 500 habitants ont au moins 10 marchés en 36 mois ; la moitié
  des communes y arrive vers 4 000 à 5 500 habitants ; EPCI 79 %, départements 95 %. Le nombre
  d'offres est renseigné pour 84 à 87 % des marchés de 2024-2025 ; le consolidé communautaire n'a pas
  de rupture en 2024 (flux comptable de la DGFiP, ~68 % des marchés des petites communes).
- **Pilote A exécuté par l'agent le 2026-11-13** (PR #7) : `docs/fondations/2026-11-13-pilote-a.md`.
  Deux communes tirées au sort (vivier favorable pour la petite) ; un signal examinable sort des deux ;
  6 déclenchements sur 17 sont des faux positifs que les fiches B1 et B2 ne prévoient pas ; ADR 0008
  proposé (fiches B1, B2, plancher des règles à percentile, seuils 2026). Seuils relevés sur
  marche-public.fr et **rapportés**, pas écrits dans `config/` : le site de la DAJ refuse les accès
  automatisés (403), et le mode auto refuse à juste titre de le contourner.
- **PR ouvertes, toutes de documentation, empilées** : #4 (`docs/fondations-2026-11-09`, base
  `main`) ← #6 (`docs/t1-detectabilite`, base #4) ← #7 (`docs/pilote-a`, base #6) ; #5
  (`chore/cloture-2026-11-09`, base `main`, cette clôture). Voir `docs/DETTE.md`, 2026-11-13, point 1
  avant de fusionner.
- **Données de travail** (consolidé DECP du 2026-11-13, tables d'identifiants, scripts) : dans le
  répertoire temporaire de la session, hors du dépôt ; les scripts utiles sont recopiés en annexe des
  comptes rendus, les empreintes des entrées aussi.
- **En attente de l'auteur** : les ADR 0003 à 0008 ; l'étape 6 du pilote A (« qu'en ferais-tu demain
  matin ») ; le pilote B (peut partir de la commune B du pilote A ; les identités des
  deux communes sont remises à l'auteur hors du dépôt, jamais écrites ici) et le pilote C ; la
  réécriture de l'historique de la PR #7 (`docs/DETTE.md`, 2026-11-13, point 2) ; `.claude/settings.json` ; le ménage des branches distantes
  fusionnées (`chore/contexte-agentique`, `chore/critique-specification-impact`,
  `chore/hypotheses-structurantes`) ; la règle de « Fusion » ci-dessus.

## Prochaines tâches, par priorité

1. L'auteur fusionne #4, #6, #7 et #5 dans l'ordre et selon la procédure de `docs/DETTE.md`
   (2026-11-13, point 1), puis tranche les ADR 0004 à 0008, au vu de T1 et du pilote A pour 0005.
2. Petite PR de la session suivante : étape 0 des compétences `critique-specification` et
   `hypotheses-structurantes` (vérification du réseau, conduite sans) — `docs/DETTE.md`, 2026-11-09,
   point 2.
3. PR de révision de la spécification selon les ADR acceptés (`docs/01`, `docs/02` §2 et §9,
   `docs/03` B1/B2, `docs/08`, `docs/10` §2, `docs/11`, en-tête du playbook, `config/`), relue par
   l'auteur ; les seuils seulement après vérification par l'auteur sur la source officielle.
4. `critique-specification` sur le nouvel étalon (première passe ; verdict « protocole 0.1 prêt à
   être codé »).
5. Puis les livrables de la phase 0 (`docs/08-roadmap.md`), dans l'ordre que donne `CLAUDE.md`
   (« État actuel »), réordonnés si l'ADR 0005 est accepté :

   a. Dépôt, licence, CI, `pyproject.toml` (avec `C901` à 9, voir `complexite-maitrisee`), structure
      de `docs/07` §3.
   b. `vigie/protocol` : schémas pydantic Document, Evidence, Signal, Report ; `PROTOCOL_VERSION =
      "0.1"`.
   c. `vigie/store` : store adressé par contenu, provenance, index duckdb.
   d. Connecteur `fr/decp`, d'après T1 et le pilote A : un marché = un `uid` ; date de notification
      prise sur la ligne `modification_id = 0` (les lignes de modification du flux comptable portent
      la date de la modification) ; ~11 % de doublons inter-sources (même SIREN, date, montant,
      titulaire) ; identifiants d'attributaire invalides (G4) ; la colonne `type` donne travaux /
      services / fournitures ; retard de publication sur les 4 derniers mois.
   e. Règles A1 (sur données 2024+ seulement), B1, C2, puis G2, G4, avec vrais positifs et faux
      positifs connus — dont « lots d'une même consultation » pour B1 et B2.
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
