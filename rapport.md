---
layout: default
title: Rapport du projet fil rouge
---

# Rapport du projet fil rouge

> **Note sur les sources :** ce rapport s'appuie sur le code source du dépôt et sur la spécification du Sprint 1. Il décrit la plateforme **SmartMatch** telle qu'elle est implémentée.

## Table des matières

- [Remerciements](#remerciements)
- [Introduction](#introduction)
- [Contexte du projet](#contexte-du-projet)
- [Objectif de formation (Solicode)](#objectif-de-formation-solicode)
- [Cahier des charges](#cahier-des-charges)
- [Méthodologie de travail](#méthodologie-de-travail)
  - [Méthodologie Scrum](#méthodologie-scrum)
  - [Processus 2TUP](#processus-2tup)
  - [Design Thinking](#design-thinking)
- [Planification](#planification)
- [Branche fonctionnelle](#branche-fonctionnelle)
  - [Carte d'empathie](#carte-dempathie)
  - [Définition du problème](#définition-du-problème)
  - [Idéation](#idéation)
  - [Diagramme de cas d'utilisation](#diagramme-de-cas-dutilisation)
- [Branche technique](#branche-technique)
  - [Stack technologique](#stack-technologique)
  - [Architecture logicielle](#architecture-logicielle)
  - [Prototype : fonctionnalités et classes](#prototype--fonctionnalités-et-classes)
- [Conception](#conception)
  - [Diagramme de classes](#diagramme-de-classes)
  - [Maquettes](#maquettes)
- [Réalisation](#réalisation)
  - [Interfaces](#interfaces)
- [Conclusion](#conclusion)

## Remerciements

## Introduction

## Contexte du projet

## Objectif de formation (Solicode)

## Cahier des charges

## Méthodologie de travail

### Méthodologie Scrum

Scrum est une méthode agile qui organise le travail en cycles courts et favorise la livraison progressive de fonctionnalités. Elle s'appuie sur trois principes :

- **Transparence** : rendre visibles les objectifs, les tâches et leur avancement.
- **Inspection** : examiner régulièrement le travail réalisé et les éventuels blocages.
- **Adaptation** : ajuster les priorités et le plan de travail à partir des résultats et des retours.

Le projet SmartMatch est organisé en sprints successifs. Chaque cycle permet de livrer un ensemble fonctionnel : la page publique et l'espace d'administration ont ainsi été développés puis enrichis au fil des itérations.

![Schéma de la méthode Scrum](SCRUM/scrum.png)

*Figure 1 — Schéma de la méthode Scrum.*

#### Sprint 1

#### Sprint 2

### Processus 2TUP

2TUP (*Two Tracks Unified Process*) est une démarche itérative organisée autour de deux axes menés en parallèle :

- **Branche fonctionnelle** : compréhension des utilisateurs, des processus et des besoins métier.
- **Branche technique** : étude des contraintes, de l'architecture et des choix technologiques.

Les deux branches convergent vers la réalisation, qui peut comprendre la conception, le développement, les tests et le déploiement. Le projet SmartMatch sépare effectivement les besoins fonctionnels — gestion des offres, des compétences et des utilisateurs — des choix techniques — API, stockage des données et authentification — avant leur intégration dans l'application.

![Schéma du processus 2TUP](2TUP/images.jpg)

*Figure 2 — Schéma général du processus 2TUP.*

### Design Thinking

Le Design Thinking est une démarche de résolution de problèmes centrée sur les besoins des utilisateurs. Elle encourage la collaboration et l'exploration de solutions avant leur mise en œuvre.

Ses principales étapes sont :

1. **Empathie** : observer et interroger les utilisateurs pour comprendre leurs besoins et leurs difficultés.
2. **Définition** : synthétiser les informations recueillies et formuler le problème à résoudre.
3. **Idéation** : proposer plusieurs solutions possibles, sans écarter prématurément les idées.
4. **Prototype** : matérialiser une solution sous une forme simplifiée.
5. **Test** : recueillir les retours des utilisateurs et améliorer la solution.

![Étapes du Design Thinking](design_thinking/design_thinking.jpg)

*Figure 3 — Étapes du Design Thinking.*

## Planification

## Branche fonctionnelle

### Carte d'empathie

### Définition du problème

### Idéation

### Diagramme de cas d'utilisation

## Branche technique

### Stack technologique

Le dépôt permet d'identifier les choix techniques suivants pour la plateforme de recrutement SmartMatch :

- **Backend** : PHP 8.1 ou supérieur, en programmation orientée objet, sans framework.
- **API** : point d'entrée unique `backend/api.php`, réponses au format JSON et méthodes HTTP GET, POST, PUT et DELETE.
- **Authentification** : sessions PHP, mots de passe stockés sous forme de hachage bcrypt et vérifiés avec `password_verify` ; accès administrateur contrôlé par le rôle de l'utilisateur.
- **Stockage des données** : fichiers JSON (`users.json`, `offres.json`, `competences.json`) lus et écrits par des DAO, avec verrouillage des fichiers (`flock`).
- **Frontend** : HTML5, Tailwind CSS chargé via CDN, JavaScript natif et Fetch API, sans framework ni étape de compilation.
- **Upload d'images** : validation du type MIME réel (`getimagesize` et `finfo`), formats JPG, JPEG, PNG et WebP, taille maximale de 2 Mo, stockage dans `backend/data/uploads/offres/`.
- **Versionnement** : Git.

À noter : la spécification du Sprint 1 prévoyait MySQL avec PDO et du CSS natif ; le code actuel a été refactorisé en programmation orientée objet avec un stockage JSON et Tailwind CDN. La description ci-dessus correspond à l'état du dépôt.

### Architecture logicielle

L'application est organisée en trois couches :

1. **Présentation** (`frontend/`) : pages HTML (`index.html`, `admin/login.html`, `admin/dashboard.html`, `admin/offres.html`, `admin/competences.html`), mise en forme avec Tailwind CSS et scripts JavaScript. Le fichier `api.js` centralise les appels HTTP ; `admin-auth.js` vérifie la session ; `main.js`, `admin-offres.js` et `admin-competences.js` animent les pages.
2. **Application** (`backend/controller` et `backend/classes/services`) : `api.php` route la requête selon le paramètre `resource` ; les contrôleurs exposent les actions HTTP ; les services appliquent les règles métier et les validations.
3. **Données** (`backend/classes/dao` et `backend/data`) : les DAO héritent de `JsonStorage`, qui assure la lecture, l'écriture, le verrouillage et l'auto-incrémentation des identifiants. Les images sont gérées par `ImageUpload`.

Les entités `User`, `Offre` et `Competence` représentent les objets métier ; `Request` et `Response` normalisent l'entrée et la sortie JSON. Le frontend et le backend sont séparés : toutes les pages passent par l'API, sans accès direct aux fichiers de données.

Exemple de flux : la page publique appelle `GET api.php?resource=offres` ; `api.php` transmet la requête à `OffreController`, puis à `OffreService` et `OffreDAO` ; les données de `offres.json` sont renvoyées en JSON et affichées sous forme de cartes par `main.js`.

Cette organisation reprend les principes MVC et trois-tiers : présentation, logique applicative et accès aux données sont séparés.

### Prototype : fonctionnalités et classes

Le prototype implémenté est une plateforme de recrutement nommée **SmartMatch**. Il couvre la page publique et l'espace d'administration.

Fonctionnalités réalisées :

- page publique responsive avec section d'accueil, présentation du fonctionnement et chargement dynamique des quatre dernières offres ;
- affichage des offres avec image (ou image de remplacement), titre, domaine, expérience requise et date de publication ;
- connexion et déconnexion de l'administrateur, avec session PHP et contrôle du rôle ;
- tableau de bord avec compteurs (offres, compétences, utilisateurs) et listes des dernières offres et compétences ;
- CRUD complet des offres : création, consultation, modification et suppression, avec upload et remplacement d'image ;
- CRUD complet des compétences : création, consultation, modification et suppression, avec unicité du nom ;
- consultation en lecture seule des utilisateurs via l'API pour les statistiques.

Fonctionnalités annoncées mais non encore implémentées :

- inscription et connexion des candidats ;
- candidature à une offre ;
- calcul du score de matching ;
- association des compétences aux candidats et aux offres ;
- tableau de bord réservé aux recruteurs.

Classes et relations :

- `User` : identifiant, nom, prénom, email, mot de passe, rôle (`admin`, `recruteur`, `candidat`), domaine et années d'expérience.
- `Offre` : identifiant, titre, description, domaine, années d'expérience requises, image, identifiant du recruteur et date de création.
- `Competence` : identifiant et nom.
- `UserDAO` gère les utilisateurs (`findByEmail`, `verifyPassword`, `publicView`) ; `OffreDAO` et `CompetenceDAO` héritent de `JsonStorage`.
- `AuthService`, `OffreService` et `CompetenceService` portent les règles de validation et de gestion.
- `AuthController`, `OffreController` et `CompetenceController` exposent les opérations de l'API.
- `Request`, `Response` et `ImageUpload` sont des helpers techniques.

Relations :

- une `Offre` est rattachée à un `User` ayant le rôle `recruteur` ou `admin`, via `id_recruteur` ;
- un `User` peut être administrateur, recruteur ou candidat ;
- les compétences constituent un catalogue indépendant ;
- les tables de liaison `candidat_competence` et `offre_competence`, ainsi que la table `candidatures`, étaient prévues dans le schéma du Sprint 1 mais ne sont pas encore exploitées dans le stockage JSON actuel.

## Conception

### Diagramme de classes

### Maquettes

## Réalisation

### Interfaces

## Conclusion
