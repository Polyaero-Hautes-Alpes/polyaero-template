---
name: docs-compact
description: "Passe en revue toute la documentation du dépôt (docs/, README.md, AGENTS.md, CLAUDE.md, CONTRIBUTING.md, .claude/) pour proposer de la compacter : contenu obsolète, contenu inutile, passages verbeux. Produit des propositions dans la conversation, gardées dans une issue du dépôt avec l'accord de l'utilisateur, sans modifier aucun fichier. Les redondances relèvent de docs-audit. À lancer à la main, à la demande d'un membre de l'équipe."
disable-model-invocation: true
---

# Compaction de la documentation

Ce skill propose, et ne fait que proposer : il ne modifie, ne déplace et ne supprime aucun fichier. Chaque proposition dit sur quoi elle repose, dans cet ordre de préférence : une règle écrite (fichier et ligne), un fait vérifié (issue fermée, fichier supprimé, historique git), ou, à défaut, un jugement, présenté comme tel.

## Périmètre

Celui de `docs-audit` : les fichiers suivis par git, dans l'état de l'arbre de travail, de `docs/`, `README.md`, `AGENTS.md`, `CLAUDE.md`, `CONTRIBUTING.md` et `.claude/`. Si l'utilisateur limite la revue à un dossier ou à des fichiers, ne traiter que ceux-là.

**Hors périmètre :**

- **les redondances**, qui relèvent de `docs-audit`. Si la lecture en fait apparaître, le rapport le dit dans ses questions ouvertes et suggère de lancer `docs-audit`, sans en faire des propositions ;
- **le bien-fondé des décisions** : proposer de raccourcir un texte, jamais de changer ce qu'il décide.

**Ne se compacte jamais :**

- les sections qu'impose un plan : celui des ADR (`docs/08_decisions/README.md`), dont chaque option avec ses avantages et ses inconvénients, et ceux des modèles `_modele.md` ;
- la justification d'une décision, le *pourquoi*, qui fait la valeur d'un ADR ;
- les renvois, et les marques `<PROJET>` et `<À COMPLÉTER …>` ;
- les nuances qui portent une règle : « sauf », « seulement », « au plus », « jamais », « même quand ».

## 1. Lire les références

Les règles qui disent ce qu'un document doit contenir, et donc ce qu'il ne doit plus contenir : `docs/README.md`, le `README.md` de chaque dossier de `docs/`, `CONTRIBUTING.md`, `AGENTS.md`, les ADR du dépôt et son glossaire. Lire aussi les issues du dépôt, en une fois :

```bash
gh issue list --state all --limit 5000 --json number,state,stateReason,title,closedAt > <issues>
```

Si GitHub n'est pas accessible, le dire, et marquer « à confirmer » toute proposition qui dépend de l'état d'une issue.

## 2. Repérer les indices

Avec des commandes (`grep`, `git log`), sur chaque fichier, sans échantillonner. Un indice n'est pas une proposition : il désigne un passage à lire à l'étape 3.

- **issues fermées citées comme en cours** : point à vérifier, question ouverte ou tâche qui cite une issue fermée. Le point est sans doute tranché ou vérifié, et son texte peut être intégré ou retiré selon la règle de son dossier ;
- **fichiers supprimés** encore nommés dans la documentation (`git log --diff-filter=D --name-only --format=`) : le passage qui les décrit peut être devenu sans objet ;
- **marqueurs temporels** : dates, « désormais », « auparavant », « initialement », « anciennement », « pour l'instant », « provisoire », « temporaire », « en attendant », « à terme ». Ils signalent souvent un état passé ou une transition terminée ;
- **historique dans un ADR** : versions successives ou récit de la façon dont une question a été tranchée, que `docs/08_decisions/README.md` renvoie à git ;
- **déroulé d'un essai dans un ADR** : objet d'essai, étapes ou mesures intermédiaires d'un point vérifié, que « Points à vérifier » de `docs/08_decisions/README.md` laisse dans l'issue ;
- **plus de cinq utilisations** sous « Dernières utilisations » d'une procédure, au-delà de la limite de `docs/11_exploitation/README.md` ;
- **statut d'une user story** écrit dans son fichier, alors qu'il se tient dans le projet GitHub (ADR-0005 de `polyaero-platform`) ;
- **`README.md` de plus d'une page**, limite fixée par `docs/README.md` : le détail peut partir dans un fichier voisin ;
- **sections vides** ou réduites à « rien à signaler », « N/A », « TODO ».

## 3. Lire et proposer

Lire en entier chaque fichier du périmètre, et classer chaque proposition dans la première catégorie qui s'applique :

1. **Obsolète** : le passage décrit un état qui n'est plus vrai, une transition terminée, un point tranché ou vérifié. Proposer la suppression, ou l'intégration du résultat selon la règle du dossier (pour un ADR, « Points à vérifier » de `docs/08_decisions/README.md`).
2. **Inutile** : le passage n'aide ni à comprendre ni à agir. Par exemple une annonce du plan qui suit, une précaution oratoire, un exemple de plus qui n'apprend rien, un récit sans conséquence pour la décision en vigueur. Proposer la suppression.
3. **Verbeux** : le même sens tient en moins de mots. Proposer le texte de remplacement. Ne rien proposer si une règle, une valeur, une exception, un renvoi ou une nuance s'y perdrait, ou si le doute persiste.

Un même défaut répété dans plusieurs fichiers fait une seule proposition, qui liste les fichiers.

**Au-delà d'une trentaine de fichiers**, confier la lecture à des sous-agents, par lots d'une quinzaine de fichiers au plus, regroupés par sujet, comme pour `docs-audit`. Chaque fichier du périmètre est attribué à un lot, et chaque sous-agent reçoit ce skill et les références de l'étape 1. **Attendre le retour de tous les sous-agents** avant de rédiger le rapport. Si l'un d'eux échoue, le rapport le dit et liste les fichiers non lus.

## 4. Rapport

Dans la conversation, sans rien modifier :

1. **Références lues** : dépôt, branche et commit.
2. **Indices cherchés** : chaque indice de l'étape 2, avec son résultat, y compris « rien trouvé ».
3. **Propositions**, par catégorie (obsolète, inutile, verbeux), puis par fichier. Pour chacune : fichier et lignes, texte actuel (extrait suffisant pour le retrouver), proposition (suppression, intégration ou texte de remplacement), fondement (règle, fait vérifié ou jugement), gain en lignes, et confiance (**sûr** ou **à confirmer**). Une proposition fondée sur un jugement est toujours **à confirmer**.
4. **Bilan** : par fichier, le nombre de lignes avant et après les propositions retenues.
5. **Questions ouvertes** : passages dont l'utilité ne se décide pas sans l'équipe, redondances aperçues (pour `docs-audit`), règle qui paraît manquer pour décider.

## 5. Garder le rapport

Proposer à l'utilisateur de garder le rapport dans une issue du dépôt, intitulée « Compaction de la documentation du AAAA-MM-JJ », qui reprend le rapport entier, et ne la créer qu'avec son accord explicite. Si le rapport dépasse la taille d'un texte d'issue, la suite va dans des commentaires de l'issue, dans l'ordre.

Le rapport ne s'écrit jamais dans `docs/` : il décrit la documentation à une date, et il est faux dès la première correction.

## 6. Après le rapport

L'utilisateur décide de chaque proposition : l'appliquer, l'adapter ou l'écarter. Une règle commune se compacte d'abord dans `polyaero-platform`. Toute modification suit le skill `git-workflow`.

Si le rapport est gardé dans une issue, ses décisions y sont notées : une case par proposition retenue, et celles qui sont écartées avec leur raison. Les Pull Requests y renvoient (`Refs #NN`), et la dernière la ferme (« Travail restant » de `CONTRIBUTING.md`).
