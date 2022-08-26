# Notes sur git

## Outils

Il est possible d'utiliser les lignes de commande **git** ou **github**, mais aussi de passer depuis le site web de [github](https://github.com) ou par différents [interfaces graphiques](https://git-scm.com/download/gui/windows).

Pour télécharger git s'il n'est pas présent sur l'ordinateur, voici un lien pour le [site officiel](https://git-scm.com/downloads).
Des vidéos tuto ainsi que des manuels de référence sont aussi présents.
Je me suis basé sur ce [tutoriel](https://code.visualstudio.com/docs/editor/versioncontrol) pour les extensions VSCode.

Des aides mémoires ont été créés pour faciliter la compréhension de git

- [aide 0](https://www.atlassian.com/fr/git/tutorials/setting-up-a-repository) (Introduction au versionning avec Git)
- [aide 1](https://training.github.com/downloads/fr/github-git-cheat-sheet/) (Aide mémoire ou *cheat sheet* git)
- [aide 2](https://ndpsoftware.com/git-cheatsheet.html#loc=workspace;) (Visualisation intéractive de l'espace de travail avec **git**)
- [aide 3](https://docs.microsoft.com/en-us/visualstudio/version-control/git-fetch-pull-sync?view=vs-2022) (Git avec VisualStudio 2022)
- [aide 4](https://phoenixnap.com/kb/git-fetch) (Explications sur la synchronisation des dépôts avec git fetch et merge)

## Commencer à collaborer

- Installez **git** si ce n'est pas le cas sur votre poste ([Télécharger git-2.36.0-64-bit](https://github.com/git-for-windows/git/releases/download/v2.36.0.windows.1/Git-2.36.0-64-bit.exe)) ou passez par l'invite de commande winget `winget install --id Git.Git -e --source winget`
- Si vous préférez utiliser une interface graphique une liste des IHM est fournie plus haut. (ex: GitHubDesktop)
- Une fois l'installation de **git** terminée, configurez votre nom et adresse email pour la collaboration

```powershell
git config --global user.name <nom>
git config --local user.email <email>
```

A l'aide de votre invite de commande ou de votre interface

- Mettez vous dans le répertoire de votre choix en local
- Clonez le dépôt HighGestCo avec `git clone https://gitea.normasys.com/Pole-dotnet/HighGestCo.git`
- Assurez vous de ne pas travailler sur la branche principale (la production) ni sur la branche de dev (recette) mais sur une branche de fonctionnalité ou de correction de bug (vérifiez dans quelle branche vous vous situez avec la commande `git status`)
- Une fois vos modifications terminées, vous pouvez indexer votre travail dans le répertoire de **staging** grâce aux commandes `git add -u` ou  `git add .` ou `git add <nom-de-fichier>`
- Vous pouvez retirer des fichiers de l'index grâce à `git rm <fichier>` ou `git reset HEAD` (réinitialise l'index du prochain commit mais pas l'espace de travail)
- Puis effectuer un **commit** pour enregistrer vos changements dans le dépôt local avec `git commit -m "<Message-indicant-les-changements-que-vous-avez-opérés-dans-ce-commit>"`
- Vous pouvez annuler votre **commit** avec `git reset --hard` (attention, tous les changements ultérieurs au précédent commit seront annulés y compris dans votre espace de travail), `git reset --soft HEAD^` annule le dernier commit en laissant les modification dans l'index
- Il faut toujours se synchroniser avec le dépôt distant avant de **push**. Vous pouvez regarder si des changements ont été faits sur le dépôt distant central à l'aide de `git fetch` (la commande télécharge du contenu mais contrairement à `git pull` elle n'effectue pas fusion des dépôts automatiquement)
- Pour approuver les changements et les fusioner avec la branche locale vérifiez les changements puis faites un **merge**

```powershell
git checkout <branche>
git log origin/<branche>
git merge origin/<branche>
```

- Pour envoyer votre dépôt local vers le dépôt distant vous pouvez faire `git push <dépôt-distant> <branche-à-envoyer>`, le serveur est donc mis à jour avec tous les commits qui ont été fait sur la branche envoyée (de base le dépôt distant se nomme `origin`)

Dans le cas où vous n'êtes pas prêt à faire un commit mais que vous devez travailler sur une fonctionnalité plus urgente, vous pouvez enregistrer votre espace de travail dans un **stash**.
`git stash` permet de prendre vos modifications non commités (espace de travail + index ou staging).
Pour ajouter les changements non trackés (nouveaux fichiers par exemple) l'option `-u` ou `--include-untracked` permet d'envoyer les fichiers non trackés dans le stash.
Pour les fichiers ignorés (dll, répertoire bin/) il est possible de les inclure avec l'option `-a` ou `--all`

Pour réappliquer les modifications enregistrées dans le stash il suffit de faire `git stash pop` (le stash est supprimé après application) ou `git stash apply` (le stash est conservé après restitution dans l'espace de travail)

## Synchroniser le dépôt local avant le push

La commande `git fetch` récupère les commits, fichiers, branches et tags en provenance d'un repo distant

```powershell
git fetch <options> <repo distant> <branche-distante>
```

Git isole le contenu téléchargé du code local. Fetch est donc une solution sécurisée pour faire la revue des informations avant d'opérer un commit dans la branche locale.

- Créez un répertoire `mkdir <nom-du-repertoire>`
- Dans ce répertoire initialisez un répertoire git local 

```powershell
cd <nom-du-repertoire>
git init
```

- Ajoutez un URL du dépôt distant en local `git remote add <alias-du-remote> <URL-du-remote>` par exemple `git remote add origin https://gitea.normasys.com/Pole-dotnet/HighGestCo.git`
- Confirmez que le dépôt est bien lié `git remote -v`
- Récupérez un dépôt distant `git fetch <alias-du-remote>` par exemple `git fetch origin`
- Listez toutes les branches chargées `git branch -r`
- Listez tous les tags `git tag`

- Pour récupérer une branche particulière du repo distant `git fetch <alias-du-repo> <branche-à-récupérer>` par exemple `git fetch origin branche-dev-v01`
- La commande ne récupère que le contenu que de cette branche spécifique. Pour regarder les modifications apportées `git checkout -b <nouvelle-branche-locale> <alias>/<branche-distante>`

Pour synchroniser le dépôt local avant le push

- Récupérez le dépôt remote `git fetch <nom-du-remote>`
- Comparez la branche locale au remote en listant les commits de différence `git log --oneline <branche-locale>..<alias-du-remote>/<branche-distante>`

```powershell
# Par exemple
git log --oneline branche-locale..origin/branche-dev-v01
```

- Faitez un checkout de la branche locale où vous souhaitez que le merge soit appliqué `git checkout <branch-locale>`
- Synchronisez la branche locale avec la commande merge `git merge <alias-du-repo>/<branche-distante>`

Il ne reste plus qu'à push vos modifications
