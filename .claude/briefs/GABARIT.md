# Gabarit de brief pour un sous-agent codeur

Rédigé par la session principale pour `codeur-opus` ou `codeur-sonnet`. Le sous-agent démarre à
froid : il ne voit ni la conversation, ni les décisions prises, ni les fichiers déjà lus. Tout ce
qu'il doit savoir est dans le brief. Un brief qu'on ne pourrait pas confier à un contributeur humain
nouveau sur le dépôt n'est pas prêt.

Ce fichier n'est pas une définition d'agent : il vit hors de `.claude/agents/` pour ne pas être
chargé comme tel. Un brief réel est gardé en fichier sous `.claude/briefs/` et nommé dans la section
« État » de `HANDOFF.md` avec sa branche et son worktree, pour survivre à un `/clear` ou à une
coupure.

---

## Agent

`codeur-opus` (ce qui décide qu'un signal existe ou qu'une preuve tient, refactoring délicat) ou
`codeur-sonnet` (CLI, rendu, téléchargement, validation des formats, i18n, docs). Isolation :
`worktree` si un autre sous-agent travaille en parallèle.

## Objectif

Une phrase : ce qui doit exister à la fin et qui n'existe pas maintenant.

## Contexte que le sous-agent ne peut pas deviner

- Décisions déjà prises, avec le document et la section qui les fondent (`docs/06` §1, fiche B2,
  ADR 0002).
- Pourquoi cette tâche maintenant (livrable de `docs/08`), et ce qui en dépend ensuite.
- Vocabulaire du dépôt utile ici (signal, preuve, strate, fiche, point de contrôle…), si le nom
  seul ne suffit pas : renvoyer au glossaire de `docs/02` §1.

## Fichiers

- À lire d'abord : chemins exacts, et ce qu'il faut y chercher.
- À créer : chemins exacts.
- À modifier : chemins exacts, et la nature de la modification.
- À ne pas toucher : ce qui est proche mais hors périmètre.
- Si la tâche change une règle lue à plusieurs endroits : la liste, obtenue par `grep`, de tous les
  endroits qui la lisent.

## Contraintes de la spécification applicables

Les règles de `CLAUDE.md` et de `docs/` qui mordent sur cette tâche précise, citées, pas
paraphrasées. Exemple : « `docs/07` §5 : une règle ne fait pas d'I/O. »

## Cas limites à tester

Liste fermée, écrite à partir du texte de la spécification et des décisions, jamais de mémoire.
Chaque ligne devient au moins un test. Exemples de la maison, tirés de la spécification :
- `nombre_offres_recues` absent (A1 : confiance basse, jamais zéro) ;
- montant égal au seuil, juste sous la bande, juste dans la bande (B1 : `seuil × (1 − bande) <=
  montant < seuil`) ;
- montant maximum d'accord-cadre (`value.is_maximum`) pris pour un montant attribué ;
- seuil qui change de valeur à une date : marché notifié la veille, le jour même ;
- borne d'une fenêtre glissante (B2, `fenetre_mois`) ;
- HT et TTC confondus ;
- doublon entre profils d'acheteurs (G4) ;
- source inaccessible, vide, lacunaire (`docs/02` §6 : trois cas distincts dans le rapport) ;
- document modifié à la source depuis l'audit (`docs/06` §3, étape 2) ;
- extrait de preuve non retrouvé, ou retrouvé seulement par recherche floue (`docs/07` §9) ;
- mot interdit avec accord, majuscule ou variante (`docs/07` §10) ;
- personne privée citée incidemment dans un document (`docs/09` §2 : rien n'est extrait) ;
- entrée invalide à une frontière (YAML de connecteur ou de playbook, réponse HTTP, sortie de
  modèle hors schéma).

## Ce qui ne doit pas bouger

Comportements, signatures, fichiers, tests et rapports golden (`tests/golden/`) que le diff ne doit
pas modifier. Le sous-agent le vérifie avant de rendre.

## Définition de terminé

- Les vérifications listées ici passent, lancées par le sous-agent (en parallèle, des tests ciblés
  sur les fichiers touchés ; la suite complète tourne en CI).
- Chaque cas limite ci-dessus a un test nommé.
- Rien d'écrit hors des fichiers listés.
- Aucun commit.
- Rapport dans l'ordre demandé par la définition d'agent.

## Questions déjà tranchées

Ce que le sous-agent pourrait être tenté de redemander, avec la réponse. Évite qu'il s'arrête pour
une hésitation déjà levée.
