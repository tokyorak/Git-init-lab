# Intro - Créer un dépôt


## Introduction

Un dépôt Git est un entrepot virtuel qui permet l'enregistrement des versions du code et d'y accéder au besoin.

C'est un système de contrôle de version distribué, cela veut dire qu'un clone en local du projet est un référentiel complet. Il est possible de travailler dessus hors connexion et à distance, pas besoin d'être connecté à un quelconque serveur avant de pourvoir faire des modifications.

Chaque fois qu'un travail est enregistré, Git crée un instantanné de tous les fichiers à un moment donné. Si un fichier n'a pas changé entre deux instantannés, Git utilise le fichier précédemment stocké. Ces instantannés sont aussi appelé **Commit**.

Les commits créent des liens vers d'autres commits, formant un graphique de l'historique de développement. Il est possible de rétablir le code à un commit précédent.

Avant de créer son dépôt en local, il est nécessaire de procéder à la configuration de son Git "local".

```powershell
git config --global user.name "Nom prénom"
git config --global user.email "mon.email@normasys.com"
git config --global init.defaultBranch main
```

Avec ces 3 lignes de code, on configure notre nom d'utilisateur sur notre poste local, notre adresse email ainsi que le nom par défaut de la branche principale lorsque qu'on initie la création d'un répertoire Git.

## Créer un dépôt Git

Pour créer un nouveau dépôt Git dans notre répertoire, il faut utiliser la commande suivante:

```powershell
cd /chemin/projet
git init
```

Pour créer un dépôt Git à partir d'un dépôt existant déjà centralisé, la commande la plus courante est **clone**:

```powershell
git clone <url-dépôt>
```

Cette commande récupère la dernière version des fichiers du dépôt distant sur la branche principale et les ajoute à un nouveau dossier.

## Enregistrer des changements dans le dépôt

Une fois le dépôt initialisé ou cloné, il est possible de travailler sur la copie locale. Il est donc maintenant possible de faire un commit dans ce même dépôt.

On va par exemple ajouter un fichier `TestCommit.txt` dans lequel on va mettre un texte de test puis faire un instantanné de notre dépôt.

```powershell
cd /chemin/projet
echo "Hello Git" > TestCommit.txt
git add TestCommit.txt
git commit -m "Ajout de TestCommit.txt dans le repo"
```

- On s'assure d'être dans le chemin où la modification sera opérée
- On crée un nouveau fichier "TestCommit.txt" dans lequel on insère le texte "Hello Git"
- On ajoute le fichier TestCommit.txt dans l'index (car il vient d'être créé et donc il n'est pas traqué par Git)
- Une fois ajouté à l'index on peut faire un instantanné du projet indexé (avec un message pour repérer plus facilement les différentes modifications apportées dans le commit)

Dans le cas où le dépôt est cloné depuis un repo Git distant, le gestionnaire de version est déjà configuré pour envoyer le code vers l'URL cloné. Pour partager les contributions vers le serveur distant il suffit de faire la commande `push`.

```powershell
# s'il s'agit du premier push
git push -u <alias-repo-distant> <nom-de-la-branche-locale>
```

```powershell
# si le premier push depuis la branche local a déjà été fait et que la branche distante n'a pas été supprimée
git push -u <alias-repo-distant> <nom-de-la-branche-locale>
```

S'il s'agit d'une nouveau dépôt Git (non cloné) ou que le répertoire distant a changé d'URL, il faut configurer l'alias du répertoire distant avant d'effectuer l'opération de `push`

```powershell
git remote add <alias-repo-distant> <url-repo-distant>
```

Git possède un mécanisme d'enregistrement supplémentaire à part le commit, il s'agit du `stash`. C'est un espace de stockage temporaire pour les changements qui ne sont pas prêts à être envoyés dans un commit.
