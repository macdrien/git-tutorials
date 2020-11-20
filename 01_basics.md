# 1. GIT les bases

## 1.1. Sommaire

- [1. GIT les bases](#1-git-les-bases)
  - [1.1. Sommaire](#11-sommaire)
  - [1.2. Elements theoriques](#12-elements-theoriques)
    - [1.2.1. Les trois etats de prise en compte d'un fichier](#121-les-trois-etats-de-prise-en-compte-dun-fichier)
  - [1.3. Versionning en local](#13-versionning-en-local)
    - [1.3.1. Creation d'un repo local](#131-creation-dun-repo-local)
    - [1.3.2. Ajout d'un fichier dans l'index](#132-ajout-dun-fichier-dans-lindex)
    - [1.3.3. Versionner un fichier](#133-versionner-un-fichier)
  - [S'informer sur l'etat du repository local](#sinformer-sur-letat-du-repository-local)
    - [Demander l'etat actuel du repository](#demander-letat-actuel-du-repository)
    - [Consulter l'historique du repository distant](#consulter-lhistorique-du-repository-distant)
  - [1.4. Travailler avec un repository distant](#14-travailler-avec-un-repository-distant)
    - [1.4.1. Recuperer un repository distant](#141-recuperer-un-repository-distant)
    - [1.4.2. Lier un repository local avec un repository distant](#142-lier-un-repository-local-avec-un-repository-distant)
    - [1.4.3. Envoyer des commits locaux vers le repository distant](#143-envoyer-des-commits-locaux-vers-le-repository-distant)
    - [1.4.4. Recuperer des commits depuis un repository distant](#144-recuperer-des-commits-depuis-un-repository-distant)

## 1.2. Elements theoriques

### 1.2.1. Les trois etats de prise en compte d'un fichier

1. Working directory : La modification est présente dans le dossier, mais elle n'est pas versionnée.
2. Index / Staged : La modification est référencée par git mais elle n'est pas encore versionnée.
3. Repository : La modification est référencée et est versionnée par git (présente dans un commit).

## 1.3. Versionning en local

### 1.3.1. Creation d'un repo local

- Pour versionner le dossier courant: `git init`
- Pour versionner un autre dossier: `git init chemin/du/dossier`

### 1.3.2. Ajout d'un fichier dans l'index

`git add chemin/fichier_a_ajouter.txt`

### 1.3.3. Versionner un fichier

*Prérequis*: Le fichier doit avoir été préalablement placé dans l'index git.

2 possibilités:

- `git commit -m "Message descriptif"` : La méthode la plus rapide en donnant un titre descriptif au commit
- `git commit` : Méthode plus longue. Elle va ouvrir l'éditeur lié à git pour pouvoir écrire un message complet.

---

## S'informer sur l'etat du repository local

### Demander l'etat actuel du repository

`git status`

Cette commande affiche un ensemble d'informations sur le repository local. On trouvera entre autre:

- Les fichiers dans le working directory
- Les fichers dans l'index
- La différence d'historique de commits avec le repository distant
- Certaines commandes usuelles avec leur effet.

### Consulter l'historique du repository distant

`git log`

*Notes*: Si l'historique est trop long pour rentrer dans une fenêtre, git log entre dans un état particulier. Dans cet état, on peut descendre et monter dans l'historique avec les flèches directionnelles. Pour quitter cet état, il suffit d'appuyer sur la touche 'Q'.

---

## 1.4. Travailler avec un repository distant

### 1.4.1. Recuperer un repository distant

`git clone http://adresse.com/du/repository.git`

*Attention*: Si tu n'es pas marqué comme contributeur du projet, tu ne pourra pas envoyer tes modifications. Dans un premier temps, utilise principalement tes repositories.

### 1.4.2. Lier un repository local avec un repository distant

`git remote add <nom> http://adresse.com/du/repository.git`

*Notes*:

1. Pour le lien principal, la convention veut qu'on le nomme "origin".
2. Le repository distant doit déjà exister. Sinon la commande renverra une erreur.

### 1.4.3. Envoyer des commits locaux vers le repository distant

`git push <nom du repository distant>`

*Note*: On peut omettre le nom du repository distant. Dans ce cas, l'envoi sera fait à l'adresse du repository appelé origin.

Si la branche (on y vient bientôt) sur laquelle on travaille n'existe pas encore sur le repository distant, il faut utiliser la version suivante de la commande:

`git push --set-upstream <nom du repository distant (origin)> nom-de-la-branche-a-publier`

Cette commande est notamment utilisée à la suite d'un `git remote add ...`.

### 1.4.4. Recuperer des commits depuis un repository distant

`git pull <nom du repository distant>`

*Note*: On peut omettre le nom du repository distant. Dans ce cas, la demande d'information sera faite à l'adresse du repository appelé origin.
