# 08 — Roadmap

Chaque phase a une **définition de terminé** vérifiable. On ne passe pas à la phase suivante sans l'atteindre. Les durées supposent un contributeur principal seul, avec assistance d'un agent de code ; elles sont indicatives.

## Phase 0 — Fondations (2 à 3 semaines)

Livrables :
- Dépôt, licence, CI, `pyproject`, structure (doc 07 §3).
- `vigie/protocol` : schémas pydantic Document / Evidence / Signal / Report ; `PROTOCOL_VERSION = "0.1"`.
- `vigie/store` : content-addressed store, provenance, index duckdb.
- `vigie/llm` : interface Provider + implémentations `openai_compatible` et `anthropic` ; journalisation ; gabarit `redaction.v1`.
- `vigie/lint` : vocabulaire + ancrage.
- Connecteur `fr/decp` (téléchargement, parsing DECP v2 → OCDS, preuves déterministes, dédoublonnage G4).
- Connecteur `fr/sirene` (annuaire des entreprises, sans clé).
- Règles **A1, B1, C2** + G2, G4, avec tests.
- Runner de playbooks minimal (collecte, regles, calcul, controle, agent) + `fr/audit-marches-publics-commune` exécutable de bout en bout.
- Rendu HTML autonome ; `verify` ; `lint`.

Terminé quand : `vigie audit --commune <une commune réelle>` produit un rapport HTML qui passe `verify` et `lint`, en moins de 15 minutes, sur une machine ordinaire, avec et sans LLM.

## Phase 1 — Marchés publics, communes (4 à 6 semaines)

- Règles A2, A3, A4, B2, C1, C3, D1, D2, E1, E2, G1 ; combinaisons.
- Connecteur `fr/comptes-collectivites` ; `fr/boamp` (optionnel) ; strates (`vigie strata build fr`) et résumés publiés dans le dépôt.
- Mode `--batch`, `diff`, marques, export feedback, exports CSV et FtM.
- Documentation utilisateur « auditer sa commune en une soirée ».
- Binaire autonome Windows/macOS.
- **Lancement** : rapports réels sur 20 communes choisies pour la diversité des strates, relus à la main, publiés par leurs auteurs (pas par le projet) ; billet de méthode ; annonce dans les communautés (GIJN, Data for Good, Hacks/Hackers, OpenAleph Discourse, forums CRC/open data).

Terminé quand : critères d'acceptation v1 (doc 02 §10) atteints ; un contributeur externe a fait accepter une règle ou un connecteur.

## Phase 2 — Assemblées délibérantes (6 à 8 semaines)

- Connecteur générique `fr/pv-conseil` + 20 fiches de communes ; OCR optionnel ; extraction LLM contrainte avec preuves relocalisées.
- Entité `Deliberation` ; règles I1–I4 ; enrichissement de B1/B2 (estimation initiale) ; E3, E4 (BODACC/RNE, sous les contraintes de la doc 09) ; D3, B3, E5, E6, F1–F3.
- Playbook `fr/audit-deliberations-commune`.
- Guide « ajoute ta ville en 20 lignes » ; CI qui valide les fiches.

Terminé quand : 50 fiches de communes dans le commons, dont 30 par des contributeurs externes ; I1 testée sur 10 communes avec taux de faux positifs documenté.

## Phase 3 — Liens, subventions, autres collectivités (6 à 8 semaines)

- Connecteurs `fr/hatvp`, `fr/subventions`, `fr/rne` (dans les limites de la doc 09) ; rapports CRC (contexte « déjà examiné »).
- Règles H1–H3, C4 ; playbook `fr/audit-subventions-commune`.
- Variantes EPCI, département, région ; strates correspondantes ; graphe institution → satellites.
- Signature des rapports (optionnelle) ; `vigie serve` local si demandé.
- Demande de financement (NLnet / NGI Zero) préparée dès la phase 1, déposée ici au plus tard.

Terminé quand : un département entier (toutes communes + EPCI + conseil départemental) audité par une association partenaire avec le mode lot, et son retour intégré.

## Phase 4 — Établissements (hôpitaux, bailleurs, SEM/SPL, syndicats)

- Sources de comptes publiés spécifiques ; strates par type ; playbooks dédiés.
- Priorité fixée par la demande des contributeurs.

## Phase 5 — International

- Connecteur `ocds/generic` ; adaptation des règles A–D et G à OCDS pur ; entités via OpenCorporates/OpenSanctions.
- Premier playbook non français écrit par un contributeur local (ne pas le faire soi-même : la dispersion passe par l'appropriation).
- i18n complète ; catalogue traduisible.

## Transversal, en continu

- Calibration des seuils avec les retours anonymisés (doc 02 §4.5).
- Référencement de la littérature pour chaque règle (`docs/references.md`).
- Relecture juridique de la doc 09 par un professionnel dès que possible (phase 1 ou 2).
- Gouvernance : passer d'un mainteneur à trois avant la phase 3 (doc 10).

## Ce qui n'est pas prévu

- Un service hébergé. Une API publique. Un classement d'institutions. Un « score ». Une application mobile. Tout ce qui ferait du projet une cible plutôt qu'un protocole.
