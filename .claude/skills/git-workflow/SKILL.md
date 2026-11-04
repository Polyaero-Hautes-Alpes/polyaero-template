---
name: git-workflow
description: Applique le flux git de ce dépôt (main/develop + feature/release/hotfix, jamais de commit direct sur main ou develop, confirmation explicite avant tout commit/push). À utiliser avant toute action git dans ce dépôt (créer une branche, committer, pousser, ouvrir une PR).
---

# Workflow git

Les règles (branches, commits, releases) sont dans `CONTRIBUTING.md`, à lire avant d'appliquer ce skill, et la règle propre aux agents dans `AGENTS.md`.

## Étapes

1. **Vérifier la branche courante** (`git branch --show-current`). Si c'est `main` ou `develop`, créer une branche courte avant d'écrire quoi que ce soit, en partant de la bonne branche (voir `CONTRIBUTING.md`). Seule exception : le tout premier commit d'un dépôt, fait sur `main` avant la création de `develop`.
2. **Confirmation explicite avant `git commit`.** Présenter à l'utilisateur le diff et le message de commit proposé. Ne committer qu'après un accord explicite.
3. **Confirmation explicite avant `git push`.** Même règle que pour le commit.
4. **Ouvrir une Pull Request après le push** (`gh pr create`), vers `develop` par défaut, vers `main` uniquement pour une PR `release/*` ou `hotfix/*`.
5. **Après la fusion d'une `release/*` ou d'un `hotfix/*` vers `main`**, créer `sync/main-to-develop` à la pointe de `main`, la pousser et ouvrir une PR vers `develop`, jamais depuis `main` elle-même (la suppression automatique des branches fusionnées la supprimerait), à fusionner par un commit de merge, jamais par *squash* ni *rebase* : le rappeler à l'utilisateur.

## Ce que ce skill ne fait pas

Tant que l'organisation reste sur le plan gratuit de GitHub, rien n'empêche techniquement un push direct sur `main` ou `develop` d'un dépôt privé (ADR-0002 de polyaero-platform) : ce skill est alors le seul garde-fou. Un agent ne pousse jamais directement sur ces branches, et ne fusionne une Pull Request qu'à la demande explicite de l'utilisateur.
