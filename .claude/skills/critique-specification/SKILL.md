---
name: critique-specification
description: Critique experte et contradictoire de la spécification de Vigie (docs/, catalogue, config/, formats du protocole, playbooks) — ce que diraient les meilleurs spécialistes (recherche anticorruption et statistique, qualité des données publiques, évaluation des modèles de langage, droit de la presse et des données, science ouverte, logiciel libre) et un adversaire de bonne foi qui chercherait la faille — et ce que l'arrivée de modèles bien plus capables casserait. Exhaustive dans la lecture, économe dans les propositions : rien n'est réécrit sans nécessité démontrée. À lancer à la main (/critique-specification) avant de déclarer une phase de la roadmap terminée, avant un changement de PROTOCOL_VERSION et obligatoirement avant le lancement public de la phase 1. Pas pour la cohérence code ↔ spécification (conformite-specification) ni pour la qualité du code (revue-de-code). Produit un rapport daté dans docs/critiques/ et des ADR proposés ; n'écrit jamais dans la spécification.
model: fable
effort: max
disable-model-invocation: true
---

# Critique de la spécification

La spécification est ce que les spécialistes liront avant de croire un seul signal, et ce qu'une
institution mise en cause lira pour faire tomber un rapport. Une faille de méthode trouvée après le
lancement coûte la crédibilité du projet entier (`docs/01`, engagement 3) ; trouvée avant, elle
coûte une révision. Cette passe cherche donc **tout ce qu'un relecteur exigeant reprocherait à la
spécification**, et ne propose de changer que ce qui doit l'être.

Deux exigences tirent en sens contraire, et la passe tient les deux :

- **Être impitoyable dans la lecture.** Chaque règle, chaque seuil, chaque format est lu comme le
  lirait un chercheur qui veut rejeter la méthode, ou une institution signalée qui veut faire tomber
  le rapport.
- **Être avare dans les propositions.** Une spécification meilleure n'est pas une spécification plus
  longue. Chaque règle ajoutée coûte du code, des tests, du temps de contributeur et une surface
  d'attaque ; le cœur doit rester lisible en une journée (`docs/07` §1). Une proposition ne survit
  que si elle change la justesse d'un signal, la vérifiabilité d'un rapport, le risque juridique ou
  la capacité d'un tiers à réimplémenter le protocole. Ce qui relève du goût ou du style n'est
  **pas** proposé.

Ce que cette passe ne fait pas : vérifier que le code applique la spécification
(`conformite-specification`), juger le code (`revue-de-code`), écrire dans `docs/`, `config/`,
`playbooks/` ou `connectors/`, trancher à la place de l'auteur, et modifier les sept règles de
`CLAUDE.md` (leur changement exige l'unanimité des mainteneurs et une consultation publique,
`docs/10` §2 : la passe peut seulement signaler une tension).

## Quand la lancer

- Avant de déclarer une phase de `docs/08` terminée, avant tout changement de `PROTOCOL_VERSION`
  ou de version du catalogue, et **obligatoirement avant le lancement public de la phase 1** (les
  premiers rapports réels publiés par leurs auteurs).
- Après l'ajout d'une famille de règles ou d'un type de source (PV, liens d'intérêts).

## Étape 0 — Vérifier le modèle

La passe est faite pour Claude Fable (alias `fable`) à l'effort maximal : c'est l'en-tête de ce
fichier pour la session, et un paramètre explicite pour chaque sous-agent. D'après la documentation
de Claude Code, l'en-tête est ignoré quand l'organisation exclut le modèle (`availableModels`), quand
le mode auto ne le prend pas en charge, et, pour l'effort, quand la variable
`CLAUDE_CODE_EFFORT_LEVEL` est définie.

- Vérifier la variable : `echo "${CLAUDE_CODE_EFFORT_LEVEL-non définie}"`. Définie à autre chose que
  `max` : s'arrêter et le dire à l'auteur.
- Le gros de la lecture (étapes 3 et 5) passe par des sous-agents lancés avec `model: "fable"` et
  `effort: "max"` explicitement ; cette compétence le demande, c'est l'instruction qui l'autorise.
  Si un lancement est refusé ou retombe sur un autre modèle, **s'arrêter** et le dire : ne pas faire
  la passe avec un modèle moindre sans l'accord écrit de l'auteur.
- Inscrire au rapport le modèle de la session tel qu'elle le connaît, et celui des sous-agents.

L'en-tête ne vaut que pour le tour en cours : tout le travail de l'étape 1 à l'étape 7 se fait dans
ce tour, sans s'arrêter pour poser de question.

## Procédure

1. **Partir d'un arbre propre**, sur une branche `docs/critique-AAAA-MM-JJ` tirée de `main`. Relever
   le commit, `PROTOCOL_VERSION`, la version du catalogue et les empreintes SHA-256 des fichiers lus
   à l'étape 2.

2. **Lire en entier, sans exception** — c'est la condition de tout le reste :
   - `CLAUDE.md`, `README.md`, `CONTRIBUTING.md` ;
   - tout `docs/` : `00` à `11`, `references.md`, et `docs/decisions/` (ADR acceptés et proposés,
     leurs options et leurs motifs) ;
   - `config/*.yaml`, `connectors/**/*.yaml`, `playbooks/**` (en-têtes, corps, modèles) ;
   - le rapport le plus récent de `docs/critiques/` s'il existe, celui de `docs/conformite/`,
     l'entrée la plus récente de `docs/DETTE.md`.

   Une passe précédente existe : ses constats acceptés ou refusés par l'auteur ne sont pas
   re-signalés, sauf argument nouveau ; ses constats ouverts sont repris. Sinon, c'est une première
   passe.

3. **Lire par lentilles, en parallèle et sans s'ancrer.** Lancer, dans un seul message, quatre
   sous-agents `general-purpose` (`model: "fable"`, `effort: "max"`), un par groupe de lentilles de
   `references/lentilles.md` :
   - A : validité des signaux et statistique ; qualité des données publiques ;
   - B : modèles de langage, ancrage et reproductibilité ; pérennité face aux modèles et aux sources
     futurs ;
   - C : usages, neutralité et effets ; lecteur hostile ;
   - D : droit et éthique ; ouverture, gouvernance et adoption du protocole ; faisabilité.

   Chaque brief donne : la liste des fichiers à lire en entier (étape 2), le texte de son groupe de
   lentilles, les règles de la passe (plus bas) et le format de constat de `references/rapport.md`.
   **Ne pas leur transmettre d'hypothèse** sur les failles : un relecteur à qui l'on souffle la
   réponse la confirme. Ils lisent, n'écrivent rien dans le dépôt, et rendent leurs constats au fil
   de l'eau dans leur réponse finale.

4. **Vérifier chaque constat soi-même**, un par un, avant de le garder. C'est l'étape qui empêche
   les erreurs, et elle ne se délègue pas :
   - **la citation existe**, mot pour mot, au fichier et à la section indiqués (`grep`) ;
   - **la spécification n'y répond pas déjà ailleurs** : chercher dans tout `docs/`, le catalogue et
     les en-têtes de `config/` les termes du constat et leurs synonymes. Une spécification répartie
     sur onze documents répond souvent dans `docs/06` à ce qu'on croit manquer dans `docs/02` : c'est
     le faux positif le plus fréquent ;
   - **un ADR ne l'a pas déjà tranché** : un constat qui rouvre un ADR accepté n'est gardé que s'il
     apporte un argument que ses options et son motif n'ont pas pesé, et il cite l'ADR ;
   - **la conséquence est réelle** : écrire le scénario concret (quelles données, quel signal ou quel
     rapport change, dans quel sens, qui peut l'exploiter). Pas de scénario, pas de constat ;
   - **les faits extérieurs sont sourcés** (règle 6 ci-dessous), ou retirés.

5. **Faire plaider la défense.** Lancer un sous-agent (`model: "fable"`, `effort: "max"`) qui reçoit
   les constats retenus et la spécification, avec une seule mission : pour chacun, défendre la
   spécification telle qu'elle est (le texte y répond-il, le coût de la correction dépasse-t-il le
   gain, une limite déclarée suffit-elle ?) et **relancer chaque calcul** en relisant ses hypothèses.
   Trancher chaque désaccord soi-même, sur pièces, et noter dans le rapport les constats que la
   défense a fait tomber, avec la raison, en une ligne.

6. **Trier et proposer** selon `references/rapport.md` : gravité, nécessité, coût, et pour chaque
   constat la **plus petite** réponse suffisante, dans cet ordre de préférence :
   1. rien, si la défense tient ;
   2. déclarer une limite — dans la section « Limites » du playbook concerné, dans le texte fixe
      « Ce que ce rapport est et n'est pas » (`docs/02` §4.1), ou dans `docs/09` §6 — quand corriger
      coûte plus que ce que la correction apporte et que le lecteur informé peut juger seul ;
   3. préciser un mot ou une phrase existante ;
   4. supprimer une règle, un paramètre ou une exigence qui n'apporte rien à la justesse (la sobriété
      est un argument : chaque degré de liberté retiré est une critique évitée) ;
   5. ajouter une règle, en dernier recours, avec son coût en code et en temps humain.

   Un texte proposé est prêt à relire : citation de l'ancien texte, nouveau texte entre guillemets,
   fichier et section. Il ne réécrit que la phrase en cause.

7. **Écrire** `docs/critiques/AAAA-MM-JJ.md` au gabarit de `references/rapport.md`, y compris le
   verdict « Prêt pour la suite ? ». Ajouter un ADR « proposé » dans `docs/decisions/` par question
   qui revient à l'auteur (bloquants et majeurs), au gabarit de la compétence `cloture-de-session`,
   options réelles et recommandation en premier. Commiter le rapport et les ADR, pousser, ouvrir une
   PR qui ne contient qu'eux ; fusion selon la règle de `HANDOFF.md`. **Rien d'autre ne change
   pendant la passe.**

8. **Après les décisions de l'auteur**, et dans des tours ultérieurs : texte de la révision dans une
   PR qui ne touche que la spécification, que l'auteur relit ; puis `conformite-specification` pour
   le code qui doit suivre.

## Règles de la passe

1. **Citer, jamais paraphraser.** Un constat porte la phrase exacte qu'il vise, avec son fichier et
   sa section. Un constat sur une absence le dit, et donne les termes cherchés en vain.
2. **Une conséquence, ou rien.** « Un spécialiste pourrait trouver cela discutable » n'est pas un
   constat. « Avec un seuil au 80e percentile de la strate, une commune sur cinq déclenche A1 par
   construction, et le rapport ne le dit pas » en est un, **si le calcul est fait et montré**.
3. **Calculer plutôt qu'estimer.** Une affirmation sur un taux de déclenchement, un effectif de
   strate, une probabilité de faux positif, un coût ou une durée se calcule (script jetable dans le
   répertoire temporaire de la session, jamais dans le dépôt) et le calcul figure au rapport, avec
   ses hypothèses. Sinon elle est formulée comme une question ouverte.
4. **Ne rien inventer.** Aucune institution, aucun marché, aucun chiffre de la commande publique,
   aucun article de loi, aucune décision de justice, aucune référence bibliographique de mémoire.
5. **Ne jamais trancher à la place de l'auteur.** Une question de méthode ouverte reçoit ses options
   réelles, leur coût et une recommandation ; la décision reste la sienne.
6. **Un fait extérieur est sourcé ou retiré.** Une pratique de référence, une norme, une étude, un
   texte juridique ne sont cités qu'après vérification en ligne, avec l'URL consultée et la date. Une
   source non vérifiée est retirée, pas gardée « à vérifier ». Une question juridique se formule comme
   une question **pour un professionnel du droit** (`docs/09`, en-tête), jamais comme un avis.
7. **Aucune institution nommée.** La critique porte sur la méthode ; un exemple est synthétique.
   Les espaces du projet ne discutent pas d'institutions nommées (`docs/10` §3).
8. **Ni style ni goût.** Pas de reformulation pour la clarté si la phrase actuelle n'admet qu'une
   lecture. Une phrase qui en admet deux est un constat (ambiguïté), avec les deux lectures.
9. **Exhaustif ne veut pas dire long.** Tout est lu ; seul ce qui survit à l'étape 4 et à la défense
   est écrit. Les mineurs tiennent en une ligne chacun.

## Prêt pour la suite ?

Le rapport conclut par un verdict, oui ou non, pour l'étape visée (fin de phase, nouvelle
`PROTOCOL_VERSION`, lancement public), sur ces critères :

- aucun constat **bloquant** ouvert ;
- chaque constat **majeur** tranché par l'auteur : corrigé, ou déclaré comme limite ;
- aucun ADR « proposé » qui touche `docs/02` à `docs/07` ou `docs/09` ;
- la dernière passe `conformite-specification` est postérieure à la dernière révision, et n'a aucune
  exigence `contredite` ;
- aucune mention **[À VÉRIFIER]** sur une valeur ou une affirmation dont dépend une règle, un
  connecteur ou un playbook de l'étape visée : `grep -rn "À VÉRIFIER" docs/ config/ connectors/
  playbooks/`, chaque occurrence classée « utilisée par l'étape » ou non ;
- pour passer en phase 2 : la relecture juridique de `docs/09` par un professionnel est faite
  (`docs/09`, en-tête ; `docs/08`, « Transversal »).

Un verdict négatif liste ce qui manque, dans l'ordre où le faire.

## Après la passe

Si la passe a payé quelque chose d'inattendu (une lentille qui a trouvé ce qu'une autre ratait, un
faux positif typique que l'étape 4 a dû écarter), l'ajouter à `LESSONS.md` et, s'il s'agit d'une
question de lecture, à `references/lentilles.md`.
