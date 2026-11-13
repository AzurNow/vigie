# Bonnes pratiques — Vigie

Ce qui a été appris en travaillant sur ce dépôt, et qu'il serait coûteux de réapprendre. Complété au
fil des sessions (compétence `cloture-de-session`). Une leçon n'entre ici que si elle a été
**payée** : une décision prise, un piège évité, une erreur commise.

`CLAUDE.md` dit comment travailler ; ce fichier dit ce que l'expérience a ajouté.

Les leçons marquées **(Banc)** ont été payées sur le Banc d'essai 2027, un projet voisin du même
auteur (audit reproductible de ce que les assistants d'IA disent des candidats à une élection),
avant la création de Vigie. Leur cas concret parle de ce projet-là ; elles sont reprises parce que le
même piège existe ici, ce que dit la dernière phrase de chacune.

---

## Modélisation des données

**Une donnée absente reste absente.** (Banc) Pas de `or 0`, pas de `"inconnu"`, pas de recopie du
modèle demandé dans le champ du modèle renvoyé quand l'API ne renvoie rien : `None` est une
information, une valeur plausible est un mensonge. Ici, l'absence est déjà une règle de la
spécification (`docs/02` §6) : elle baisse la confiance ou produit un signal G, elle n'est jamais
comblée.

**Un identifiant ne doit rien signifier.** (Banc) Un identifiant d'item qui encodait le candidat et
le thème devenait faux dès qu'un annotateur corrigeait le thème. Identifiants opaques, champs
séparés. Ici : un identifiant de signal ou de document qui encoderait une institution ou une règle
deviendrait faux à la première correction.

**Ne jamais stocker deux fois une valeur qui peut diverger.** (Banc) Deux dates décrivant le même
instant ont fini par ne plus coïncider ; un champ unique a réglé le problème. Ici : une valeur de
`constat.valeurs` recopiée au lieu d'être tirée de la preuve finit par s'en écarter.

**Épingler la version de ce contre quoi on a jugé.** (Banc) Git dit *quand* une donnée a changé,
jamais *contre quelle version* un résultat a été produit. Ici, le rapport porte déjà les versions du
protocole, du catalogue, des règles et l'empreinte de la configuration (`docs/06` §1) : ne jamais en
retirer une.

**Les bornes temporelles se déclarent.** (Banc) Intervalles semi-ouverts partout (`debut <= t <
fin`), un instant de référence unique, un décalage horaire explicite sur chaque horodatage. Ici :
fenêtres glissantes (B2), seuils datés (`config/seuils.fr.yaml`), périodes d'audit.

**Un offset n'existe pas sans son unité.** (Banc) Des positions comptées en points de code puis
cherchées avec une fonction qui compte en unités UTF-16 décalaient tout le surlignage d'un caractère
après le premier emoji ou idéogramme — sans erreur ni test rouge. Ici : `offset_debut` et
`offset_fin` des preuves (`docs/06` §1) disent leur unité, et `verify` relocalise dans la même.

**Compter un dénominateur sur le plan, jamais sur les fichiers présents.** (Banc) Une réponse reçue
mais non stockée sortait du dénominateur des manquantes, donc d'un seuil de qualité, sans un message.
Ici : une part de marchés ou une complétude se calcule sur ce qui devait être examiné, pas sur ce qui
a été téléchargé.

## Tests

**Les entrées invalides sont les vrais tests.** (Banc) Un exemple valide vérifie qu'on n'a rien
oublié ; un exemple invalide vérifie qu'une règle est réellement gardée. Chacun cible une règle
nommée. Ici : les faux positifs connus du catalogue jouent ce rôle pour les règles.

**Vérifier qu'un test échoue pour la bonne raison.** (Banc) Un exemple invalide peut tomber par
accident (une virgule, un champ oublié) et sembler valider une règle qu'il n'atteint jamais ; la règle
visée ne produisait que la deuxième erreur du lot. Lister **toutes** les erreurs de validation.

**Faire échouer volontairement le test qui protège une propriété critique.** (Banc) Un test
d'aveuglement écrit d'un trait passait — ce qui ne prouvait rien ; une fuite délibérée a montré qu'il
tombait et nommait la route fautive. Ici : le linter de vocabulaire, l'ancrage et `verify` se
prouvent en y glissant une faute.

**Un test qui ne construit que le cas favorable passe par hasard.** (Banc) Un test de filtre qui
n'utilisait que des cas retenus est resté vert alors que le résultat attendu en contenait d'autres.
Pour une règle qui filtre, construire au moins un cas que le filtre doit écarter.

**Tester une bibliothèque sur l'entrée qu'elle devrait refuser.** (Banc) Une bibliothèque PDF
ignorait le type demandé quand elle reconnaissait du HTML, et un nom de codage normalisé ne valait pas
celui qu'on comparait : deux défauts muets, trouvés par des tests de refus, aucun par la
documentation. Ici : extraction PDF et OCR des PV (phase 2), décodage des pages HTML.

**Tester une correspondance dans les deux sens.** (Banc) Une fonction censée distinguer « tout ce que
lisent les métriques » ne le faisait que dans un sens ; le test qui faisait varier chaque champ un à
un a trouvé quatre écarts. Ici : rejouer une règle doit donner les mêmes signaux, et des signaux
différents doivent venir d'entrées différentes.

**Un exemple normatif écrit dans la spécification se lit par un test.** (Banc) Une instance JSON du
protocole « valide » a cessé de l'être sans qu'aucun test ne tombe pendant quatre jours. Ici : les
objets JSON de `docs/06` §1 se valident contre les schémas de `vigie/protocol/`.

**Après avoir fusionné `main` dans une branche, relancer toutes les vérifications, pas seulement les
tests du lot.** (Banc) Deux branches vertes séparément ont donné un exemple invalide une fois
réunies : un champ rendu obligatoire par l'une, un exemple neuf sans ce champ dans l'autre.

## Outillage

**Fixer les fins de ligne dans le dépôt, pas dans la configuration de chacun.** (Banc) Sans
`.gitattributes`, Git pour Windows convertissait les fichiers de référence en CRLF : des tests
échouaient sous Windows alors que la CI Linux était verte, et les empreintes et offsets des textes
auraient changé. Ici, le store hache des fichiers et vise Windows : `eol=lf` dès `pyproject.toml`.

**Traiter « fichier binaire » sur un source comme une alarme.** (Banc) Un NUL brut dans un source,
invisible à l'écran, faisait classer le fichier comme binaire par Git : ses diffs disparaissaient des
PR. La revue de code relève ces fichiers (`revue-de-code`, « Signaux mécaniques »).

**Écrire les caractères invisibles par échappement, puis vérifier les octets.** (Banc) L'outil
d'écriture de l'agent a transformé un accent combinant échappé en caractère brut : `été` composé et
décomposé ne se distinguent pas à l'œil. Ici : tests de normalisation du linter et de l'ancrage.

**Vérifier une fixture par ses propriétés, pas à l'œil.** (Banc) Un identifiant sans largeur fixe
faisait collisionner l'item 1 et l'item 10 (dix écrits, neuf sur le disque), et une date prise à
l'heure courante rendait le jeu non reproductible. Compter les fichiers produits et comparer deux
exécutions.

**Les scripts jetables vivent hors du dépôt.** (Banc) Génération et validation ponctuelles : dans le
répertoire temporaire de la session, jamais dans le dépôt.

**Ce qui doit être relu plus tard va dans le dépôt ; le répertoire temporaire ne survit pas à la
session.** La première passe `hypotheses-structurantes` promettait que les faits relevés mais non
vérifiés « pourront être vérifiés à la prochaine passe » depuis ses fichiers de travail — qui
disparaissent avec la session. Corrigé avant fusion : ce qui compte est dans la passe (titres des
sources), le reste n'existe plus et on le dit.

**Vérifier le réseau du shell à l'étape 0 d'une passe qui doit calculer, et écrire d'avance ce
qu'elle fera sans.** Le sous-agent « calculs » de la première passe du registre devait télécharger les
DECP consolidées ; le réseau du shell était coupé et il a dû se rabattre sur des agrégats publiés et une
estimation sous hypothèses. Le résultat est utilisable parce qu'il est étiqueté, mais la compétence ne
l'avait pas prévu (`docs/DETTE.md`, 2026-11-09, point 2).

**Un site qui refuse les accès automatisés se remplace par une autre source, il ne se contourne
pas.** Pour les seuils du pilote A, le site de la DAJ répondait 403 ; une requête avec un en-tête de
navigateur a été refusée par le mode auto (« Bot Check Circumvention »), conformément à `CLAUDE.md`.
Les pages d'articles de marche-public.fr, qui reprennent Légifrance, ont suffi, étiquetées comme
source secondaire. Ne pas non plus deviner des URL : un sous-agent a perdu son tour sur un chemin
inventé (404) ; partir des liens d'une page qui répond.

**Ne pas écrire de contenu accentué multi-ligne par heredoc dans le shell de l'agent.** (Banc) Sur
la machine de l'auteur, l'enveloppe shell de l'agent digérait mal un heredoc avec accents et
guillemets (`unexpected EOF`). Utiliser l'outil d'écriture de fichier.

**Ne jamais faire comparer des agents parallèles par `git stash`.** (Banc) La pile de stash est
commune au dépôt et à tous ses worktrees : un `stash pop` peut ramener le diff d'un autre lot dans le
sien. Comparer par un commit temporaire du worktree.

**Exclure `.claude/worktrees/` des outils.** (Banc) Les worktrees des sous-agents vivent dans le
dépôt : le linter y a signalé des fonctions déjà corrigées, depuis des copies périmées. Git les
ignore ici (`.gitignore`) ; vérifier que pytest et les autres outils ne les parcourent pas.

**Avant de retirer un worktree, comparer ses fichiers non suivis à ceux du dépôt, pas leurs noms.**
(Banc) Un fichier du même nom portait une autre version ; un `--force` sur la foi du nom l'aurait
perdu. `git status --porcelain`, puis `cmp` fichier par fichier, et archiver plutôt que supprimer.

**Mettre à jour une PR en conflit par un commit de fusion, pas par rebase.** (Banc) Le mode auto a
refusé le `push --force-with-lease` d'une branche rebasée ; une fusion de `main` donne un push
ordinaire.

## Méthode

**Ne pas nommer une institution, c'est aussi ne pas la rendre identifiable.** Le compte rendu du
pilote A (2026-11-13) ne nommait aucune commune, mais citait des montants au centime, des populations
exactes, un profil d'acheteur départemental et la graine du tirage : chacun suffisait à retrouver la
commune dans le consolidé public. Vu à la clôture, après deux commits poussés sur un dépôt public.
Dans tout ce qui remonte au dépôt sur une institution réelle : montants en tranches, dates à l'année,
populations en strates, aucune graine ni source qui localise.

**Vérifier une agrégation en recomptant à la main un groupe qu'on connaît.** La règle B2 du pilote A
groupait la fenêtre glissante sans l'identifiant du marché d'ancrage : trois lots notifiés le même
jour comptaient chacun trois fois (599 194 € au lieu de 199 731 €). L'écart s'est vu parce que la
somme des trois lots était connue ; sans ce recomptage, B2 aurait affiché des sommes triplées.

**Lire `state = MERGED` avant toute suppression de branche, et ne rien chaîner derrière une fusion
avec `;`.** (Banc) `gh pr checks --watch` est sorti en code 0 sur une erreur réseau ; la fusion a été
refusée, mais la chaîne a continué et supprimé la branche, ce qui a fermé la PR.

**Avant chaque commit, `git branch --show-current`, et ne pas lancer en arrière-plan ce qui change de
branche.** (Banc) Une attente de CI puis de fusion lancée avant un `/clear` a basculé l'arbre sur
`main` sous les pieds de la session suivante.

**Une autorisation en mémoire ne lève pas une garde de l'outil.** (Banc) La fusion des PR a été
refusée par le contrôle de Claude Code malgré l'autorisation de l'auteur. Ne pas contourner : le dire.
Payé de nouveau ici le 2026-11-08 : l'écriture de `.claude/settings.json` a été refusée
(auto-modification) ; le contenu a été remis à l'auteur.

**Quand une règle change, la corriger partout où elle est écrite.** (Banc) Une même règle a vécu
trois états à la fois dans trois fichiers. Un fichier qui dit qu'un autre est périmé n'est pas une
correction : on corrige l'autre, et la mémoire locale avec.

**Clôturer chaque session, et écrire dans chaque PR ce qui pourrait casser.** (Banc) Seize PR
fusionnées sans clôture ont obligé à trier 21 000 lignes de diff, et un biais introduit entre-temps
n'est apparu qu'à ce moment-là.

**Porter le jour même une question qu'une PR laisse ouverte.** (Banc) Des questions posées dans le
corps de PR fusionnées sont restées sans réponse des semaines, alors que le code appliquait déjà une
option contraire au texte. Ici, elle devient un ADR « proposé » dans `docs/decisions/`.

**Une dette marquée « réglée » se vérifie dans le code, pas dans le récit.** (Banc) Un point barré
« jamais implémenté » visait une fonction qui existait depuis deux jours. `grep`, puis lire.

**Chercher la règle dans la spécification avant de la poser à l'auteur.** (Banc) Une question posée
alors que le texte fusionné deux jours plus tôt y répondait a reçu une réponse contraire au texte.

**Une réponse de l'auteur ne vaut pas autorisation d'écrire la spécification.** (Banc) Préparer le
texte exact, et ne l'écrire que dans une PR à part que l'auteur relit (`CLAUDE.md`, « Où l'agent
s'arrête »).

**Un trou de la spécification se signale, il ne se comble pas en silence.** (Banc) Les
contradictions relevées ont été documentées comme candidates à un amendement, pas résolues
d'autorité dans le code.

**Une passe de conformité trouve ce qu'aucune revue de code ne voit.** (Banc) La revue lisait la
qualité ; la passe de conformité, le code contre le texte : neuf écarts graves derrière des tests
verts. Lancer `conformite-specification` après tout lot qui touche une règle ou la chaîne de preuve.

**Faire relire les calculs d'une critique par une défense, pas seulement les citations.** (Banc)
Toutes les citations étaient exactes, mais trois chiffres ne tenaient pas ; la défense a relancé
chaque script et ramené quinze constats majeurs au rang de mineurs.

**Chiffrer le volume avant de choisir le stockage.** (Banc) « Tout dans Git » tenait jusqu'à ce qu'on
multiplie : le dépôt cessait d'être clonable, donc vérifiable. Ici : DECP nationales (plusieurs
millions de lignes), store et résumés de strates publiés dans le dépôt (`docs/07` §7 et §8).

**Relire le texte qui fait autorité plutôt que le résumé qu'on en a fait.** (Banc) Deux exigences
distinctes, fusionnées à tort dans un résumé, ont failli imposer une architecture que le texte
n'exigeait pas.

**Une contrainte qui s'applique en écrivant va dans `CLAUDE.md` ; une compétence ne porte que le
comment.** (Banc) Le seuil de complexité doit être présent à chaque fonction écrite : d'où la règle
dans `CLAUDE.md` et la façon de découper dans `complexite-maitrisee`.

**Un garde-fou qu'on n'a pas vu attraper quelque chose ne garde rien.** (Banc) Un contrôle de types
ajouté pour interdire une dépendance ne tombait pas quand on l'enfreignait exprès : il a fallu le
corriger pour qu'il garde.

**Durcir un parseur, c'est distinguer l'entrée malformée de l'entrée légale qu'on n'avait pas vue.**
(Banc) Le parseur durci refusait aussi des entrées parfaitement légales. Ici : formats DECP, dates,
montants.

**Une compétence importée garde le but du projet d'où elle vient : relire son critère de survie des
propositions contre les mesures de succès de ce projet.** `critique-specification`, importée du Banc
(dont l'objet est la reproductibilité d'un audit), ne gardait une proposition que si elle changeait
la justesse, la vérifiabilité, le risque juridique ou la réimplémentabilité ; aucun critère ne
nommait le lecteur ni l'action attendue de lui, et son échelle de réponse n'avait pas de barreau
« re-prioriser ». Une spécification pouvait en sortir avec le verdict « Oui » et ne servir à
personne. Pour chaque compétence importée : chercher les mots de `docs/01` (« Mesures de succès »,
« Utilisateurs cibles ») dans son texte ; s'ils n'y sont pas, c'est elle qu'il faut adapter.

### Sous-agents

**Rouvrir chaque source qu'un sous-agent cite avant de s'en servir : la citation peut dire le
contraire de la source.** Première passe `hypotheses-structurantes` : sur 55 faits du relevé du
monde, 9 ne tenaient pas à la relecture par la session — dont « DoZorro n'utilise pas d'IA pour les
red flags » alors que la page citée dit qu'il a été « le premier à tester l'apprentissage
automatique », et des limites d'une base *traitée* présentées comme la qualité des données brutes.
Non rouverts, ces faits auraient porté des statuts du registre. Les sources non rouvertes n'entrent
pas au registre, même plausibles.

**Un brief qui liste ses cas limites en liste fermée et ses questions déjà tranchées revient sans
question.** (Banc) 28 cas limites numérotés, sept hésitations réglées d'avance : rapport sans question
ouverte, tests verts du premier coup, diff dans le périmètre.

**Écrire les cas limites d'un brief à partir du texte de la décision, pas de mémoire.** (Banc) Un
brief écrit de mémoire demandait l'inverse de la décision ; seul un agent prudent l'a vu.

**Suivre une règle jusqu'à tous ceux qui la lisent.** (Banc) Une règle changée dans un module restait
appliquée à l'ancienne dans deux autres. Avant de briefer, `grep` chaque lecteur et les mettre tous
dans le périmètre.

**Un brief ne contredit pas la définition de l'agent qui le reçoit.** (Banc) Un brief qui demandait un
commit, interdit par la définition de l'agent, a coûté un aller-retour. Partir de
`.claude/briefs/GABARIT.md`, et commiter soi-même après relecture.

**Vérifier qu'un sous-agent n'a pas affaibli une assertion se lit dans le diff, pas dans son
rapport.** (Banc) Un `git diff` filtré sur les lignes d'assertion montre en une commande qu'aucune
attente n'a bougé.

**Un sous-lot lancé s'inscrit hors de la conversation.** (Banc) Coupé par la limite d'usage, un
sous-lot dont le nom, le brief et le worktree ne vivaient que dans la conversation était introuvable
après un `/clear`. Brief en fichier sous `.claude/briefs/`, et ligne dans « État » de `HANDOFF.md`.

**Un sous-agent long écrit ses livrables au fil de l'eau.** (Banc) Trois sous-agents coupés après une
heure de lecture n'avaient rien écrit. Le brief dit d'écrire dès qu'il y a assez de matière.

**En parallèle, des tests ciblés par agent, la suite complète en CI.** (Banc) Quatre agents qui
lançaient chacun la suite complète ont saturé la machine sans qu'aucune n'aille au bout.

**Relire le plan contre les briefs avant de les lancer.** (Banc) Un constat annoncé dans le plan ne
figurait dans aucun des cinq briefs envoyés.

**Ne pas fusionner une PR dont la base est la branche d'une autre PR.** (Banc) Fusionnées dans
l'ordre, des PR empilées ont atterri dans leur branche parente, jamais dans `main`. Rebaser une PR
empilée sur `main`, ou une seule PR depuis le sommet.
