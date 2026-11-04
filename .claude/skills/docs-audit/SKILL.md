---
name: docs-audit
description: "Audite toute la documentation du dépôt (docs/, README.md, AGENTS.md, CLAUDE.md, CONTRIBUTING.md, .claude/) et les renvois du code : règles non respectées, renvois à polyaero-platform et aux issues, contradictions et redondances. Produit un rapport dans la conversation, gardé dans une issue du dépôt avec l'accord de l'utilisateur, sans rien corriger. À lancer à la main, à la demande d'un membre de l'équipe."
disable-model-invocation: true
---

# Audit de la documentation

Ce skill dit comment auditer. Il ne contient aucune règle : les règles sont dans les sources lues à l'étape 1, et chaque constat cite celle qu'il applique, avec son fichier et sa ligne. Un écart qui ne viole aucune règle écrite n'est pas un constat : s'il paraît gênant, il va dans les questions ouvertes du rapport, comme proposition de règle. Il en va de même d'un fait constaté hors de la documentation, comme un réglage GitHub, qui contredit un document.

## Périmètre

Les fichiers suivis par git, dans l'état de l'arbre de travail : `docs/`, `README.md`, `AGENTS.md`, `CLAUDE.md`, `CONTRIBUTING.md` et `.claude/`. Si l'utilisateur limite l'audit (un dossier, les fichiers d'une branche), n'auditer que ceux-là, mais lire quand même les références dont ils dépendent.

Les autres fichiers suivis par git, comme le code et la configuration, ne passent que par les renvois de la passe mécanique : `ADR-NNNN`, `ADR-NNNN of <dépôt>`, `#NNN of <dépôt>` et `TODO(#NNN)`, sous leur forme anglaise (« Renvois entre dépôts » de `docs/README.md`). Ils ne sont pas lus en entier, et la passe de jugement ne les concerne pas.

Ne sont pas des renvois à vérifier : les exemples (introduits par « par exemple », ou qui illustrent un format), les fichiers `_modele.md`, les marques à remplacer, et les chemins que la documentation annonce comme pas encore créés.

## 1. Lire les références

- **Les règles communes et les décisions communes**, dans `polyaero-platform` : `docs/README.md`, le `README.md` de chaque dossier de `docs/`, `README.md`, `CONTRIBUTING.md`, `AGENTS.md`, les skills de `.claude/skills/`, l'index de `docs/08_decisions`, puis chaque ADR auquel le dépôt renvoie.
- **Les règles propres au dépôt** : ses ADR, son glossaire et les `README.md` de ses dossiers.

Les autres dépôts se lisent sur `main`, leur version publiée, et jamais sur une copie locale, qui peut être en cours de modification. Le dépôt audité se lit dans l'arbre de travail : dans `polyaero-platform`, les règles communes sont donc celles de la branche courante.

```bash
R=repos/Polyaero-Hautes-Alpes/<dépôt>
gh api "$R/commits/main" --jq .sha                                    # commit lu, à citer dans le rapport
gh api "$R/git/trees/main?recursive=1" --jq '.tree[].path'            # liste des fichiers
gh api "$R/contents/<chemin>?ref=main" -H "Accept: application/vnd.github.raw" > <copie> || echo "ÉCHEC <chemin>"
```

Tester le code retour : en cas d'erreur, `gh api` écrit le message d'erreur à la place du fichier. Si GitHub n'est pas accessible, s'arrêter et le dire, sans se rabattre en silence sur une copie locale.

Les règles communes s'appliquent dans leur version de `polyaero-platform`, même quand le dépôt ne les a pas encore reportées. Un constat qui ne dépend que d'une règle pas encore reportée le dit (« règle à reporter »).

**Si le dépôt audité est `polyaero-platform`**, ses propres ADR sont les décisions communes, et les règles écrites pour un projet envers `polyaero-platform` ne s'appliquent pas à lui. Sa section « Dépendance entre dépôts » de `docs/08_decisions/README.md` les remplace.

## 2. Passe mécanique

Vérifier **chaque fichier**, avec des commandes (`grep`, `find`, `diff`), sans échantillonner :

- chaque convention vérifiable des sources : dossiers de `docs/` et leurs numéros, `README.md` présents, nommage et plan des documents, statuts, titres imposés, numéros d'issue des points à vérifier ;
- chaque index (ADR, user stories, procédures) : chaque fichier y figure, et chaque ligne a son fichier ;
- chaque lien Markdown relatif mène à un fichier existant. Un chemin du dépôt cité entre accents graves est cherché dans tout le dépôt, et n'est un constat, à confirmer, que s'il reste introuvable ;
- chaque renvoi existe : `ADR-NNNN` seul dans le dépôt courant, `ADR-NNNN de <dépôt>` (`of` dans le code) dans ce dépôt, `#NNN` seul dans les issues du dépôt courant, `#NNN de <dépôt>` (`of` dans le code) dans celles du dépôt nommé. Les issues de chaque dépôt cité se lisent en une fois, puis se croisent avec les renvois, sans un appel par renvoi :

  ```bash
  gh issue list -R Polyaero-Hautes-Alpes/<dépôt> --state all --limit 5000 --json number,state,stateReason,title,body > <issues>
  ```

  **Une issue fermée, citée comme encore en cours, signale un texte à relire** : un point à vérifier ou une question ouverte qui la cite encore, un `TODO(#NNN)` dans le code. Le plus souvent, la décision qu'elle attendait a été prise, ou le point a été vérifié, sans que le texte suive ;
- dans l'autre sens, chaque issue **ouverte** du dépôt qui se présente comme le point à vérifier ou la question ouverte d'un ADR (par exemple « Point à vérifier de l'ADR-NNNN » dans son texte) est encore citée par cet ADR. Sinon, soit l'issue est à fermer, soit le point a quitté l'ADR sans être vérifié ;
- le statut de chaque ADR : la date de sa dernière mise à jour n'est pas antérieure au dernier commit qui a modifié le fichier (`git log -1 --format=%cs -- <fichier>`). Un écart est un constat **à confirmer** : seul un changement de la décision demande de mettre à jour le statut (`docs/08_decisions/README.md`), pas une correction de forme ;
- les marques `<PROJET>` et `<À COMPLÉTER` restantes ;
- les termes de la colonne des termes à éviter du glossaire, quel que soit l'intitulé de la colonne. Seul un emploi dans le sens que le glossaire écarte est un constat.

## 3. Passe de jugement

Lire en entier tous les fichiers du périmètre, et chercher :

- **les contradictions** : un même sujet traité avec des valeurs, des décisions ou un périmètre différents, en priorité entre un résumé, dans l'un des documents que « Une information, un seul endroit » de `docs/README.md` y autorise, et sa référence ;
- **les redondances**, selon « Une information, un seul endroit » de `docs/README.md` : un résumé hors des documents prévus, sans référence citée, ou qui ajoute à sa référence ;
- **les renvois entre dépôts**, selon « Renvois entre dépôts » de `docs/README.md` : un ADR de `polyaero-platform` résumé au-delà d'une phrase, une décision commune prise dans le dépôt d'un projet, un renvoi vers le mauvais dépôt, un besoin envers `polyaero-platform` sans issue ;
- **le vocabulaire** : un terme du glossaire employé dans un autre sens.

Ne pas juger le style, ni le bien-fondé des décisions.

**Au-delà d'une trentaine de fichiers**, confier la lecture à des sous-agents, par lots d'une quinzaine de fichiers au plus, regroupés par sujet : un ADR avec les procédures et les user stories qui l'appliquent, les petits dossiers ensemble. Chaque fichier du périmètre, fichiers racine compris, est attribué à un lot. Chaque sous-agent reçoit les références dont son lot dépend. **Attendre le retour de tous les sous-agents** avant de croiser leurs résultats et de rédiger le rapport. Si l'un d'eux échoue, le rapport le dit et liste les fichiers non lus.

## 4. Rapport

Dans la conversation, sans rien corriger :

1. **Références lues** : dépôt, branche et commit.
2. **Contrôles effectués** : chaque contrôle de l'étape 2, avec son résultat, y compris « rien trouvé ».
3. **Constats**, dans cet ordre : contradictions, règles non respectées (autres que celles des catégories suivantes), renvois entre dépôts, vocabulaire, redondances. Un écart ne fait qu'un constat, dans la première de ces catégories qui s'applique. Un même défaut répété dans plusieurs fichiers fait un seul constat, qui liste les fichiers. Pour chacun : fichier et ligne, constat, règle appliquée (fichier et ligne), correction proposée, gravité (**bloquant** : contradiction avec une référence, renvoi faux ; **nettoyage** : le reste) et confiance (**sûr** ou **à confirmer**).
4. **Questions ouvertes** : écarts qu'aucune règle écrite ne couvre, règles ambiguës ou contradictoires entre elles, faits hors de la documentation.

## 5. Garder le rapport

Un rapport resté dans la conversation se perd avec elle. Proposer à l'utilisateur de le garder dans une issue du dépôt audité, intitulée « Audit de la documentation du AAAA-MM-JJ », qui reprend le rapport entier, et ne la créer qu'avec son accord explicite. Si le rapport dépasse la taille d'un texte d'issue, la suite va dans des commentaires de l'issue, dans l'ordre.

Le rapport ne s'écrit jamais dans `docs/` : c'est l'état de la documentation à une date, faux dès la première correction.

## 6. Après le rapport

L'utilisateur décide de chaque constat : corriger dans ce dépôt, corriger la règle à sa source, ou ignorer. Une règle commune se corrige d'abord dans `polyaero-platform`, et un besoin envers `polyaero-platform` est une issue de ce dépôt (« Renvois entre dépôts » de `docs/README.md`). Toute correction suit le skill `git-workflow`.

Si le rapport est gardé dans une issue, ses décisions y sont notées : une case par constat retenu, et ceux qu'il ignore avec leur raison. Les Pull Requests de correction y renvoient (`Refs #NN`), et la dernière la ferme (« Travail restant » de `CONTRIBUTING.md`).
