# Index de la documentation

| # | Fichier | Nature | À lire quand |
|---|---------|--------|--------------|
| 01 | `01-vision-et-principes.md` | Métier | Toujours, en premier |
| 02 | `02-specification-metier.md` | Métier | Toujours |
| 03 | `03-catalogue-red-flags-marches-publics.md` | Métier | Implémentation ou revue d'une règle |
| 04 | `04-format-playbook.md` | Protocole | Écriture ou exécution d'un playbook |
| 05 | `05-format-connecteur.md` | Protocole | Écriture d'un connecteur |
| 06 | `06-chaine-de-preuve.md` | Protocole | Tout ce qui touche aux preuves, rapports, vérification |
| 07 | `07-specification-technique.md` | Technique | Toute implémentation |
| 08 | `08-roadmap.md` | Pilotage | Choix de la prochaine tâche |
| 09 | `09-ethique-et-juridique.md` | Métier | Nouvelle source, nouvelle règle, toute donnée de personne |
| 10 | `10-gouvernance-et-communaute.md` | Pilotage | Licence, contribution, financement |
| 11 | `11-sources-de-donnees-fr.md` | Métier | Connecteurs français |

Le mot « protocole » désigne les trois formats que d'autres implémentations peuvent adopter sans utiliser notre code : playbook, connecteur, chaîne de preuve. Ils sont versionnés indépendamment du code (`PROTOCOL_VERSION` dans `vigie/protocol/__init__.py`).

Convention de marquage dans toute la documentation : **[À VÉRIFIER]** signale une affirmation factuelle (seuil, URL, obligation légale) à confirmer sur la source officielle avant de s'en servir dans une règle. Un item marqué ainsi ne peut pas être codé en dur : il va dans `config/` avec un commentaire renvoyant à la source.
