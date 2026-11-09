# Trier un constat

## Gravité

La même échelle que `docs/DETTE.md`, jugée à la même aune : *un rapport pourrait-il affirmer
quelque chose de faux, ou de non prouvé, sans que rien ne l'ait signalé ?*

- **haute** — le défaut peut produire un signal faux, une affirmation non ancrée, une preuve
  invérifiable ou une donnée de personne indue sans qu'aucun test, ni `verify`, ni `lint` ne le
  montre. Exemples : une valeur par défaut qui remplace une donnée manquante dans une règle, une
  erreur avalée dans un connecteur qui fait passer une source inaccessible pour vide, un extrait de
  preuve retouché avant empreinte, une sortie de modèle non validée qui entre dans un signal.
- **moyenne** — le défaut coûte cher à la prochaine modification ou trompe le relecteur : règle ou
  seuil défini deux fois, module à plusieurs responsabilités, commentaire faux, test lié à la
  plateforme ou au réseau.
- **basse** — friction de lecture : nom approximatif, fonction un peu longue, `cast` justifié mais
  non commenté.

## Corriger ou accepter

Un constat n'est pas toujours à corriger. **Accepter** est une décision légitime, à condition
d'écrire pourquoi dans le rapport : la revue suivante ne le re-signalera pas.

On accepte quand :
- le coût du changement dépasse ce qu'il protège (fixture longue mais linéaire) ;
- le signal est un faux positif (`.get(clé, [])` sur une liste facultative est l'idiome) ;
- la correction demande une décision de l'auteur hors du périmètre de la revue : on accepte *pour
  la revue* et on propose la décision (ADR « proposé » ou entrée de `docs/DETTE.md`).

On corrige toujours une gravité haute, ou on la transforme en entrée de `docs/DETTE.md` si la
correction dépasse une revue.

## Les faux constats fréquents

| Tentant d'écrire | Ce que c'est vraiment |
| --- | --- |
| « Ce fichier est long » | Un signal. Le constat est ce qui rend le fichier difficile : plusieurs responsabilités, état partagé. |
| « Il faudrait plus de commentaires » | Presque jamais. Le constat est ce que le code ne dit pas par ses noms ou sa structure. |
| « Ce n'est pas comme je l'aurais écrit » | Rien, sauf si la forme actuelle cache un défaut nommable. |
| « Ajouter une abstraction pour le futur » | Une anticipation. La revue retire de la complexité, elle n'en ajoute pas ; le cœur doit rester lisible en une journée. |
| « Cette règle donne trop de faux positifs » | Une question de calibration ou de catalogue (décision de l'auteur), pas un défaut de code. |
