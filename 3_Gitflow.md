# 1. Gitflow

## 1.1. Sommaire general

- [1. Git les bases](.)
- [2. Git avancé](./2_Git_avance.md)
- [3. Gitflow](./3_Gitflow.md)

## 1.2. Sommaire

- [1. Gitflow](#1-gitflow)
  - [1.1. Sommaire general](#11-sommaire-general)
  - [1.2. Sommaire](#12-sommaire)
  - [1.3. Introduction](#13-introduction)
    - [1.3.1. Problème](#131-problème)
    - [1.3.2. Principe](#132-principe)
  - [1.4. Les types de branches](#14-les-types-de-branches)
    - [1.4.1. Le tronc (trunk)](#141-le-tronc-trunk)
    - [1.4.2. La branche principale de developpement](#142-la-branche-principale-de-developpement)
    - [1.4.3. Branches de fonctionnalités (features branches)](#143-branches-de-fonctionnalités-features-branches)
    - [1.4.4. Branche de version (release branches)](#144-branche-de-version-release-branches)
    - [1.4.5. Branche de hot-fix](#145-branche-de-hot-fix)
  - [1.5. Regles a suivre](#15-regles-a-suivre)
  - [1.6. Schema de la vie d'un projet suivant le GitFlow](#16-schema-de-la-vie-dun-projet-suivant-le-gitflow)
  - [1.7. GitFlow simplifie](#17-gitflow-simplifie)
  - [1.8. Source](#18-source)

## 1.3. Introduction

### 1.3.1. Problème

- A force de créer des branches, le repo peut devenir fouilli
- Dans un contexte de CD => Nécéssité d'avoir une branche stable pour les déploiments
- Sans organisation, de nombreux conflits peuvent survenir

### 1.3.2. Principe

Structuration du repository via des "types" de branches qui ont des intéractions définies entre eux.

---

*Note* : Dans les parties suivantes, le gitflow présenté sera la version la plus complète existente. Une dernière partie sur des idées d'adaptations pour un système simplifié.

---

Le GitFlow s'utilise très bien dans les contextes de gestion agile (scrum, kanban, ...) mais peut également être utilisé dans les méthodes de gestion de projet "traditionnelles" (cycle en cascade, ...).

## 1.4. Les types de branches

Les types de branches seront décrits de la manière suivante:

1. Le type de branche en titre
2. Le nommage conventionnellement juste après en gras (si il existe)
3. Une description du type de branche

### 1.4.1. Le tronc (trunk)

**main** (anciennement **master**)

Contient l'historique des versions déployées.  
HEAD est la dernière version disponible (majeure.mineure.bugfix).  
Les autres versions sont dipsonible via des tags.

### 1.4.2. La branche principale de developpement

**develop**.

La branche sur laquelle l'équipe de développement synchronise sont travail pour la prochaine version du projet.

### 1.4.3. Branches de fonctionnalités (features branches)

Pas de nommage définit, la convention dépendra des équipes/entreprises.

Branche de travail pour l'ajout d'une fonctionnalité, correction d'un bug, refactor...

### 1.4.4. Branche de version (release branches)

Peut porter le numéro de la version à produire.

Branche temporaire, après le développement d'une nouvelle version. Elle permet une dernière phase de tests et la préparation des livrables avant la production. Elle peut également accueillir des commits de correction de bugs.

### 1.4.5. Branche de hot-fix

Pas de nommage définit, la convention dépendra des équipes/entreprises.

Similaire aux branches de fonctionnalités, ces branches permettent la correction de bugs directement en production (quand possible).

**Attention**: Les bugs corrigés ici doivent être des bugs majeur impactant grandement l'utilisabilité du projet. Ces branches **doivent** rester exceptionnelles.

## 1.5. Regles a suivre

- Aucun commit/push sur les branches main et develop.
- Si il y a une CI:
  - elle doit toujours être au vert pour les branches main, develop et les branches de version.
  - les merges d'une branche vers une autre ne doivent se faire que si la CI est au vert.
- La mise en place d'une CD:
  - Peut se faire sur la branche main vers l'environnement de production
  - Peut se faire sur les branches de version vers un environnement de pré-production.
  - Peut se faire sur la branche develop vers un environnement de développement.
- La branche develop est créée depuis la branche main
- Les branches de version sont créées depuis la branche develop
- Les branches de fonctionnalités sont créées depuis la branche develop
- Les branches de hot-fix sont créées depuis la branche main
- Les branches de fonctionnalités sont rappatriées sur la branche develop
- Les branches de version sont rappatriées sur la branche main. Elle le sera également sur la branche develop si il y a eu des commits sur la branche.
- Les branches de hot-fix sont rappatriées sur la branche main et sur la branche develop.
- Règle de bonne pratique: S'assure que notre branche est basé sur la HEAD de la branche mère avant de la rappatrier. En cas de conflits au moment du changement de référenciel, cela permet de gérer les conflits sur sa branche et non sur la branche mère (qui est souvent la develop).
- Règle de bonne pratique: Intégrer un processus de relecture et de test avant tout merge (en particulier ceux de hot-fix).

## 1.6. Schema de la vie d'un projet suivant le GitFlow

```plaintext
tags                                   v1.0.0    v1.0.1/HEAD
main     >--O--------------------------O---------O
             \                        / \       /
hot-fix       \                      /   O--O--O
               \                    /           \
v1.0.0          \               O--O             \
                 \             /    \             \
develop           O-----------O------O-------------O
                   \         /
feature-1           O---O---O
```

## 1.7. GitFlow simplifie

Le GitFlow est un système, certes très robuste, mais également assez lourd à mettre en place. il est donc pertinent de l'utiliser dans sa forme complète dans les gros projets ou dans les projets demandant un haut niveau de qualité et de fiabilité dans ses livrables. Il est également très utile pour cadrer une grande équipe afin de chacun ne déborde pas sur les parties des autres (conflits je vous regarde).  
Pour les projets de plus faible envergure (prototypage, side-project de test/entraînement, ...) et unipersonnel ou dans une petite équipe, le GitFlow peut être simplifié avec de rendre son implémentation plus accessible.

Il est possible par exemple de supprimer les branches de version. La phase de test et les livrables qu'elles incluent peut être faites au sein de la branche develop (pas directement mais via une branche de fonctionnalité).  
La suppression de ces branches peut simplifier le déploiement d'une nouvelle version.

Dans le cadre d'un projet qui n'est pas déployé en production (entraînement, ...), il est également possible de supprimer les branches de hot-fix et de faire la résolution de ces bugs via des branches de fonctionnalités et en produisant une version plus rapidement pour corriger le bug plus tôt.

Avec ces deux propositions, le nombre de type de branches n'est plus que de 3. Les branches avec plusieurs merge à faire (donc plus lourde à utiliser) ne sont plus présentes.

```plaintext
tags                       v1.0.0           v1.0.1/HEAD
main     >--O--------------O----------------O
             \            /                /
develop       O----------O----------------O
              \         / \              /
feature-1      O---O---O   \            /
                            \          /
feature-2 (fix de bug)       O--O--O--O
```

## 1.8. Source

- <https://www.atlassian.com/fr/git/tutorials/comparing-workflows/gitflow-workflow>
