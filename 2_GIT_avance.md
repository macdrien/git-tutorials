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
    - [Generer une cle SSH](#generer-une-cle-ssh)
      - [Windows](#windows)
  - [1.5. Utiliser rebase avant de faire un merge](#15-utiliser-rebase-avant-de-faire-un-merge)
  - [1.6. Le rebase interractif](#16-le-rebase-interractif)
  - [1.7. Les adds partiels](#17-les-adds-partiels)
  - [1.8. Le nommage](#18-le-nommage)
    - [1.8.1. Nommer un commit](#181-nommer-un-commit)
    - [1.8.2. Nommer une branche](#182-nommer-une-branche)

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

### Generer une cle SSH

*Note: Pour Linux, vous pouvez suivre le même tutoriel en ouvrant un terminal et en vous rendant dans votre `/home/<Nom d'utilisateur>` à la place de `/<Lettre de disque>/Users/<Nom d'utilisateur`. Les commandes restent les mêmes.*

#### Windows

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

## 1.6. Le rebase interractif

## 1.7. Les adds partiels

## 1.8. Le nommage

### 1.8.1. Nommer un commit

### 1.8.2. Nommer une branche
