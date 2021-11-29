# 1. GIT avance

## 1.1. Sommaire general

- [1. GIT les bases](.)
- [2. GIT avancé](./2_GIT_avance.md)

## 1.2. Sommaire

- [1. GIT avance](#1-git-avance)
  - [1.1. Sommaire general](#11-sommaire-general)
  - [1.2. Sommaire](#12-sommaire)
  - [1.3. Introduction](#13-introduction)
  - [1.4. Configurer l'authentification SSH](#14-configurer-lauthentification-ssh)
    - [1.4.1. Generer une cle SSH](#141-generer-une-cle-ssh)
      - [1.4.1.1. Windows](#1411-windows)
  - [1.5. Utiliser rebase avant de faire un merge](#15-utiliser-rebase-avant-de-faire-un-merge)
  - [1.6. Le rebase interractif](#16-le-rebase-interractif)
  - [1.7. Ajouter des modifications au dernier commit](#17-ajouter-des-modifications-au-dernier-commit)
  - [1.8. Les adds partiels](#18-les-adds-partiels)
  - [1.9. Le nommage des commits](#19-le-nommage-des-commits)
    - [1.9.1. Nommer un commit](#191-nommer-un-commit)
  - [1.10. Mettre son travail de cote](#110-mettre-son-travail-de-cote)
    - [1.10.1. Mettre son travail de cote](#1101-mettre-son-travail-de-cote)
    - [1.10.2. Lister ses stashs](#1102-lister-ses-stashs)
    - [1.10.3. Recuperer le contenu d'un stash](#1103-recuperer-le-contenu-dun-stash)
    - [1.10.4. Supprimer un stash](#1104-supprimer-un-stash)

## 1.3. Introduction

Ceci est la deuxième partie du tutoriel sur GIT. Si vous ne connaissez pas GIT et que vous n'avez pas lu la première partie, je vous recommande de vous former aux bases de son utilisation en [cliquant ici](./README.md).

---

Bien, vous avez donc déjà une petite expérience avec GIT. Mais vous êtes acharné! Il vous en faut plus!  
Très bien, dans cette session, nous allons aborder quelques concepts clés de GIT pour une utilisation qui se rapproche de ce que j'utilise au quotidien dans mon travail et dans mes projets annexes.

On va commencer par se détourner un peu des commandes pour se concentrer sur un aspet de sécurité.

## 1.4. Configurer l'authentification SSH

Secure SHell est un protocole de communication entre deux machines qui permet bien des choses. SSH permet entre autres l'accès à distance de serveurs, ou de RaspberryPI qui tourne chez vous pour de la domotique ou pour un serveur privé. Dans le cadre de l'utilisation de GIT, SSH va nous permettre de nous connecter de manière sécurisée à notre repo distant, et, en plus de ça, d'avoir accès à différents repo sur différentes plateformes à la fois.  
C'est le grand problème que j'ai rapidement recontré quand j'ai voulu explorer les différentes plateformes d'hébergements de repo sur internet (GitHub, GitLab, ...). Pour passer de l'une à l'autre, il faut réinitialiser ses identifiants sur la première plateforme pour se connecter à la seconde. C'est très peu pratique, en particulier si vous changez régulièrement de plateforme.

La solution c'est SSH. Ce protocole va vous permettre, en générant une paire de clés et en plaçant votre clé publique sur la plateforme, de pouvoir vous connecter à plusieurs plateforme dynamiquement via cette clé.

### 1.4.1. Generer une cle SSH

*Note: Pour Linux, vous pouvez suivre le même tutoriel en ouvrant un terminal et en vous rendant dans votre `/home/<Nom d'utilisateur>` à la place de `/<Lettre de disque>/Users/<Nom d'utilisateur`. Les commandes restent les mêmes.*

#### 1.4.1.1. Windows

Ouvrez un terminal Git Bash (installation via l'exe de d'installation de GIT, normalement installé si vous n'avez pas touché aux options lors de l'installation de GIT).  
Votre dossier courant (indiqué en jaune dans l'en-tête de la ligne) doit être `~`. Il correspond à votre dossier utilisateur de Windows.

Afin d'être sûr que vous soyez dans le bon dossier, entrez la commande `pwd`.  
Le résultat doit être quelque chose de l'ordre `/<Lettre de votre disque dur>/Users/macdrien`.  
Si ce n'est pas le cas, entrez la commande suivante `cd /<Lettre de votre disque dur principal>/Users/<Votre nom d'utilisateur windows>`.

---

*Note: Si toutes ces commandes sont de la magie noire pour vous. Pas de panique! Il existe milles tutoriels qui vous expliquent les bases des commandes linux.*

---

Listez vos fichiers courants avec les dossiers/fichiers cachés avec la commande `ls -la`.  
On recherche dans la liste obtenue le dossier `.ssh`. Pour le trouver plus rapidement, vous pouvez ajouter `| grep .ssh` à la fin de la ligne de commande (ce qui donne `ls -la | grep .ssh`).

Si le dossier n'existe pas encore, créez-en un avec la commande `mkdir .ssh`

Rendez-vous dans le dossier `.ssh` avec la commande `cd .ssh`.

On va pouvoir commencer par générer une nouvelle clé SSH en utilisant la commande suivante:

`ssh-keygen -t rsa -b 4096 -C "votre_adresse@mail.com"`

Cette commande va vous demander plusieurs choses à la suite pour générer la clé qui vous convient:

1. Le nom de la clé. Vous pouvez le laisser par défaut si vous voulez, ou lui donner un nom (que vous pouvez saisir de suite).  
   Si vous laissez le nom par défaut, la clé s'appellera 'id_rsa'.  
   **Important**: Ne mettez pas d'espaces dans le nom de votre clé. Utilisez plutôt le '_' ou le '-'.
2. Une passphrase. Autrement dit, un mot de passe complexe qui rendra votre clé plus sécurisé car cette passphrase vous sera demandée pour utiliser la clé.
   Vous pouvez la laisser vide si vous voulez. Dans ce cas votre clé n'aura pas de passphrase.
3. Confirmation de la passphrase. Si vous n'en avez pas saisie, appuyez à nouveau sur entrée.

Vous allez obtenir dans votre dossier (toujours `ls -la` pour lister les fichiers de votre dossier) les fichiers `<Nom de votre cle>` et `<Nom de votre cle>.pub` (id_rsa et id_rsa.pub si vous avez laissé par défaut.).

A présent, il nous faut ajouter cette clé dans votre agent ssh qui tourne en fond. Pour ça, on va exécuter les commandes suivantes:

- `eval $(ssh-agent -s)`. Ca va permettre de s'assurer que l'agent est bien en fonctionnement. Si il est arrêté, alors la commande va le démarrer automatiquement.
- `ssh-add <Votre clé SSH>`. Cette commande ajoute votre clé à la liste des clés connues par votre agent.

On va prévoir un petit peu la suite à faire en copiant maintenant le contenu de votre clé publique dans votre presse-papier avec la commande `cat ~/.ssh/<Nom de votre cle>.pub | clip` (pour **linux**, la commande est `xclip -sel clip < ~/.ssh/<Nom de votre cle>.pub`)

Votre clé est maintenant présente dans votre agent. On va maintenant la lier à votre plateforme d'hébergement de repo. Pour cela, connectez vous à votre plateforme favorite, allez dans vos paramètres (ou préférences pour GitLab), puis dans la section Clé SSH dans le menu latéral.  
Dans le champ "Key"/"Clé", collez la clé publique que vous avez copié précédemment.  
Puis appuyez sur "Add Key"/"Ajouter la clé".

Normalement, votre clé est maintenant liée à votre compte. On va tester ça avec la commande suivante: `ssh -T git@github.com` si vous utiliser github ou `ssh -T git@gitlab.com` (si vous êtes sur une autre plateforme, suivez le principe de ces deux commandes en adaptant l'URL).  
Votre terminal va vous demander une validation pour votre clé SSH, saisissez "yes"/"oui"(si vous êtes en français) puis valider.  
Si la connexion réussie, vous obtiendrez un message du style "Welcome macdrien!".

Félicitaion! Vous avez configuré votre plateforme d'hébergement de repo pour utiliser le SSH. A partir de maintenant, vous pourrez cloner vos repo en utilisant le lien SSH plutôt que le lien HTTP. Git s'occupera du reste.  
Pour les repos que vous avez déjà cloné, je vous invite à modifier l'adresse de votre origin avec la commande: `git remote set-url origin <Adresse SSH de votre repo>`. Pour donner un exemple, faire ceci avec le repo git-tutorials sur lequel vous êtes donnerez ceci: `git remote set-url origin git@github.com:macdrien/git-tutorials.git`.

---

Vous pourriez vous arrêter là. Mais on va rajouter un petit quelque chose qui va vous permettre de gérer vos clés automatiquement en fonction de l'url que vous demandez.

Toujours dans votre dossier .ssh, créez et ouvrez un fichier appelé config (sans extension, c'est très important). Dans ce fichier, vous allez ajouter les 3 lignes suivantes pour chaque plateforme que vous voulez utiliser:

```plaintext
Host addresse.plateforme
        PreferredAuthentications publickey
        IdentityFile ~/.ssh/votre_cle_privee
```

Par exemple, si vous utilisez GitHub et GitLab, votre fichier sera:

```plaintext
Host github.com
        PreferredAuthentications publickey
        IdentityFile ~/.ssh/votre_cle_github_privée

Host gitlab.com
        PreferredAuthentications publickey
        IdentityFile ~/.ssh/votre_cle_gitlab_privée
```

Notez que vous pouvez utiliser la même clé pour toutes vos plateforme. Mais ce n'est pas une bonne idée. Si votre clé privée est compromise, alors le voleur aura accès à toutes vos plateforme. Alors que si vous faites une clé par plateforme vous limitez les dégâts.

## 1.5. Utiliser rebase avant de faire un merge

Dans la première partie du tuto, on a évoqué le merge d'une branche vers sa branche mère. Le merge est très efficace mais il a un défaut, il insère, dans la branche mère, les commits dans l'ordre croissant de date de création. Autrement dit, vos commits et les commits de l'autre branche vont se mélanger. Si en pratique cela ne change rien au projet, cela peut en revanche poser quelques problèmes de lecture de l'historique.

Pour un merge de la branche B sur la branche A, cela pourrait donner ceci:

Avant le merge:

```plaintext
Branche A -X----X-----X----X--
            \                 
Branche B    -O----O----O----O
```

Après le merge:

```plaintext
Branche A -X--O--X--O--X--O--X--O-
```

Afin d'avoir un arbre de commits plus propre et surtout plus clair à lire, on va utiliser la commande rebase. En utilisant la commande rebase sur la branche B avant de merge, l'origine de la branche B va se déplacer sur le dernier commit de la branche A. Cela va permettre de réorganiser le tout avant de merge:

Sur la branche B:

```bash
git rebase A
git checkout A
git merge B
```

En faisant cela, l'arbre des commits devient:

Avant le rebase:

```plaintext
Branche A -X----X-----X----X--
            \                 
Branche B    -O----O----O----O
```

Après le rebase:

```plaintext
Branche A -X----X-----X----X
                            \                 
Branche B                    -O----O----O----O
```

Après le merge:

```plaintext
Branche A -X----X-----X----X-O----O----O----O
```

Comme on peut le voir, rebase modifie le référenciel des commits. Par précaution, GIT ne nous laissera pas push cette modification. Pour envoyer un rebase, il faut ajouter l'arguement `--force` à `git push`. Ou mieux, `git push --force-with-lease` (j'aborderai cet argument plus tard).

Rebase est une commande très vaste et aussi très risquée. On va voir tout de suite un autre example d'utilisation.

## 1.6. Le rebase interractif

Pour passer en rebase interactif: `git rebase -i A` ou `git rebase --interactive A`.

*Note:* Il est possible de changer l'ordre des lignes afin de réordonner les commits.

La liste des commits est ordonnée du plus ancien (en haut) au plus récent (dernier de la liste):

- **pick** applique le commit sans modification.
- **reword** ouvrira le commit pour changer le title et/ou la description.
- **edit** met le rebase en pause à ce commit pour pouvoir y faire des modifications via des ammends.
- **squash** fusionne le commit avec le précédent (celui de la ligne du dessus). Ouvrira une interface avec les deux titres et description de commit pour choisir ce qui sera conservé. *Note*: Il est possible de squash plusieurs commits d'un coup dans un commit.
- **fixup** comme squash mais n'affiche pas l'interface d'édition du commit. Conserve uniquement le descriptif du commit non-squash. En utilisant l'arguement `-C`, le decsriptif conservé sera celui du commit avec `fixup -C`.
- **exec** suivi d'une commande exécutera la commande shell.
- **break** met ne pause le rebase à cet endroit. Demandera un `git rebase --continue` pour poursuivre.
- **drop** supprime le commit.
- **label**/**reset**/**merge** ne sont utile que dans un cas très spécifique. Je ne les détaillerai donc pas ici.

Après avoir fait les changements appropriés, vous pourrez sauvegarder et quitter l'éditeur, le rebase s'exécutera en suivant les instructions notées.

Tout comme le rebase classique, il faudra push avec un `git push --force-with-lease` pour que les changements soient publiés.

## 1.7. Ajouter des modifications au dernier commit

Il peut arriver qu'après un commit, vous vous rendiez compte que vous avez oublié une modification dans votre source.  
Pas de panique, ajoutez la modification à votre index et saisissez la commande `git commit --amend --no-edit`. Elle va stocker les modifications dans le dernier commit effectué et sans vous demander de ressaisir votre message.

De même, si vous avez fait une erreur dans le texte du commit, vous pouvez entrer `git commit --amend` pour ouvrir l'éditeur de commit.  
Vous pouvez également y ajouter le `-m` que vous connaissez déjà pour modifier le titre du commit (`git commit --amend -m "Titre du commit"`).

## 1.8. Les adds partiels

Vous pouvez n'avoir besoin d'ajouter à votre index qu'une partie d'un fichier. Pour faire cela, pour pouvez ajouter l'option `-p` pour `--patch` à votre `git add`. Vous aurez alors directement dans la console une présentation des changement avec la possibilité de choisir si vous voulez ajouter le changement ou non à votre index.

Différentes options sont possible:

- **y** pour ajouter le changement et passer au suivant.
- **n** pour marquer le changement comme n'étant pas à ajouter et passer au suivant.
- **q** pour quitter cette interface de sélection sans sélectionner le morceau (hunk) présenté ni les suivants.  
  Concervera les choix déjà effectués.
- **a** pour ajouter le morceau (hunk) présenté et tous les suivants.
- **d** pour ne pas ajouter le morceau (hunk) présenté et aucun des morceaux suivants.
- **g** pour indiquer un numéro de hunk et s'y rendre.
- **/** pour rechercher un hunk en utilisant une regex et s'y rendre.
- **j** pour laisser le hunk courant non-décidé et aller au prochain hunk non-décidé.
- **J** pour laisser le hunk courant non-décidé et aller au prochain hunk.
- **K** pour laisser le hunk courant non-décidé et aller au hunk précédent.
- **s** pour séparer le hunk courant en plusieurs hunks plus petits.
- **e** pour éditer le hunk présenté.
- **?** pour afficher un résumé des actions possibles.

*Note:* l'option `-p` est également disponible dans d'autres commandes comme stash push par exemple.

## 1.9. Le nommage des commits

### 1.9.1. Nommer un commit

Nommer un commit est important car le titre d'un commit est l'élément principal qui servira à vous ou à la personne reprennant le code à savoir ce que le commit fait.  
On oublie donc les "test", "qsd" et autre " " comme j'ai pu en voir dans un contexte professionnel.

A la place, je vous recommande d'adopter une convention de nommage simple et fixe, sinon à toute l'équipe, du moins à vous. C'est en servant d'exemple que vous pourrez inciter les autres à changer leurs pratiques.

Par exemple, si vous utilisez Jira, vous pouvez lier le commmit que vous faite à la story Jira que vous êtes en train de traiter en commençant votre commit par: "Story XP[S]:XXX". Le S n'étant pas toujours présent et XXX correspondant à la story sur laquelle vous travaillez.  
En faisant cela, en plus d'une suite de titre explicite, vous aider le futur relecteur. Ce dernier pourra, au besoin, aller voir la story en question pour voir son objectif.

J'ai personnellement adopté une convention de nommage personnelle que j'applique sur tous les projets sur lesquels je passe du moment que ma convention n'entre pas en conflit avec une convention existante. Elle est simple et lisible par n'importe qui sans avoir besoin de documentation. Il s'agit d'un préfixage de 3 lettres majuscules de chaque titre de commit. Cela sert à indiquer le type de commit dont il s'agit. Je complète ensuite le titre pour apporter une précision sur l'action.

- **ADD** indique que le commit ajoute quelque chose (fonctionnalité, nouvelle classe/méthode, ...).
- **DEL** indique, au contraire, que le commit ne fait que supprimer des éléments.
- **FIX** indique que le commit résoud un bug.
- **WIP** indique que le commit concerne un travail en cours et non-terminé.
- **REF** indique que le commit applique un réfactor du code (sans ajout ou suppression de fonctionnalité).
- **UPD** est en dernier. Il signifie Update et est un peu passe-partout. Il est utilisé lors de modification naturellement, mais également si j'ai un commit qui effectue plusieurs petites tâches qui devront toutes être décrite en description du commit.

Je ne vous présente pas mon système pour que vous le repreniez, mais plutôt pour que vous ayez un exemple de ce qui peut être possible afin que vous puissiez créer votre système ou adapter un système existant à vos besoins (qui peuvent être différents des autres développeurs).

## 1.10. Mettre son travail de cote

Il peut arriver, pendant votre travail, que vous ayez besoin de mettre de côté tout votre travail ou une partie de ce dernier le temps de faire autre chose. La première idée qui pourra vous venir maintenant est de faire un premier commit que vous complèterai avec un amend. Mais ce n'est pas une bonne chose (le amend est une solution de secours en cas d'erreur).

Pour éviter cela, Git dispose de la commande `git stash`. Stash est une zone annexe de votre repo dans laquelle vous pouvez stocker des modifications en attendant de les réutiliser plus tard.

Comme toutes les commandes, et même plus encore pour stash, plusieurs actions et options sont possibles. En voici les principales:

### 1.10.1. Mettre son travail de cote

Avec la commande `git stash push`, vous pouvez mettre votre travail de côté.

Vous pouvez compléter cette commande avec l'option `-m` suivi d'un titre (le fonctionnement et l'objectif sont les mêmes que l'option `-m` de git commit).

Si vous ne préciser aucun argument concernant vos fichiers, tous les modifications de fichiers déjà suivis par Git seront misent de côté.  
Pour les fichiers nouvellement créé, vous pouvez les inclures à votre stash en les ajoutant à votre index au préalable avec un `git add`.  
Si seulement certains fichiers doivent être mis de côté, vous pouvez finir votre commande par `--` suivi, entourés d'espaces, de tous les fichiers que vous voulez ajouter.  
Par exemple `git stash push -- index.js css/style.js`.

*Note:* Vous pourrez peut-être trouver sur internet des gens parler de la commande `git stash save`. Il s'agit de l'ancienne commande pour stash push. Bien que toujours présente dans les commandes exécutables, save est dépréciée et sera supprimée tôt ou tard.

### 1.10.2. Lister ses stashs

Vous pouvez faire plusieurs stash différents en faisant plusieurs push. Dans ce cas, ils seront listé et référencés par une liste dont le premier élément sera votre dernier stash effectué et le dernier élément sera votre stash le plus ancien.

Pour lister vos stash, vous pouvez utiliser la commande: `git stash list`

Le numéro que vous observez alors entre {} est l'index de chaque stash. Il vous sera utile si vous voulez récupérer un stash en particulier.

### 1.10.3. Recuperer le contenu d'un stash

Pour récupérer le contenu d'un stash, vous avez deux commandes de disponible:

`git stash apply` va placer dans votre repo local le dernier stash que vous avez fait. Mais une copie de ce stash va rester dans la pile de côté.

`git stash pop` va placer dans votre repo local le dernier stash que vous avez fait et va supprimer la copie de ce stash dans la pile.

Si vous ne voulez pas récupérer le dernier stash fait, vous pouvez préciser l'index du stash que vous voulez (cf. [1.10.2](#1102-lister-ses-stashs)) à la fin de votre commande.

### 1.10.4. Supprimer un stash

Après un apply ou pour faire du nettoyage, vous pouvez vider votre pile de stash. Pour cela, vous avez à nouveau 2 options:

- `git stash drop` qui va supprimer le dernier stash enregistré. Vous pouvez y appliquer un numéro d'index pour supprimer le stash voulu.
- `git stash clear` qui va supprimer toute votre pile.

---

Comme je l'ai mentionné plus haut, il ne s'agit ici que des commandes principales de Stash. Vous pourrez en trouver d'avantage dans la documentation ou en faisant des recherches si vous avez besoin.
