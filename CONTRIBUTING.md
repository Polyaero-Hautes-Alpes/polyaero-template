# Contribuer à <nom-du-dépôt>

Ce guide est la référence pratique pour contribuer à ce dépôt, qu'on soit un humain ou un agent IA. Les décisions communes et leurs justifications sont dans les ADR de `polyaero-platform` ; ce guide ne fait que les appliquer.

## Langue

- **Code en anglais américain** : noms de variables, de fonctions, de fichiers, de tables, commentaires.
- **Documentation en français** : `docs/`, README, messages de commit, titres et descriptions de PR.

## Branches et Pull Requests

Les règles dépendent du type du dépôt, déclaré par son topic GitHub (ADR-0002 de `polyaero-platform`) : **service déployé** (`service`), **plugin distribué** (`plugin`), **prototype** (`prototype`) ou **documentation seule** (`docs`). Garder ici la partie qui correspond au dépôt.

Pour un service déployé, un plugin distribué ou un dépôt `docs` :

- **Jamais de commit direct sur `main` ni sur `develop`.** Tout passe par une branche courte et une Pull Request. Seul le tout premier commit d'un dépôt est fait sur `main`, avant la création de `develop`.
- `feature/...`, `fix/...`, `chore/...`, `docs/...` partent de `develop`, et y reviennent par une PR.
- **Service déployé ou dépôt `docs`** : `release/AAAA-MM-JJ` part de `develop` et est fusionnée vers `main`. `hotfix/...` part de `main` pour un correctif urgent.
- **Plugin distribué** : une version se livre par un tag `vX.Y.Z` sur `main`, après une PR de `develop` vers `main`.
- Après chaque release, hotfix ou version, `main` est reportée vers `develop` par une PR fusionnée avec un **commit de merge**, jamais par *squash* ni *rebase*. **La PR part d'une branche de synchronisation** (`sync/main-to-develop`), créée à la pointe de `main`, et jamais de `main` elle-même : la suppression automatique des branches fusionnées supprimerait `main`.
- Vers `develop`, l'auteur peut fusionner sa PR une fois prête. Vers `main`, une approbation est exigée.

Pour un prototype : une seule branche `main`, des PR conseillées, aucune obligation.

## Messages de commit

Format [Conventional Commits](https://www.conventionalcommits.org/) : `type(portée): description`, par exemple `feat(api): ...`, `fix(deploy): ...`. Les modifications de `docs/` utilisent le type `docs`, sans portée. Le corps tient en une ou deux phrases, centrées sur le *pourquoi*.

## Documentation

- Les dossiers de `docs/` sont numérotés, avec le même sens dans tous les dépôts (voir `docs/README.md`).
- **ADR** : un ADR décrit la décision en vigueur. Quand une décision ultérieure le modifie, on corrige son texte et on l'indique dans son statut. Une décision commune à PolyAéro se prend dans `polyaero-platform`, pas ici.
- **Points ouverts** : sous un titre `## Questions ouvertes`.
- **Travail restant** : une issue GitHub par tâche, qu'une Pull Request ferme (`Closes #NN`). Une personne est assignée à l'issue avant son passage en In Progress dans le projet GitHub `polyaero` (ADR-0005 de `polyaero-platform`). Un besoin envers un autre dépôt est une issue de cet autre dépôt (voir `docs/README.md`).

## Versions

Pour un service ou un plugin, les montées de version des dépendances arrivent par les Pull Requests mensuelles de Dependabot, vers `develop` : on lit les notes de version avant de fusionner (ADR-0006 de `polyaero-platform`). Un correctif de sécurité n'attend pas le mois suivant.

## Secrets

Aucun secret dans git, dans une issue ou dans une PR (ADR-0004 de `polyaero-platform`). Un fichier `.env` n'est jamais commité : on commite un `.env.example`, sans valeurs.
