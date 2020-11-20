# 1. GIT les bases

## 1.1. Sommaire

- [1. GIT les bases](#1-git-les-bases)
  - [1.1. Sommaire](#11-sommaire)
  - [1.2. Elements prealable](#12-elements-prealable)
    - [Installer git](#installer-git)
    - [Les retours ecrits de git](#les-retours-ecrits-de-git)
    - [1.2.1. Les trois etats de prise en compte d'un fichier](#121-les-trois-etats-de-prise-en-compte-dun-fichier)
  - [1.3. Versionning en local](#13-versionning-en-local)
    - [1.3.1. Creation d'un repo local](#131-creation-dun-repo-local)
    - [1.3.2. Ajout d'un fichier dans l'index](#132-ajout-dun-fichier-dans-lindex)
    - [1.3.3. Versionner un fichier](#133-versionner-un-fichier)
  - [1.4. S'informer sur l'etat du repository local](#14-sinformer-sur-letat-du-repository-local)
    - [1.4.1. Demander l'etat actuel du repository](#141-demander-letat-actuel-du-repository)
    - [1.4.2. Consulter l'historique du repository distant](#142-consulter-lhistorique-du-repository-distant)
  - [1.5. Travailler avec un repository distant](#15-travailler-avec-un-repository-distant)
    - [1.5.1. Recuperer un repository distant](#151-recuperer-un-repository-distant)
    - [1.5.2. Lier un repository local avec un repository distant](#152-lier-un-repository-local-avec-un-repository-distant)
    - [1.5.3. Envoyer des commits locaux vers le repository distant](#153-envoyer-des-commits-locaux-vers-le-repository-distant)
    - [1.5.4. Recuperer des commits depuis un repository distant](#154-recuperer-des-commits-depuis-un-repository-distant)
  - [1.6. Travailler avec les branches](#16-travailler-avec-les-branches)
    - [1.6.1. Lister les branches](#161-lister-les-branches)
    - [1.6.2. Creer une branche](#162-creer-une-branche)
    - [1.6.3. Supprimer une branche](#163-supprimer-une-branche)
    - [1.6.4. Changer de branche](#164-changer-de-branche)
    - [1.6.5. Fusionner deux branches](#165-fusionner-deux-branches)
      - [1.6.5.1. Resoudre un conflit](#1651-resoudre-un-conflit)
  - [1.7. Liens utiles](#17-liens-utiles)

## 1.2. Elements prealable

### Installer git

[https://git-scm.com/downloads](https://git-scm.com/downloads)

La procédure d'installation sera détaillée par une suite de fenêtre.  
Et pour les linuxiens, on oublie pas l'éternel `apt-get install git` qui fonctionne aussi très bien.

Une fois installer, rends toi sur ton terminal. Commence par vérifier que git est bien installer avec `git --version`. Ensuite, il va falloir que tu indique à git qui tu es afin qu'il puisse identifier dans son versionning qui fait quoi. Pour cela, voici les deux commandes à exécuter:

`git config --global user.name "tonnom ou ton pseudo`

et

`git config --global user.email "ton@adresse.mail"`

Par la suite, il sera intéressant pour toi de te créer un compte github, il faudra que tu utilise la même adresse mail que celle saisie ici. Sinon git risque de te refuser des mises à jours de tes projets en croyant que tu n'es pas qui tu prétends être.

### Les retours ecrits de git

Git est très explicite dans le texte qu'il affiche sur la ligne de commande. Souvent, la solution à un problème ou une interrogation peut être trouvée en lisant les retours de git (description d'erreur ou message informatif). Si le problème persiste, la section [liens utiles](#17-liens-utiles) t'amènera vers des articles ou des vidéos expliquant le fonctionnement de git plus ou moins en profondeur (en anglais pour certains, en français pour d'autres).

### 1.2.1. Les trois etats de prise en compte d'un fichier

1. Working directory : La modification est présente dans le dossier, mais elle n'est pas versionnée.
2. Index / Staged : La modification est référencée par git mais elle n'est pas encore versionnée.
3. Repository : La modification est référencée et est versionnée par git (présente dans un commit).

## 1.3. Versionning en local

### 1.3.1. Creation d'un repo local

- Pour versionner le dossier courant: `git init`
- Pour versionner un autre dossier: `git init chemin/du/dossier`

### 1.3.2. Ajout d'un fichier dans l'index

`git add chemin/fichier_a_ajouter.txt chemin/sous-dossier/fichier_a_ajouter_2.txt`

Cette commande permet de déplacer un fichier cible du Working directory vers ton Index.

*Note*: Si tu es sûr de toi et que tu veux ajouter tous les fichiers d'un dossier, tu peux simplement indiquer le nom du dossier cible. Dans le cas de l'exemple au desss, si il n'y a que c'est deux fichiers qui ont été modifié, on peut faire la commande `git add chemin`.  
Si tu es à la racine de ton projet et que tu veux tout ajouter d'un coup, alors la commande sera `git add .` (le point est essentiel ici).

### 1.3.3. Versionner un fichier

*Prérequis*: Le fichier doit avoir été préalablement placé dans l'index git (cf. [Ajout d'un fichier dans l'index](#132-ajout-dun-fichier-dans-lindex)).

2 possibilités:

- `git commit -m "Message descriptif"` : La méthode la plus rapide en donnant un titre descriptif au commit
- `git commit` : Méthode plus longue. Elle va ouvrir l'éditeur lié à git pour pouvoir écrire un message complet.

---

## 1.4. S'informer sur l'etat du repository local

### 1.4.1. Demander l'etat actuel du repository

`git status`

Cette commande affiche un ensemble d'informations sur le repository local. On trouvera entre autre:

- Les fichiers dans le working directory
- Les fichers dans l'index
- La différence d'historique de commits avec le repository distant
- Certaines commandes usuelles avec leur effet.

### 1.4.2. Consulter l'historique du repository distant

`git log`

*Notes*: Si l'historique est trop long pour rentrer dans une fenêtre, git log entre dans un état particulier. Dans cet état, on peut descendre et monter dans l'historique avec les flèches directionnelles. Pour quitter cet état, il suffit d'appuyer sur la touche 'Q'.

---

## 1.5. Travailler avec un repository distant

### 1.5.1. Recuperer un repository distant

`git clone http://adresse.com/du/repository.git`

*Attention*: Si tu n'es pas marqué comme contributeur du projet, tu ne pourra pas envoyer tes modifications. Dans un premier temps, utilise principalement tes repositories.

### 1.5.2. Lier un repository local avec un repository distant

`git remote add <nom> http://adresse.com/du/repository.git`

*Notes*:

1. Pour le lien principal, la convention veut qu'on le nomme "origin".
2. Le repository distant doit déjà exister. Sinon la commande renverra une erreur.

### 1.5.3. Envoyer des commits locaux vers le repository distant

`git push <nom du repository distant>`

*Note*: On peut omettre le nom du repository distant. Dans ce cas, l'envoi sera fait à l'adresse du repository appelé origin.

Si la branche (on y vient bientôt) sur laquelle on travaille n'existe pas encore sur le repository distant, il faut utiliser la version suivante de la commande:

`git push --set-upstream <nom du repository distant (origin)> nom-de-la-branche-a-publier`

Cette commande est notamment utilisée à la suite d'un `git remote add ...`.

*Note 2*: Si c'est la première fois que tu tente d'envoyer des commits depuis ta machine, git te demandera tes identifiants pour la plateforme que tu vise. Je te conseille donc dans un premier temps d'utiliser une seule plateforme (le changement d'identifiants est embêttant). Et dans cette optique, Github est parfait pour une première utilisation. On verra plus tard comment se débarrasser de cette limitation.

### 1.5.4. Recuperer des commits depuis un repository distant

`git pull <nom du repository distant>`

*Note*: On peut omettre le nom du repository distant. Dans ce cas, la demande d'information sera faite à l'adresse du repository appelé origin.

---

## 1.6. Travailler avec les branches

Par défaut, la branche master est créée. Il s'agit de la branche principale du projet.

### 1.6.1. Lister les branches

`git branch`

### 1.6.2. Creer une branche

`git branch nom-de-la-nouvelle-branche`

*Note*: Créer une nouvelle branche ne fait pas se déplacer sur cette branche.

### 1.6.3. Supprimer une branche

`git branch -d nom-de-la-nouvelle-branche`

### 1.6.4. Changer de branche

`git checkout branche-destination`

*Note*: En ajoutant l'option '-b' entre checkout et le nom d'une branche innexistante. La commande va d'abord créer une branche puis se placer dessus.

### 1.6.5. Fusionner deux branches

Depuis la branche qui va accueillir la fusion (souvent la branche mère):

`git merge nom-de-la-seconde-branche`

*Note*: Faire un merge de la branche branche1 ne supprimera pas cette dernière.

#### 1.6.5.1. Resoudre un conflit

Si deux branches qui vont être fusionnées comportent des modifications qui ne peuvent pas coexister (ex: la première branche modifie une ligne et la seconde supprime cette même ligne), alors un conflit est créé.

Un conflit prend la forme suivante (le premier '.' n'est pas présent normalement):

```plaintext
.<<<<<<< HEAD
Ligne avec sa modification
=======
>>>>>>> branche-a-fusionner
```

Les '====' représentent le milieu du conflit, les '<<<<' représentent le début et les '>>>>' la fin.

Entre le bloc <<<< et ==== se trouve l'état de la zone de conflit pour la branche courante (celle sur laquelle on se trouve et qui va accueillir la fusion).

Entre le bloc ==== et >>>> on trouve la zone de conflit du point de vue de la branche qui va être fusionnée.

Pour résoudre le conflit, il faut conserver, à la main, la ou les partie(s) qui nous intéresse. On peut donc:

- Conserver uniquement la première partie
- Conserver uniquement la seconde partie
- Faire un mélange des deux au besoin.
  Par exemple, on peut conserver le premier paragraphe de la seconde partie et le second paragraphe de la première et échanger leur ordre.

Pour résoudre un conflit, il est essentiel de supprimer les lignes contenant les <<<< >>>> et les ====.

Une fois que les modifications sont terminé, on ajoute le fichier conserné à l'index et on l'enregistre via un commit.

## 1.7. Liens utiles

1. (EN) Le site officiel de git, la base en somme: [http://www.git-scm.org/docs](http://www.git-scm.org/docs)
2. (EN) Les articles/tutos de git de Atlassian: [https://www.atlassian.com/git/tutorials](https://www.atlassian.com/git/tutorials)
3. (FR) La vidéo de la capsule, idéal pour avoir/réviser les bases rapidement: [https://www.youtube.com/watch?v=hPfgekYUKgk&t=1209s](https://www.youtube.com/watch?v=hPfgekYUKgk&t=1209s)
4. (FR) La playlist des cours sur git de Grafikart, pour avoir les bases comme des notions avancées: [https://www.youtube.com/playlist?list=PLjwdMgw5TTLXuY5i7RW0QqGdW0NZntqiP](https://www.youtube.com/playlist?list=PLjwdMgw5TTLXuY5i7RW0QqGdW0NZntqiP)
