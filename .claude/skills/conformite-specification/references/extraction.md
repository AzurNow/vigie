# Extraire, tracer, trier

## Ce qui est une exigence

Une exigence est une phrase de la spécification dont on peut dire, en lisant le code et les tests,
si elle est respectée. Le test : *peut-on imaginer un code qui la viole ?* Si non, ce n'est pas une
exigence.

| Est une exigence | N'en est pas une |
| --- | --- |
| « Un signal sans au moins une preuve […] est rejeté à la validation » (`CLAUDE.md`, règle 3) | « La vérifiabilité *est* le produit ; l'IA n'est que le moteur » (`docs/01`) |
| « Une règle ne fait pas d'I/O » (`docs/07` §5) | Un critère d'acceptation de phase (`docs/02` §10) — tracé dans la roadmap, pas ici |
| B2, « Calcul » : « signal si `somme >= seuil` et `max(individuel) < seuil` » (`docs/03`) | La « Littérature » d'une fiche (elle est copiée, pas exécutée) — mais sa copie conforme est une exigence |
| « Codes de sortie stables (0 ok, 2 validation, 3 vérification, 4 source indisponible) » (`docs/07` §11) | Une décision reportée (`docs/07` §16) |

Une phrase qui contient plusieurs obligations donne plusieurs exigences, une par obligation
vérifiable séparément. Les tableaux, les listes de sections obligatoires et les objets JSON
normatifs de `docs/06` sont des exigences ligne par ligne (un champ, un invariant).

Chaque fiche du catalogue donne au moins ces exigences : le **calcul** ; chaque **paramètre** lu dans
`config/` (et non écrit dans le code) ; la règle de **confiance** ; un test de **vrai positif** ; un
test de **faux positif connu** (`docs/03`, « Ajouter une règle ») ; la **copie conforme** des sections
« explications ordinaires », « vérifier ensuite » et « littérature » dans le rapport.

**Nature** de chaque exigence (champ `nature` de la matrice) :
- `code` — un comportement du code (une règle, l'ancrage, `verify`, un code de sortie) ;
- `donnee` — une forme ou un champ que les modèles pydantic de `vigie/protocol/` ou les validateurs
  de playbook et de connecteur doivent imposer ;
- `processus` — une étape humaine outillée (point de contrôle, marque, export de retour) : on trace
  l'outil qui la rend possible et le garde-fou qui empêche de la contourner ;
- `hors-code` — relecture juridique, financement, gouvernance : noté pour mémoire, jamais un
  constat.

## Identifiants

Stables, jamais réattribués :

| Source | Forme | Exemple |
| --- | --- | --- |
| Règles non négociables de `CLAUDE.md` | `R<n>.<rang>` | `R3.01` |
| « Ce que tu ne fais jamais » de `CLAUDE.md` | `J.<rang>` | `J.04` |
| Fiche du catalogue | `<code>.<rang>` | `B2.03` |
| Autre document normatif | `D<nn>§<section>.<rang>` | `D06§2.03` |
| ADR accepté | `ADR<nnnn>.<rang>` | `ADR0002.01` |

Le rang croît dans l'ordre du texte à la **première** extraction. Une exigence supprimée passe à
`retiree` et garde son identifiant ; une exigence insérée entre deux autres prend le rang libre
suivant de sa section. L'ordre des rangs cesse alors de suivre le texte, et c'est voulu : la
stabilité vaut plus que l'ordre. Le code d'une règle du catalogue n'étant jamais réattribué
(`docs/03`, en-tête), ses identifiants survivent à un retrait.

## Tracer

Pour chaque exigence de nature `code` ou `donnee` :
1. **Chercher les mots de la spécification** dans le code (`grep -rn` sur le terme, le code de
   règle, le nom du champ) puis **lire** l'endroit trouvé. Une docstring oriente, elle ne prouve
   rien.
2. **Noter chaque emplacement** qui applique l'exigence (`chemin:fonction`). Une exigence appliquée à
   deux endroits est un signal de règle dupliquée : le noter.
3. **Chercher le test** qui échouerait si l'exigence était violée. Le nommer par fichier et nom du
   test. Un modèle pydantic qui impose une forme, avec un test qui montre le refus d'une entrée qui
   la viole, compte comme test. Un rapport golden (`tests/golden/`) compte pour ce qu'il couvre
   réellement, pas pour tout.
4. **Comparer le détail**, pas seulement l'existence : bornes incluses ou exclues (`<` ou `<=` dans
   « juste sous un seuil »), fenêtres glissantes, seuil en vigueur à quelle date, HT ou TTC,
   arrondis, ordre de tri, traitement de l'absent. C'est là que vivent les contradictions.

## Sens inverse : les règles non écrites

Parcourir le code et relever chaque **règle qui change un signal, une preuve ou un rapport**, puis
chercher la phrase de la spécification qui la fonde. Si aucune ne la fonde, c'est un constat
« règle non écrite ». Familles à chercher systématiquement :
- **constantes** : tout nombre qui conditionne un signal doit venir de `config/` ; un nombre écrit
  dans le code est un constat même s'il est égal à la valeur de `config/` ;
- **ordres et départages** : tri des signaux au-delà de « sévérité × confiance, puis récence »
  (`docs/02` §4.1), départage des égalités, numérotation des identifiants `SIG-…`, ordre de lecture
  des documents, choix du doublon gardé (G4) ;
- **traitement de l'absent** : champ manquant, `nombre_offres_recues` absent, source inaccessible,
  vide ou lacunaire (`docs/02` §6), strate trop petite, rapprochement non trouvé — exclu, compté,
  signalé, confiance baissée ?
- **dates** : bornes des fenêtres et des périodes, seuil applicable à la date de notification (en-tête
  de `config/seuils.fr.yaml`), fuseau des horodatages ;
- **confiance et sévérité** : `docs/02` §4.3 décrit la confiance par ses facteurs ; toute table ou
  formule que le code en tire est une règle à écrire ou à accepter ;
- **paramètres d'outils externes** qui changent une donnée : extraction de texte PDF, OCR et sa
  version, normalisation avant comparaison (ancrage, relocalisation des extraits), seuil de recherche
  floue (`docs/07` §9, « seuil configurable ») ;
- **choix de modélisation** fixés dans `vigie/protocol/` ou `vigie/models/` sans phrase de la
  spécification ;
- **vocabulaire** : variantes et accords de `vigie/lint/vocabulaire.fr.yaml` au-delà de la liste de
  `docs/02` §4.2.

Une règle non écrite déjà consignée ailleurs (`docs/DETTE.md`, un ADR proposé) reste un constat tant
que la spécification ne l'écrit pas ou que l'auteur ne l'a pas acceptée par écrit — mais le rapport
cite l'endroit où elle est déjà connue.

## Trier un constat

Gravité, à l'aune de `docs/DETTE.md` : *un rapport pourrait-il affirmer quelque chose de faux, ou de
non prouvé, sans que rien ne l'ait signalé ?*

- **haute** — le code **contredit** une exigence qui touche un signal, une preuve, l'ancrage, le
  vocabulaire, `verify` ou les données de personnes ; ou applique une règle non écrite qui change un
  signal sans qu'un lecteur de la spécification puisse la deviner (une autre implémentation du
  protocole produirait d'autres signaux).
- **moyenne** — exigence de ce type implémentée **sans test** ; règle non écrite qui ne change un
  signal qu'en cas limite ; contradiction sur un processus outillé (point de contrôle, export).
- **basse** — écart de vocabulaire (le code et la spécification nomment différemment le même
  concept), référence de section périmée dans une docstring, exigence de donnée gardée par le code
  mais pas par le modèle pydantic.

Proposition, une seule par constat :
- **corriger le code** — la spécification est claire et le code s'en écarte ;
- **réviser la spécification** — le code a fixé une règle raisonnable que le texte n'écrit pas ou
  écrit autrement. Le rapport propose le texte ; pour un format du protocole, il dit que la révision
  incrémente `PROTOCOL_VERSION` (`docs/06` §7) ;
- **accepter** — l'écart est sans effet sur un signal ou un rapport, et l'écrire coûterait plus qu'il
  ne protège. Écrire pourquoi : la passe suivante ne le re-signale pas.

Le choix final appartient à l'auteur. La passe propose, elle ne tranche pas.

## Faux constats fréquents

| Tentant d'écrire | Ce que c'est vraiment |
| --- | --- |
| « La règle E3 n'est pas implémentée » | Rien : `a-implementer`, phase 2 de `docs/08`. |
| « La spécification devrait plutôt dire… » | Un avis de méthode, pour `critique-specification`. Hors périmètre, sauf si le code a déjà tranché autrement. |
| « Ce seuil est encore [À VÉRIFIER] » | Une tâche connue, sauf si le code l'a codé en dur ou s'en sert sans la mention dans le rapport. |
| « Cette fonction ne cite pas sa section » | Un défaut de lisibilité, pour `revue-de-code`. |
| « L'exigence est implémentée » sans fichier ni fonction | Un souvenir. Aller lire. |
