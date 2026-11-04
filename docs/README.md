# Documentation

La documentation est rangée dans des dossiers numérotés. **Un numéro a le même sens dans tous les dépôts de PolyAéro**, et un dépôt ne crée que les dossiers qu'il utilise : pas de dossier vide, et des trous dans la numérotation si besoin. La numérotation est celle de Formasup Méditerranée.

Chaque dossier créé a un `README.md` court (une page maximum) qui résume et indexe son contenu. Le détail vit dans des fichiers voisins. Seule exception, le `README.md` de `01_vision` contient la vision elle-même, qui tient en une page.

| Dossier | Contenu |
|---|---|
| `01_vision` | Vision : acteurs, usages, proposition de valeur, périmètre |
| `02_architecture` | Architecture technique : composants, contraintes non fonctionnelles. Résume les ADR, sans les contredire |
| `03_domaines` | Modèle de domaine : entités métier et règles de gestion |
| `04_apis` | Contrats d'API : endpoints et formats d'échange |
| `05_epics` | Epics : regroupements de fonctionnalités à haut niveau |
| `06_features` | Features : découpage des epics en fonctionnalités livrables. Seul un projet lourd a ce dossier (ADR-0005 de `polyaero-platform`) |
| `07_user-stories` | User stories et critères d'acceptation |
| `08_decisions` | ADR : décisions techniques en vigueur et leurs justifications |
| `09_backlog` | Backlog : liste et priorisation des éléments à traiter |
| `10_reference` | Références : glossaire, normes, ressources externes |
| `11_exploitation` | Procédures d'exploitation : opérations courantes, montées de version, incidents |
| `99_roadmap` | Roadmap : planning et jalons |

Un nouveau numéro s'ajoute d'abord ici, dans `polyaero-platform`, pour qu'il garde le même sens partout.

## Une information, un seul endroit

Chaque information a un seul document de référence, qui fait foi : par exemple une décision dans son ADR, un terme dans le glossaire, une règle de gestion dans `03_domaines`, une opération courante dans sa procédure. Ailleurs, on y renvoie. Le glossaire définit un terme sans énoncer les règles de gestion qui s'y appliquent : il renvoie à `03_domaines`.

Un résumé n'est permis que dans les documents dont c'est le rôle : les `README.md`, `AGENTS.md`, `01_vision` pour le périmètre, `02_architecture` pour les ADR, `CONTRIBUTING.md` et les skills de `.claude/skills/` pour ce qu'il faut savoir pour agir (entre dépôts, la règle est plus stricte : voir « Renvois entre dépôts »). Il respecte trois conditions :

- **il cite sa référence**, par exemple `ADR-0001` ou `glossaire.md` ;
- **il ne la contredit pas** : en cas d'écart, la référence fait foi ;
- **il n'y ajoute rien** : une précision, une exception ou une valeur s'écrit d'abord dans la référence, puis le résumé la reprend s'il le faut.

Mentionner n'est pas résumer : nommer une décision ou un composant pour situer le propos, avec son renvoi, est toujours permis. Toute autre répétition est une redondance, à remplacer par un renvoi. Les skills `docs-audit` et `docs-compact` aident à le vérifier.

## Points à trancher

Dans tous les documents, les points à trancher sont écrits sous un titre `## Questions ouvertes`.

## Renvois entre dépôts

Une décision n'est écrite qu'une fois, dans le dépôt qui la prend. Les autres dépôts y renvoient, sans la recopier.

- **Un renvoi nomme son dépôt** : `ADR-0002 de polyaero-platform`, `#12 de polyaero-platform`, et dans le code `ADR-0002 of polyaero-platform`. Un numéro seul désigne un document du dépôt courant.
- **Un projet ne résume pas les décisions de `polyaero-platform`.** Un document du projet y renvoie, avec au plus une phrase sur ce que cela implique pour lui, et n'écrit en détail que ses propres choix : ses noms, ses valeurs, ses exceptions. Seuls `CONTRIBUTING.md` et les skills reprennent d'une décision de `polyaero-platform` ce qu'il faut savoir pour agir, avec son renvoi : c'est le cas du flux git (ADR-0002 de `polyaero-platform`). Quand `polyaero-platform` change, le projet n'a rien d'autre à corriger.
- **Un besoin d'un projet envers `polyaero-platform` est une issue de `polyaero-platform`.**
