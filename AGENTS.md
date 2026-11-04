# <nom-du-dépôt> Agent Instructions

## Objectif

<Ce que ce dépôt fait, en deux ou trois phrases. Acteurs, usages et périmètre : voir `docs/01_vision/README.md`.>

## Dépôts de référence

- `polyaero-platform` : ses ADR font foi pour tous les dépôts de PolyAéro. Ce dépôt y renvoie sans les résumer (`docs/README.md`).

## Structure du dépôt

- `docs/` : documentation (voir « Sources de vérité » ci-dessous)
- `.claude/skills/` : skills des agents IA : `git-workflow`, et `docs-audit` et `docs-compact`, lancés à la main pour auditer ou compacter la documentation
- <les dossiers de code du dépôt>

## Sources de vérité

Les dossiers de `docs/` sont numérotés, avec le même sens dans tous les dépôts de PolyAéro (`docs/README.md`). Chacun a un `README.md` court qui résume et indexe son contenu.

- `docs/01_vision` : vision : problème résolu, acteurs, usages, périmètre.
- `docs/08_decisions` : ADR de ce dépôt : décisions en vigueur, à ne pas contredire. Un ADR au statut « proposé » n'est pas encore en vigueur : le signaler avant de s'appuyer dessus. Les décisions communes sont dans les ADR de `polyaero-platform`.
- <les autres dossiers de `docs/` que le dépôt a créés>

## Contribution

Les règles de contribution sont dans `CONTRIBUTING.md`, à lire avant toute action git ou toute modification. Le skill `.claude/skills/git-workflow` les applique.

Règle propre aux agents : un agent ne committe et ne push jamais sans avoir présenté le diff et le message de commit, et obtenu une confirmation explicite de l'utilisateur, en particulier quand une décision est encore en cours de discussion. Il ne fusionne une Pull Request qu'à la demande explicite de l'utilisateur.

## Règles

- ne jamais écrire un secret (mot de passe, clé d'API, clé SSH, jeton) dans un fichier du dépôt, ni dans une issue ou une Pull Request (ADR-0004 de `polyaero-platform`) ;
- ne jamais implémenter une fonctionnalité hors périmètre ;
- écrire le code en anglais américain et la documentation en français (`CONTRIBUTING.md`) ;
- ne pas valider systématiquement les propositions de l'utilisateur : signaler les hypothèses fragiles, incohérences, risques ou complexité inutile plutôt que d'acquiescer ;
- lorsque plusieurs solutions existent, les comparer et formuler une recommandation plutôt que choisir silencieusement ;
- en cas d'incertitude, indiquer explicitement son niveau de confiance.
