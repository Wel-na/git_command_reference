# Git Command Reference
📘 Guide de Maîtrise Git & GitHub
Guide pratique et structuré pour comprendre **Git**, **GitHub** et les commandes essentielles du quotidien.
Ce document regroupe les concepts, commandes et workflows essentiels pour la gestion de version, basés sur les standards de l'industrie.

\---

## Sommaire

* [Git vs GitHub](#git-vs-github)
* [Cycle de travail Git](#cycle-de-travail-git)
* [Premieres commandes avec Git](#premieres-commandes-avec-git)
* [Connecter un depot local a GitHub](#connecter-un-depot-local-a-github)
* [Cloner un depot distant](#cloner-un-depot-distant)
* [Git Credential Manager](#git-credential-manager)
* [Voir le statut des modifications](#voir-le-statut-des-modifications)
* [Ajouter des fichiers avec `git add`](#ajouter-des-fichiers-avec-git-add)
* [Annuler des changements](#annuler-des-changements)
* [Configurer Git et creer des commits](#configurer-git-et-creer-des-commits)
* [Supprimer et restaurer des fichiers](#supprimer-et-restaurer-des-fichiers)
* [Consulter l'historique](#consulter-lhistorique)
* [Branches et fusions](#branches-et-fusions)
* [Comprendre les conflits de fusion](#comprendre-les-conflits-de-fusion)
* [Naviguer dans l'historique et comparer](#naviguer-dans-lhistorique-et-comparer)
* [Travailler avec les depots distants](#travailler-avec-les-depots-distants)
* [Mettre du travail de cote avec le stash](#mettre-du-travail-de-cote-avec-le-stash)
* [Annuler un commit proprement avec `git revert`](#annuler-un-commit-proprement-avec-git-revert)
* [Collaborer avec les Pull Requests](#collaborer-avec-les-pull-requests)
* [Diagnostic rapide](#diagnostic-rapide)
* [Creer ou publier un depot GitHub](#creer-ou-publier-un-depot-github)
* [Connexion SSH](#connexion-ssh)
* [Simplifier les connexions SSH](#simplifier-les-connexions-ssh)

\---

## Git vs GitHub

**Git** est un système de gestion de versions.  
Il enregistre l'historique de vos fichiers localement, ce qui permet de :

* suivre chaque modification ;
* conserver plusieurs versions d'un projet ;
* revenir a un état précèdent si nécessaire.

**GitHub** est une plateforme en ligne d'hébergement de dépôts Git.  
Elle sert de point central pour :

* sauvegarder un dépôt a distance ;
* partager le code ;
* collaborer ;
* faire relire et fusionner des changements.

### Image simple

* **Git** = l'outil local de versioning
* **GitHub** = la plateforme distante de collaboration

\---

## Cycle de travail Git

En résume :

1. Vous travaillez dans votre **working directory** (répertoire de travail local).
2. Vous **staging** vos changements.
3. Vous **commit** ces changements dans votre depot local.
4. Facultativement, vous **push** vers un dépôt distant pour sauvegarder ou partager.

### Schéma mental

`Working Directory -> Staging Area -> Local Repository -> Remote Repository`

\---

## Premières commandes avec Git

### Vérifier l'installation

```bash
git --version
```

> Le fichier source mentionnait `git -v`, mais la commande standard recommandee est `git --version`.

### Créer un projet de test

Exemple de séquence (plutôt orientée Unix/macOS/Linux a cause de `touch`) :

```bash
mkdir fortest
cd fortest
touch un.txt
touch deux.txt
mkdir myfolder
cd myfolder
touch trois.txt
```

### 🛠️ Initialiser Git

```bash
git init
```

Cette commande cree un **depot Git local** dans le dossier courant.

Pour verifier le resultat :

```bash
ls -la
```

\---

## 🔗 Connecter un dépôt local a GitHub

Dans votre projet local :

```bash
git remote add origin https://user@github.com/TON-USER/TON-REPO.git
```

Verifier la branche actuelle :

```bash
git branch
```

Si besoin, renommer la branche principale en `main` :

```bash
git branch -M main
```

Ajouter et committer les fichiers si ce n'est pas deja fait :

```bash
git add .
git commit -m "Initial commit"
```

Envoyer sur GitHub :

```bash
git push -u origin main
```

### Cas courant

Si le depot GitHub contient deja un `README`, une licence ou un commit initial :

```bash
git pull origin main --rebase
git push -u origin main
```

\---

## Cloner un depot distant

### Cloner en forcant une identite specifique

Lorsque vous clonez un depot, vous pouvez inclure l'identite avant le nom de domaine pour aider Git et Git Credential Manager a utiliser le bon compte.

Au lieu de :

```bash
git clone https://example.com/open-source/library.git
```

Utilisez :

```bash
git clone https://contrib123@example.com/open-source/library.git
```

Puis :

```bash
cd happyday
ls -la
```

### Si le dépôt est déjà clone

Vous pouvez mettre a jour l'URL distante :

```bash
git remote set-url origin https://employee9999@example.com/big-company/secret-repo.git
```

\---

## Git Credential Manager

Commande source :

```bash
git credential-manager github --help
```

Informations utiles :

```text
Description:
  Commands for interacting with the GitHub host provider

Usage:
  git-credential-manager github \[command] \[options]

Options:
  --no-ui         Do not use graphical user interface prompts
  -?, -h, --help  Show help and usage information

Commands:
  list              List all known GitHub accounts.
  login             Add a GitHub account.
  logout <account>  Remove a GitHub account.
```

\---

## 📊 Voir le statut des modifications

```bash
git status
```

Cette commande affiche l'etat actuel du depot :

* fichiers modifies ;
* fichiers stagges ;
* fichiers non suivis ;
* branche courante.

\---

## ➕ Ajouter des fichiers avec `git add`

### Tout le projet

```bash
git add --all
git add -A
```

Stage toutes les modifications :

* ajouts ;
* modifications ;
* suppressions.

### Repertoire courant

```bash
git add .
```

Stage les changements du dossier courant et de ses sous-dossiers, y compris les suppressions.

### Fichiers visibles du dossier courant

```bash
git add \*
```

Ajoute les nouveaux fichiers et les fichiers modifies visibles, mais **pas** les suppressions.  
Comportement a utiliser avec prudence selon le shell.

### Un fichier precis

```bash
git add fichier.txt
git add myFolder/six.txt
```

### Tous les fichiers d'une extension dans le dossier courant

```bash
git add \*.txt
```

Ajoute les fichiers `.txt` du dossier courant uniquement, sans les sous-dossiers.

\---

## Annuler des changements

### Restaurer un fichier modifie

```bash
git restore premier.txt
git restore folder/
```

### Restaurer tout le depot

```bash
git restore .
```

### Retirer un fichier de la zone de staging

```bash
git restore --staged fichier.txt
```

### Retirer tous les fichiers de la zone de staging

```bash
git restore --staged .
```

### Revenir a un etat non stage

```bash
git reset
```

> `git reset` retire du staging, mais ne supprime pas les modifications locales du working directory.

\---

## Configurer Git et creer des commits

### Configurer un utilisateur global

```bash
git config --global user.name "Bidouille"
git config --global user.email "bidouille@gmail.com"
```

### Configurer un utilisateur local a un projet

```bash
git config --local user.name "Matt"
git config --local user.email "matt@monentreprise.net"
```

### Lire la configuration actuelle

```bash
git config --list
```

### Aide detaillee

Sous Linux/macOS :

```bash
man git-config
```

### Enregistrer les changements

```bash
git commit -m "I have made some changes to files"
```

### Revenir au commit precedent tout en gardant les changements

```bash
git reset HEAD
```

\---

## Supprimer et restaurer des fichiers

### Supprimer un fichier et stager la suppression

```bash
git rm four.txt
```

### Retirer seulement l'information stagee

```bash
git reset
```

### Restaurer completement a un etat precedent

```bash
git reset --hard
```

> Attention : `git reset --hard` efface les modifications locales non committees.

### Forcer la suppression d'un fichier modifie

```bash
git rm -f four.txt
```

### Retirer du suivi Git sans supprimer physiquement

```bash
git rm --cached four.txt
```

### Supprimer un dossier recursivement

```bash
git rm -r myfolder
```

\---

## Consulter l'historique

### Historique complet

```bash
git log
```

### Historique compact

```bash
git log --oneline
```

### Historique compact avec statut des fichiers

```bash
git log --oneline --name-status
```

Codes frequents :

* `A` = ajout
* `M` = modification
* `D` = suppression

\---

## Branches et fusions

Un point important : une nouvelle branche reprend exactement l'etat de la branche active au moment de sa creation.

### Voir les branches

```bash
git branch
```

L'asterisque `\*` indique la branche courante.

### Creer une branche

```bash
git branch frontend
```

### Changer de branche

```bash
git checkout frontend
```

Seuls les fichiers presents dans la branche courante sont visibles dans l'etat correspondant du projet.

### Creer et basculer en une seule commande

```bash
git checkout -b destination
```

### Fusionner des branches

Importer les changements de `main` dans `frontend` :

```bash
git checkout frontend
git merge main -m "import changes from main into frontend"
```

Puis reintegrer `frontend` dans `main` :

```bash
git checkout main
git merge frontend -m "import changes from frontend into main"
```

### Supprimer une branche locale

```bash
git branch -d nom\_de\_la\_branche
```

### Forcer la suppression d'une branche non mergee

```bash
git branch -D nom\_de\_la\_branche
```

### Supprimer une branche distante

```bash
git push origin --delete nom\_de\_la\_branche
```

> Remarque moderne : `git switch` peut remplacer `git checkout` pour les changements de branche, mais les commandes source ont ete conservees ici.

\---

## Comprendre les conflits de fusion

Un **merge conflict** apparait lorsque la meme zone d'un fichier a ete modifiee dans des branches differentes.

Git ne sait alors pas quelle version conserver.

### Resolution manuelle

1. Ouvrir le fichier en conflit.
2. Choisir la bonne version ou fusionner les deux.
3. Sauvegarder le fichier.
4. Ajouter le fichier.
5. Faire un commit.

\---

## Naviguer dans l'historique et comparer

### Voir les commits

```bash
git log --oneline
```

### Revenir temporairement sur un ancien commit

```bash
git checkout b52a404
```

### Comparer deux commits

```bash
git diff f0819e3 a81d08a
```

> Naviguer ainsi place generalement le depot en \*\*detached HEAD\*\* si vous n'etes pas sur une branche.

\---

## Travailler avec les depots distants

### Envoyer ses changements

```bash
git push origin main
git checkout staging
git push origin staging
```

### Recuperer les changements distants

```bash
git fetch
git merge
```

> En pratique, apres `git fetch`, on fusionne souvent explicitement la branche distante cible, par exemple `git merge origin/main`.

### Recuperer et fusionner en une seule commande

```bash
git pull
```

`git pull = git fetch + git merge`

### Bonne pratique recommandee

Faire une fois :

```bash
git push -u origin frontend-v0.0
```

L'option `-u` (`--set-upstream`) permet de :

* lier la branche locale a la branche distante ;
* simplifier les prochains `git push` ;
* simplifier les prochains `git pull`.

Ensuite, vous pourrez souvent utiliser simplement :

```bash
git push
git pull
```

### Verification apres le push

```bash
git branch -a
```

\---

## Mettre du travail de cote avec le stash

Le stash permet de sauvegarder temporairement un travail non termine pour changer de branche ou traiter autre chose.

### Sauvegarder temporairement

```bash
git stash
```

### Restaurer et retirer du stash

```bash
git stash pop
```

### Restaurer sans retirer du stash

```bash
git stash apply
```

### Voir la liste des stashes

```bash
git stash list
```

### Restaurer un stash precis

```bash
git stash pop stash@{0}
git stash apply stash@{0}
```

### Supprimer un stash manuellement

```bash
git stash drop
```

\---

## Annuler un commit proprement avec `git revert`

`git revert` permet de revenir a l'etat precedant un commit **en creant un nouveau commit**, sans reecrire l'historique.

```bash
git log --oneline
git revert 0429467
```

\---

## Collaborer avec les Pull Requests

Une **Pull Request (PR)** est une demande de fusion de vos changements dans une autre branche, souvent `main`.

L'idee est simple :

* vous travaillez dans votre propre branche ;
* vous poussez cette branche sur GitHub ;
* vous demandez une revue ;
* si tout est valide, la branche est fusionnee.

### Exemple de logique GitHub

Dans l'interface GitHub :

* onglet **Pull requests**
* `base: main`
* `compare: feature`

Autrement dit :

> "J'ai fait des changements dans la branche de developpement et je demande leur fusion dans `main`."

\---

## Diagnostic rapide

### Verifier l'URL distante actuelle

```bash
git remote -v
```

### Passer en SSH

```bash
git remote set-url origin git@github.com:Matrak/happyday.git
```

### Verifier le compte utilise

```bash
git config --global user.name
git config --global user.email
```

\---

## Creer ou publier un depot GitHub

### Creer un nouveau depot depuis la ligne de commande

```bash
echo "# nawel\_portfolio" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/6net7/nawel\_portfolio.git
git push -u origin main
```

### Publier un depot existant

```bash
git remote add origin https://github.com/6net7/nawel\_portfolio.git
git branch -M main
git push -u origin main
```

\---

## 🔐 Connexion SSH

### Creer une paire de cles

```bash
ssh-keygen -o -a 100 -t ed25519 -f \~/.ssh/id\_ed25519 -C "fred@bzhack.bzh"
```

### Copier la cle publique vers un serveur

```bash
ssh-copy-id admin@192.168.6.66
```

Avec les parametres par defaut, `ssh-copy-id` :

* utilise le fichier `id\_rsa.pub` ou la cle publique indiquee selon votre configuration ;
* ajoute la cle dans `authorized\_keys` du compte distant ;
* demande generalement le mot de passe une derniere fois.

> Sur certains systemes modernes, si vous creez une cle `ed25519`, c'est cette cle qui sera utilisee plutot que `id\_rsa.pub`.

\---

## Simplifier les connexions SSH

Pour gerer plus facilement plusieurs serveurs, vous pouvez utiliser :

* des alias shell ;
* ou, de facon plus propre, un fichier `\~/.ssh/config`.

### Creer le fichier de configuration SSH

```bash
mkdir -p \~/.ssh \&\& chmod 700 \~/.ssh
touch \~/.ssh/config
chmod 600 \~/.ssh/config
```

### Structure type

```sshconfig
Host mon-serveur
    HostName 192.168.6.66
    User admin
    IdentityFile \~/.ssh/id\_ed25519
```

Ensuite, la connexion devient :

```bash
ssh mon-serveur
```

\---

## Rappels utiles

* `git init` initialise un depot local.
* `git add` prepare les changements.
* `git commit` enregistre un snapshot local.
* `git push` envoie les commits vers le depot distant.
* `git pull` recupere et fusionne les changements distants.
* `git stash` met temporairement des modifications de cote.
* `git revert` annule un commit sans casser l'historique.

\---

## Licence

Ce document est fourni comme aide-memoire pedagogique sur Git, GitHub et SSH.

