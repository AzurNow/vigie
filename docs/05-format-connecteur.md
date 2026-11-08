# 05 — Format de connecteur (protocole, v0.1)

Un connecteur décrit **une source de données publique** : où elle est, sous quelle licence, à quelle fréquence, comment on la récupère, comment on la lit, et ce qu'on en tire. Il se compose d'un fichier YAML (la description, portable) et, si nécessaire, d'un parseur Python (l'implémentation). L'objectif de dispersion impose que le cas le plus fréquent — « ajouter ma commune » — tienne dans **le YAML seul**, sans code.

## Emplacement et nommage

```
connectors/<juridiction>/<slug>.yaml               # source nationale ou générique
connectors/<juridiction>/communes/<code_insee>.yaml # fiche d'une institution pour un connecteur générique
vigie/connectors/<juridiction>/<slug>.py           # parseur, si le format l'exige
```

Identifiant : `<juridiction>/<slug>`, ex. `fr/decp`, `eu/ted`, `ocds/generic`.

## Deux familles

1. **Connecteur de source** (`kind: source`) : un jeu de données national ou international, structuré, avec un parseur dédié. Ex. `fr/decp`, `fr/sirene`.
2. **Connecteur générique + fiches** (`kind: generic`) : un type de documents publié par chaque institution sur son propre site (PV de conseil, recueils d'actes). Le connecteur générique définit le traitement ; la fiche par institution donne les URL et sélecteurs. Ex. `fr/pv-conseil` + `connectors/fr/communes/69123.yaml`.

## YAML d'un connecteur de source

```yaml
id: fr/decp
version: 0.1.0
protocol: 0.1
kind: source
nom: Données essentielles de la commande publique (consolidées)
juridiction: FR
editeur: "État (DAJ / Etalab) [À VÉRIFIER]"
licence: "Licence Ouverte 2.0 [À VÉRIFIER]"
obligation_legale: "Code de la commande publique, art. R2196-1 et s. [À VÉRIFIER]"
description: >
  Marchés publics au-dessus du seuil de publication, tous acheteurs, avec attributaires,
  procédure, montant, durée et modifications.
acces:
  type: http              # http | api | fichier_local | import_manuel
  url: "https://www.data.gouv.fr/... [À VÉRIFIER]"
  format: json            # json | csv | xml | pdf | html
  schema: decp-v2         # nom du schéma source, documenté dans vigie/connectors/schemas/
  auth: aucune
  frequence: quotidienne
  taille_indicative: "plusieurs centaines de Mo"
  user_agent: "vigie/{version} (+https://... )"
  politesse: {requetes_par_seconde: 2, respecter_robots_txt: true}
filtres:
  - {champ: acheteur.siret, type: prefixe_siren}
  - {champ: date_notification, type: periode}
produit:                  # entités produites, schéma interne
  - {type: Contract, schema: ocds}
  - {type: Organization, schema: ftm}
champs_cles: [id, acheteur.siret, attributaires[].siret, cpv, procedure, montant, date_notification, modifications[]]
qualite_connue:
  - "nombre_offres_recues souvent absent"
  - "modifications mal renseignées"
  - "doublons inter-profils d'acheteurs"
parseur: vigie.connectors.fr.decp:parse
provenance:
  capturer: [url_finale, date_heure, etag, content_length, sha256]
tests: tests/connectors/fr/test_decp.py
```

## YAML d'un connecteur générique

```yaml
id: fr/pv-conseil
version: 0.1.0
protocol: 0.1
kind: generic
nom: Procès-verbaux et listes des délibérations des assemblées délibérantes
juridiction: FR
obligation_legale: "CGCT, publication en ligne des PV et listes des délibérations depuis le 1er juillet 2022 [À VÉRIFIER]"
documents:
  - {type: pv_seance, formats: [pdf, html]}
  - {type: liste_deliberations, formats: [pdf, html]}
traitement:
  ocr: si_necessaire
  extraction: {schema: Seance, methode: llm_contraint}   # schéma dans vigie/protocol/schemas/
  preuve: {localisation: page_et_extrait}
fiche_institution:         # ce que chaque fiche doit fournir
  requis: [code_insee, siren, pages[]]
  page:
    url: "URL de la page listant les séances"
    selecteur_liens: "sélecteur CSS ou XPath des liens vers les PDF"
    motif_date: "regex pour extraire la date de séance du titre ou du nom de fichier"
    type: pv_seance | liste_deliberations
```

Fiche d'une commune (c'est **ça** que « ajouter ma ville » signifie) :

```yaml
# connectors/fr/communes/69123.yaml
code_insee: "69123"
siren: "216901231"
nom: Lyon
connecteurs:
  fr/pv-conseil:
    pages:
      - url: "https://www.exemple.fr/conseil-municipal/proces-verbaux"
        selecteur_liens: "a[href$='.pdf']"
        motif_date: "(\\d{2})[-_](\\d{2})[-_](\\d{4})"
        type: pv_seance
verifie_le: "2026-11-07"
verifie_par: "pseudo"
```

## Obligations de tout connecteur

- **Provenance** : chaque document récupéré est stocké dans le store de contenu avec URL finale, date/heure, en-têtes utiles, empreinte SHA-256, taille. Sans provenance, pas d'archivage.
- **Politesse** : respecter `robots.txt` quand il existe, limiter le débit, s'identifier, ne jamais contourner un contrôle d'accès ou un captcha. Si une source refuse l'accès automatisé, le connecteur passe en `import_manuel` (l'utilisateur télécharge lui-même) et la doc l'explique.
- **Licence** : documentée ; un connecteur vers une source dont la licence interdit la réutilisation est refusé.
- **Idempotence** : relancer un `fetch` ne retélécharge que ce qui a changé (ETag / Last-Modified / empreinte).
- **Pas de personnes** : un connecteur ne produit des entités `Person` que dans les cas listés par la doc 09 (fonction publique ou légale), avec le champ `role` renseigné.
- **Tests** : fixtures synthétiques ou extraits réels sans données personnelles ; test de parsing ; test de provenance.

## Validation

`vigie connector validate <chemin>` vérifie : schéma YAML, licence renseignée, URL joignable (optionnel, `--online`), parseur importable, tests présents, absence de `Person` hors cas autorisés, champs de provenance.

## Contribution au commons

Accepté si : validation passée ; `verifie_le` récent pour les fiches ; une personne autre que l'auteur a pu exécuter le connecteur ; licence source compatible.
