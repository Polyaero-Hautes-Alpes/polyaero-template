# polyaero-template

Gabarit des dépôts de PolyAéro Hautes-Alpes : il fournit les conventions communes, pour qu'un nouveau dépôt démarre aligné sur les règles de [polyaero-platform](https://github.com/Polyaero-Hautes-Alpes/polyaero-platform). Un dépôt existant s'aligne en reprenant les mêmes fichiers.

## Ce que fournit le gabarit

| Fichier | Rôle |
|---|---|
| `CONTRIBUTING.md` | Flux git, commits, langue, documentation, secrets |
| `AGENTS.md`, `CLAUDE.md` | Instructions des agents IA |
| `.claude/skills/` | `git-workflow`, `docs-audit`, `docs-compact` |
| `docs/README.md` | Numérotation commune des dossiers de documentation |
| `.github/dependabot.yml.example` | Exemple de suivi des versions (ADR-0006 de `polyaero-platform`) |
| `.gitignore` | Fichiers locaux et `.env` jamais commités |

## Utiliser le gabarit

1. Suivre la procédure [Créer un dépôt](https://github.com/Polyaero-Hautes-Alpes/polyaero-platform/blob/main/docs/11_exploitation/creer-un-depot.md) de `polyaero-platform` : nom, type, topics, droits des équipes, branches.
2. Dans le nouveau dépôt, remplacer chaque `<...>` de `AGENTS.md` et de `CONTRIBUTING.md`, et réécrire ce `README.md`.
3. Garder dans `CONTRIBUTING.md` la partie qui correspond au type du dépôt, et supprimer les autres.
4. Pour un service ou un plugin : renommer `.github/dependabot.yml.example` en `dependabot.yml` et ne garder que les écosystèmes du dépôt. Un prototype n'en a pas.
5. Créer seulement les dossiers de `docs/` dont le dépôt a besoin (voir `docs/README.md`).

## Mettre à jour un dépôt existant

Comparer ses fichiers à ceux de ce gabarit, et reporter les écarts qui ne sont pas des choix propres au dépôt. Les règles font foi dans `polyaero-platform` : en cas d'écart, ce dépôt-là a raison.

## Ce dépôt

Type `docs` (ADR-0002 de `polyaero-platform`) : rien n'y est déployé, et chaque release publie le gabarit sur `main`.
