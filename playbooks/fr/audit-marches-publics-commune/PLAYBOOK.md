---
id: fr/audit-marches-publics-commune
version: 0.1.0
protocol: 0.1
titre: Audit des marchés publics d'une commune
resume: Examine les marchés publics d'une commune française sur trois ans et signale les configurations qui méritent une vérification humaine.
juridiction: FR
cible:
  type: commune
  identification: [nom, siren, code_insee]
entrees:
  - {nom: institution, type: institution, obligatoire: true}
  - {nom: periode, type: periode, defaut: "36 mois glissants"}
sources:
  - {id: fr/decp, obligatoire: true}
  - {id: fr/sirene, obligatoire: true}
  - {id: fr/comptes-collectivites, obligatoire: false}
  - {id: fr/boamp, obligatoire: false}
regles: [A1, A2, A3, A4, B1, B2, C1, C2, C3, D1, D2, E1, E2, G1, G2, G4]
catalogue: ">=0.1.0"
niveau: debutant
duree_estimee: 15 min
necessite_llm: false
points_de_controle: [identification, rapprochements_forts, export]
licence: CC-BY-SA-4.0
auteurs: [Vigie]
---

## Objectif

À la fin, vous saurez combien de marchés publics votre commune a passés sur la période, avec qui, selon quelles procédures, et lesquels présentent une configuration que la littérature anticorruption recommande d'examiner. Vous aurez pour chacun les documents sources et la liste de ce qu'il faudrait vérifier avant d'en tirer quoi que ce soit.

## À qui s'adresse ce playbook

À n'importe qui. Aucune connaissance des marchés publics n'est nécessaire : le rapport explique chaque signal et ses explications ordinaires possibles.

## Étapes

1. `[collecte] fr/sirene recherche={institution.nom} type=commune` — identifier la commune (SIREN, code INSEE, population).
2. `[controle] identification` — si plusieurs communes correspondent, ou si le nom a été saisi sans SIREN, afficher les candidates (nom, département, population) et faire choisir.
3. `[collecte] fr/decp acheteur={institution.siren} periode={periode}` — récupérer les marchés publiés. Archiver avec provenance. Si zéro marché : continuer (G1 jugera).
4. `[collecte] fr/comptes-collectivites siren={institution.siren} periode={periode}` (si disponible) — dépenses d'équipement et charges à caractère général, pour G1 et la strate.
5. `[collecte] fr/boamp acheteur={institution.siren} periode={periode}` (si disponible) — avis de publicité et d'attribution, pour A1/A2.
6. `[collecte] fr/sirene siren={attributaires[*].siren}` — fiche de chaque attributaire (création, effectif, état, NAF, adresse).
7. `[calcul] normalisation` — DECP → contrats OCDS ; attributaires → entités FollowTheMoney ; rapprochement BOAMP ↔ DECP (exact par identifiant quand possible, sinon « probable »).
8. `[regles] G2 G4` — qualité des données ; fixe la confiance.
9. `[regles] A1 A2 A3 A4 B1 B2 C1 C2 C3 D1 D2 E1 E2 G1` — règles principales avec les seuils de `config/seuils.fr.yaml` et paramètres de `config/regles.fr.yaml`.
10. `[calcul] strate_percentiles` — chaque indicateur agrégé comparé à la strate de la commune (données nationales DECP de la période).
11. `[calcul] combinaisons` — co-occurrences du catalogue.
12. `[controle] rapprochements_forts` — pour tout signal de sévérité forte reposant sur un rapprochement « probable », montrer les deux fiches et demander confirmation ; sinon le signal passe en confiance basse.
13. `[agent] schema=Rapport.redaction` (si un modèle est configuré) — rédiger constat et synthèse à partir des signaux uniquement. Chaque phrase factuelle référence des identifiants de signal ; chaque chiffre, nom, montant, date est copié d'une preuve. Pas d'ajout d'information extérieure.
14. `[calcul] lint` — vocabulaire interdit et ancrage ; échec = pas de rapport.
15. `[calcul] verify` — recalcul des empreintes et rejeu des règles.
16. `[controle] export` — afficher le rappel de responsabilité et les signaux à confiance basse avant tout export partageable.

## Points de contrôle humain

- **identification** : se déclenche si ambiguïté ou saisie par nom. Choix : une commune, ou abandon.
- **rapprochements_forts** : se déclenche s'il existe au moins un signal `forte` fondé sur un rapprochement probable. Choix par rapprochement : confirmer / infirmer / je ne sais pas (→ confiance basse).
- **export** : se déclenche à toute demande d'export HTML/Markdown/CSV/FtM. Affiche le texte fixe de responsabilité et la liste des signaux `basse`. Choix : exporter / revenir.

## Sortie attendue

Rapport complet au contrat de sortie (doc 02 §4) : en-tête, avertissement fixe, périmètre et limites, synthèse, signaux, tableaux (marchés, attributaires, indicateurs vs strate), méthodologie, manifeste des preuves, suites possibles. Exports : JSON canonique, HTML autonome, Markdown, CSV des marchés, FollowTheMoney (`.ftm.json`).

## Pièges connus et faux positifs

- Les communes adhérant à un **groupement de commandes** publient peu : leurs marchés apparaissent chez le coordonnateur. G1 le signale ; vérifier avant de conclure à un défaut de publication.
- Les **montants d'accords-cadres** sont souvent des maximums, pas des dépenses ; B1, C2 et D1 peuvent surréagir. Le rapport indique quand un montant est un maximum.
- Les **petites communes** (strates C1-C2) ont peu de marchés : les règles agrégées ne se déclenchent pas sous `min_marches_pour_agregat`, et c'est voulu.
- Les **doublons DECP** (publication par plusieurs profils d'acheteurs) gonflent artificiellement les volumes ; G4 les dédoublonne, mais vérifier les totaux.
- **Nombre d'offres absent** : A1 s'appuie alors sur BOAMP ; si BOAMP est indisponible, A1 est à confiance basse.

## Suites d'enquête suggérées

- Pour un signal A1/A3 : demander le rapport d'analyse des offres et la motivation de la procédure (`templates/cada-rapport-analyse-offres.md`).
- Pour un signal B2 : demander la computation des seuils retenue par l'acheteur et l'existence d'un accord-cadre (`templates/cada-computation-seuils.md`).
- Pour un signal C2/C3 : enchaîner `fr/audit-deliberations-commune` (phase 2) pour retrouver les délibérations et les votes.
- Pour un signal E1/E2 : consulter BODACC (historique) ; vérifier les déclarations de sous-traitance.
- Question type en conseil municipal : `templates/question-conseil.md`.
- Transmettre à qui peut agir : journaliste local, élu d'opposition, association de transparence, CRC (saisine citoyenne possible **[À VÉRIFIER : modalités]**).

## Limites

Ne regarde ni les délibérations, ni les subventions, ni les liens d'intérêts (phases 2-3). Ne voit que ce qui a été publié en DECP ; un marché non publié est invisible, sauf par G1. Ne juge pas la qualité ou le prix des prestations.

## Historique

- 0.1.0 — version initiale.
