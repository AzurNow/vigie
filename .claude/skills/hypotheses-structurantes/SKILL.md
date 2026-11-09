---
name: hypotheses-structurantes
description: Examen des hypothèses structurantes de Vigie — la thèse, les engagements, les utilisateurs visés, les mesures de succès, la tête de pont, la forme (protocole + CLI locale), le modèle de dispersion, l'ordre des phases, les règles 4 à 7 de CLAUDE.md et les ADR acceptés — traitées comme des paris falsifiables sur le monde, pas comme un étalon. Chaque hypothèse reçoit les faits que le web et les données ouvertes fournissent (calculs, usages révélés, littérature, analogues, tout sourcé et daté), ce qui la falsifierait, le test le moins cher avant d'écrire du code, et la meilleure alternative. Ne remplace ni critique-specification (qui mesure la spécification à cet étalon) ni conformite-specification. Les règles 1, 2 et 3 sont hors champ. À lancer à la main (/hypotheses-structurantes) : avant la première critique de la spécification et la première ligne de code ; après les premiers rapports réels ; avant chaque nouveau domaine ou juridiction ; quand un pari assumé rencontre la réalité. Tient un registre dans docs/fondations/ ; ne réécrit rien ; chaque décision revient à l'auteur en ADR.
model: fable
effort: max
disable-model-invocation: true
---

# Hypothèses structurantes

La spécification repose sur des énoncés qu'aucune relecture ne vérifie : « un citoyen auditera sa
commune en une soirée », « le modèle OpenStreetMap attirera des contributeurs », « être observé
dissuade », « les DECP suffisent pour une commune », « la première forme est une CLI locale ». Ce
sont des **paris sur le monde**. `critique-specification` les prend pour étalon et mesure le reste
contre eux ; cette passe les prend pour **hypothèses** et cherche ce que le monde en sait déjà.
Tant que rien n'est codé, en changer une coûte une phrase ; après, elle coûte ce qui a été construit
dessus. C'est pourquoi la passe tourne avant la première critique et la première ligne de code, puis
chaque fois que la réalité a parlé.

La posture est générative mais disciplinée. Une hypothèse est **une phrase de la spécification, le
mécanisme qu'elle suppose, et ce qui la falsifierait** ; « il faudrait réfléchir à » n'est pas une
hypothèse. Un agent apporte des **faits** — un calcul, une source datée — et pas des opinions ; une
opinion s'écrit « pari », jamais « constat ». Le but de la passe est l'impact du projet (`docs/01`,
« La thèse ») : une hypothèse ne mérite le registre que si sa chute changerait la forme du projet,
l'ordre de ses utilisateurs, sa tête de pont, son modèle de croissance, sa gouvernance ou une règle 4
à 7. Douze hypothèses au plus : au-delà, ce sont des détails, et ils appartiennent à
`critique-specification`.

## Hors champ

- **Les règles 1, 2 et 3 de `CLAUDE.md`** (jamais d'accusation ; institutions, jamais individus ;
  pas de preuve, pas de signal). Décision de l'auteur du 2026-11-09 : aucun gain d'impact ne compense
  leur perte, parce qu'elles sont ce qui sépare le projet d'« une machine à harcèlement »
  (`docs/01`). Une hypothèse qui exigerait d'en affaiblir une s'écrit **impossible**, pas « option ».
- Le code, la cohérence code ↔ spécification (`conformite-specification`), la qualité de la
  spécification à étalon donné (`critique-specification`).
- Toute institution nommée (`docs/10` §3) : calculs en agrégats, exemples synthétiques, et rien de
  repris sur une institution réelle, même tirée d'une source publique.

## Les preuves admises, par force décroissante

1. **Calcul sur données ouvertes.** Ce que les données peuvent réellement produire : effectifs,
   couvertures, densités, distributions, par strate. Script jetable dans le répertoire temporaire de
   la session, hypothèses écrites, résultat et script recopiés dans la passe. Données téléchargées
   depuis les portails institutionnels de `docs/11`, dans leurs conditions d'usage, hors du dépôt,
   supprimées après.
2. **Usage révélé.** Ce que des gens ont *déjà fait* de ces données et de ces outils : articles qui
   les exploitent, rapports d'associations, rapports de chambres des comptes, dépôts de code
   (activité, dernier commit, contributeurs, issues), programmes de financement, campagnes,
   post-mortems de projets éteints. Ce que des gens *disent* qu'ils feraient n'en fait pas partie.
3. **Littérature.** Ce qui est établi sur l'effet de l'observation de la commande publique et sur la
   qualité des données ; une étude citée est lue, pas déduite de son titre.
4. **Analogues.** D'autres pays, d'autres domaines ; chaque analogie dit où elle casse.
5. **Pari.** Le raisonnement seul. Admis, écrit comme tel, jamais déguisé.

Tout fait extérieur porte son URL et sa date de consultation ou disparaît. `docs/11` exclut la
presse et les forums comme sources *du produit* ; ici ce sont des traces d'usage, pas des preuves
d'un signal, et rien n'y est repris sur une institution nommée.

**Le biais, écrit dans chaque passe.** Le web sur-représente ceux qui écrivent : techniciens,
journalistes, associations équipées. Le citoyen qui ne publie rien n'y laisse rien. L'absence de
trace n'est pas une absence de besoin ; la présence d'articles n'est pas une volonté d'installer un
outil. Le registre dit, pour chaque utilisateur visé de `docs/01`, s'il est observé directement, par
analogie, ou pas du tout.

**Ce que le web ne dira jamais.** Le comportement de quelqu'un face à un produit qui n'existe pas
encore. Pour les hypothèses de comportement (« il installera », « il agira », « il contribuera »),
la passe ne conclut pas : elle prépare un **pilote** (étape 6) que seul l'auteur peut exécuter,
parce que le projet ne publie rien et que la livraison exige un humain (`docs/02` §5). Le résultat
d'un pilote est la preuve la plus forte du registre.

## Étape 0 — Vérifier le modèle

Mêmes vérifications que `critique-specification`, étape 0 : `CLAUDE_CODE_EFFORT_LEVEL` non définie
ou `max`, sous-agents lancés avec `model: "fable"` et `effort: "max"` explicites, arrêt si l'un
retombe sur un autre modèle, modèles inscrits à la passe. L'en-tête ne vaut que pour le tour : tout
se fait dans ce tour, et tout ce qui est formé s'écrit au fil de l'eau dans le répertoire
temporaire.

## Procédure

1. **Arbre propre**, branche `docs/fondations-AAAA-MM-JJ` tirée de `main`. Relever le commit. Lire
   en entier : `CLAUDE.md`, `docs/01`, `docs/02`, `docs/08`, `docs/10`, `docs/11`, `docs/decisions/`,
   `README.md`, `CONTRIBUTING.md`, le registre `docs/fondations/registre.md` et la passe la plus
   récente de `docs/fondations/` s'ils existent, la critique la plus récente de `docs/critiques/` si
   elle existe (ses tensions sont des candidates). Le reste de `docs/` se lit quand une hypothèse y
   renvoie.

2. **Extraire les hypothèses**, dans la session, sans sous-agent : c'est de la lecture de la
   spécification, pas du monde. Pour chacune : la phrase exacte, le mécanisme supposé (par quel
   chemin la phrase produit l'impact qu'elle promet), ce qui la falsifierait, et ce qui changerait
   dans le projet si elle tombait. Les pistes de `references/pistes.md` sont un point de départ ;
   une hypothèse qu'aucune piste ne prévoit reste une hypothèse. Une passe suivante repart du
   registre : les hypothèses tranchées par l'auteur ne reviennent pas sans fait nouveau.

3. **Deux sous-agents, en un seul message**, `general-purpose`, `model: "fable"`, `effort: "max"`,
   chacun avec un fichier de résultats dans le répertoire temporaire qu'il complète au fil de l'eau :
   - **Relevé du monde** : reçoit les hypothèses sous forme de **questions**, jamais avec la réponse
     attendue, et les preuves admises 2 à 4. Pour chacune : ce qui existe (outils, projets, usages,
     leurs signes d'adoption et leur état), ce qui a échoué et pourquoi, ce que la littérature
     établit, les analogues et où ils cassent. URL et date pour tout ; rien de mémoire ; aucune
     institution nommée dans ses conclusions.
   - **Calculs** : reçoit les hypothèses quantifiables (couverture, densité, effectifs, coûts) et les
     portails de `docs/11`. Pour chacune : les données, le script, les hypothèses de calcul, le
     résultat par strate, et ce que le résultat permet ou non de conclure. Agrégats seulement.
   Pendant qu'ils travaillent, la session fait l'étape 4 sur ce qui ne dépend pas d'eux.

4. **Vérifier soi-même** : chaque URL est ouverte et dit ce qu'on lui fait dire ; chaque calcul est
   relancé après relecture de ses hypothèses ; chaque fait a une date. Avant de changer le statut
   d'une hypothèse, écrire **l'argument contraire le plus fort** et y répondre sur pièces ; s'il
   tient, le statut ne change pas. Un fait non vérifié est retiré, pas gardé « à vérifier ».

5. **Statuer et proposer**, selon `references/registre.md`. Les statuts : **confirmée** (faits pour,
   rien contre), **infirmée** (faits contre), **à tester** (le test, son coût, qui l'exécute — agent
   ou auteur — et ce qui compterait comme falsification), **pari assumé** (aucune preuve possible
   avant la réalité ; l'auteur choisit de parier et l'écrit). Pour chaque hypothèse infirmée ou à
   tester : **une seule alternative**, la meilleure, avec la mesure de succès de `docs/01` qu'elle
   sert, son impact attendu, son coût, et la recommandation en premier (`HANDOFF.md`, « Façon de
   travailler »). Pas de liste d'options pour se couvrir.

6. **Préparer les pilotes**, pour les hypothèses de comportement que le web ne tranche pas. Un pilote
   est une procédure que l'auteur exécute seul, hors du dépôt : l'audit à la main d'une institution
   réelle avec le playbook et les données brutes, un rapport remis à une seule personne qui peut
   agir, une fiche de commune confiée à un contributeur. La passe écrit pour chaque pilote :
   l'hypothèse testée, la procédure pas à pas, ce qu'il faut observer, le résultat qui falsifierait,
   le coût en heures. Le résultat revient au registre sous forme d'agrégat ou de « oui / non », sans
   jamais nommer l'institution ni la personne.

7. **Écrire** : le registre `docs/fondations/registre.md` (vivant, une entrée par hypothèse, statut
   et date de dernière mise à jour) et la passe datée `docs/fondations/AAAA-MM-JJ.md` (ce qui a
   changé, les faits, les calculs, le biais, les pilotes), au gabarit de `references/registre.md`.
   Une décision qui revient à l'auteur — ordre des utilisateurs, tête de pont, forme, règle 4 à 7,
   révision d'un ADR accepté — devient un ADR « proposé » dans `docs/decisions/`, au gabarit de la
   compétence `cloture-de-session` ; une règle 4 à 7 y porte le rappel de `docs/10` §2. Commiter,
   pousser, ouvrir une PR qui ne contient que cela ; fusion selon `HANDOFF.md`. **Rien d'autre ne
   change pendant la passe** : ni `docs/01`, ni une règle, ni la roadmap.

8. **Après les décisions de l'auteur**, dans des tours ultérieurs : révision de `docs/01`, `docs/08`
   ou d'un ADR dans une PR à part qu'il relit ; puis `critique-specification` sur le nouvel étalon.

## Règles de la passe

1. **Une phrase, un mécanisme, une falsification.** Sans les trois, ce n'est pas une hypothèse et
   elle n'entre pas au registre.
2. **Des faits, pas des avis.** Un énoncé sur le monde porte sa preuve (1 à 4) et sa date, ou s'écrit
   « pari ». Le modèle ne sait rien du monde d'aujourd'hui de mémoire.
3. **Calculer plutôt qu'estimer**, et montrer le calcul.
4. **Le biais est écrit**, pour chaque passe et pour chaque utilisateur visé.
5. **Une alternative, la meilleure**, par hypothèse infirmée ou à tester ; jamais de réécriture de
   `docs/01` dans la passe.
6. **Les règles 1 à 3 ne se discutent pas**, et une hypothèse qui en aurait besoin s'écrit impossible.
7. **Aucune institution ni personne nommée**, ni dans le registre, ni dans les passes, ni dans les
   fichiers des sous-agents.
8. **Ne jamais trancher à la place de l'auteur.** La passe recommande ; il décide.
9. **Court.** Douze hypothèses au plus ; une entrée tient en une page ; une passe se lit en vingt
   minutes.

## Quand la lancer

- **Avant la première `critique-specification` et la première ligne de code** : polir un cadre
  avant d'avoir demandé s'il est le bon, c'est travailler pour rien.
- **Après les premiers rapports réels** et après chaque pilote : la réalité a parlé, les statuts
  bougent.
- **Avant chaque nouveau domaine ou juridiction** (PV, subventions, autres collectivités,
  international) : la tête de pont suivante est une hypothèse comme la première.
- **Quand un pari assumé rencontre un fait** : une passe courte, sur cette hypothèse seule.

## Après la passe

Ce que la passe a payé d'inattendu (une hypothèse que le calcul a renversée, une source qui disait
le contraire de son titre, un pilote qui a mesuré autre chose que ce qu'il visait) va dans
`LESSONS.md` ; une question de lecture qui manquait va dans `references/pistes.md`.
