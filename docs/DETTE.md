# Dette — ce qui peut casser plus tard

Journal tenu par la compétence `cloture-de-session` : une entrée par session, la plus récente en
tête, au gabarit de `.claude/skills/cloture-de-session/references/gabarits.md`. Un point réglé est
barré sur place, avec ce qui l'a réglé. Les trous de la spécification n'y vont pas : ils deviennent
des ADR « proposés » dans `docs/decisions/`.

---

## 2026-11-08 — Contexte agentique importé du Banc d'essai 2027 (`.claude/`, `CLAUDE.md`, `HANDOFF.md`)

### 1. Les compétences citent des commandes et une arborescence qui n'existent pas encore — *basse*

`revue-de-code`, `conformite-specification`, les agents et le gabarit citent `uv run pytest`,
`uv run mypy --strict vigie/`, `uv run vigie lint`, `tests/golden/` et les modules de `docs/07` §3.
Rien de cela n'existe en phase 0.

**Pourquoi ça casse.** Si la structure réelle s'écarte de `docs/07` §3 (un module renommé, une
commande différente), une session ou un sous-agent suit une consigne fausse ; on le voit à la
première commande qui échoue, d'où la gravité basse.

**Ce qu'il faut faire.** À la fin de la phase 0, lancer `revue-contexte-agentique` : son axe 1 relève
mécaniquement les chemins et commandes absents.

### 2. Le partage des tâches entre `codeur-opus` et `codeur-sonnet` place les parseurs des connecteurs du côté d'Opus — *basse*

Le parsing qui produit des preuves est confié à `codeur-opus`, le téléchargement à `codeur-sonnet` :
un connecteur passe donc par deux agents, ou par Opus seul.

**Pourquoi ça casse.** Un brief qui confie un connecteur entier à Sonnet le fait s'arrêter à mi-chemin
(sa définition le lui demande). Coût : un aller-retour, visible.

**Ce qu'il faut faire.** Garder ce partage tant que les preuves déterministes des parseurs ne sont pas
couvertes par `verify` et des tests ; le revoir ensuite (décision de l'auteur).
