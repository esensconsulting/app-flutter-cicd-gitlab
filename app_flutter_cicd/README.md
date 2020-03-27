# Exemple d'application FLUTTER (Avec deploiement)

"App deploy example" est une application présentant le fonctionnement d'une CI/CD avec une application flutter.

Les différentes étapes de cette implementation sont :

Pour une CI/CD seuelement pour Android :

1. Lancement des tests unitaires
2. Build l'app
    - APK pour android
3. Deploiement APK
    - Dans App distribution pour toutes les MR

Pour une CI/CD pour Android et IOS :

1. Lancement des tests unitaires
2. Build l'app
    - APK pour android
    - IPA pour IOS
3. Deploiement APK/IPA
    - Dans App distribution pour toutes les MR

## Pré-requis

- [Installer sur votre pc flutter](https://flutter.dev/docs/get-started/install)
- [Projet Gitlab](https://gitlab.com/)
- [Firebase CLI](https://firebase.google.com/docs/cli#install-cli-windows) et de se connecter en local

## Choix du gitlab-ci.yml

Vous désirez une CI/CD pour une application android ?

--> Récupérer le contenu du fichier .gitlab-ci_android.yml dans votre fichier .gitlab-ci.yml

Vous désirez une CI/CD pour une application android et IOS ?

--> Récupérer le contenu du fichier .gitlab-ci_android_and_ios.yml dans votre fichier .gitlab-ci.yml

## Android

### Initialiser Firebase

Dans un premier temps il faut se connecter à [firebase](https://console.firebase.google.com) et de créer un projet.

Il est néccesaire d'initialiser une application Android, pour cela :
- Aller dans App Distribution
- Cliquer sur l'incone Android
    - Nom du package : com.enterprise.example_app
    - Nom de mon application : Example App
    - Cliquer sur Enregistrer l'application
    - Puis Suivant
    - Suivant
    - Acceder à la console

Sur la console App Distribution cliquer sur "Commencer", vous voici dans l'App distribtuion de votre applciation Android

#### Création d'un groupe de testeur dans firebase

Dans l'app distribution aller dans l'onglet "Testeurs et groupes", puis :
- Cliquer sur "Ajouter un groupe"
- Saisir le nom de groupe : qa-team
- Puis ajouter autant d'adresse mail que nécessaire

#### Initialiaser firebase dans le projet

Créer le projet dans firebase
A la racine du projet executer cette commande :

```bash
$ firebase init
```

- Remplir par "y" pour "You are initializing in an existing Firebase project directory"
- Choisir "Hosting"
- Choisir "Use an existing project"
- Choisir votre application
- Appuyer sur entrer pour "public"
- Remplir par "N" pour "Configure as a single-page app (rewrite all urls to /index.html)?"
- Supprimer le dossier "public"

#### Générer une clé api pour GitLab

Sur votre poste executer cette commande :

```bash
$ firebase projects:list
```

Récupérer l'id de votre PROJET puis:

```bash
$ firebase use id_de_votre_projet
$ firebase login:ci
```

Connectez-vous, puis, dans le terminal vous devez récupérer la clé générer. Elle commence par 1/*****.

Coller cette clé dans une variable d'environnement de GitLab sur votre Projet:

* Nom de la variable : FIREBASE_TOKEN_CLI
* Value de la variable : 1/*****


#### Ajouter le projet ID Firebase de l'application Android dans le gitlab-ci

Aller dans la console firebase puis dans l'onglet "Vue d'ensemble du projet":
- Cliquer sur votre application Android (L'icone Android)
- Puis aller dans les paramètres récupérer l'ID de l'application commencant par 1:****
- Créer une variable dans Gitlab en remplissant comme suit :
    - key = APP_ID_FIREBASE_ANDROID
    - value = 1:****

## Ios

Seulement si vous avez choisi le fichier gitlab-ci_android_and_ios

### Pré-requis supplémentaire pour IOS

- Un Mac ou un serveur Mac (ex : [Mac stadium](https://www.macstadium.com/))
- Une [licence apple developper](https://developer.apple.com/programs/) (pour la compilation)
- [Créer un runner Gitlab CI sur votre mac](https://docs.gitlab.com/runner/install/osx.html#manual-installation-official) ATTENTION : Pour l'étape 'Register the runner' sélectionner l'executor ssh
- [Installer sur votre mac flutter](https://flutter.dev/docs/get-started/install/macos)
- [Installer sur votre mac firebase](https://firebase.google.com/docs/cli#install-cli-mac-linux)

### Configuration runner gitlab

 Associer votre Runner Mac sur votre projet Gitlab, pour cela aller sur votre projet Gitlab et suiver ces instructions :
  - Cliquer sur "Settings" de votre projet Gitlab
  - Onglet "CI / CD"
  - Déplier "Runners"
  - Sélectionner votre runner en cliquant sur "Enable for this project"
  - Cliquer sur "Disable shared Runners" pour désactiver tous les autres runners

### Créer une application IOS dans firebase

Dans un premier temps il faut se connecter à [firebase](https://console.firebase.google.com)

Il est néccesaire d'initialiser une application IOS, pour cela :
- Aller dans "Vue d'ensemble du projet"
- Cliquer sur "+ Ajouter une application"
- Cliquer sur l'icon IOS
    - Nom du package : com.enterprise.example_app
    - Nom de mon application : Example App
    - Cliquer sur Enregistrer l'application
    - Puis Suivant
    - Suivant
    - Acceder à la console
- Cliquer sur "App Distribution"
- Choisir l'application IOS
- Cliquer sur commencer

Vous voici dans l'App distribtuion de votre applciation IOS

#### Ajouter le projet ID Firebase de l'application IOS dans le gitlab-ci

Aller dans la console firebase puis dans l'onglet "Vue d'ensemble du projet":
- Cliquer sur votre application Android (L'icone Android)
- Puis aller dans les paramètres récupérer l'ID de l'application commencant par 1:****
- Créer une variable dans Gitlab en remplissant comme suit :
    - key = APP_ID_FIREBASE_IOS
    - value = 1:****