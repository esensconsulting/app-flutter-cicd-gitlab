# Exemple d'une CI/CD Flutter dans GitLab

'app flutter cicd gitlab' est une application présentant le fonctionnement d'une CI/CD avec une application Flutter.

Les différentes étapes de cette implémentation sont :

Pour une CI/CD seulement pour Android :

1. Lancement des tests unitaires
2. Build l'app
    - APK pour Android
3. Déploiement APK
    - Dans App distribution pour toutes les MR

Pour une CI/CD pour Android et iOS :

1. Lancement des tests unitaires
2. Build l'app
    - APK pour Android
    - IPA pour iOS
3. Déploiement APK/IPA
    - Dans App distribution pour toutes les MR

## Pré-requis

- [Installer Flutter sur votre pc](https://flutter.dev/docs/get-started/install)
- [Projet GitLab](https://gitlab.com/)
- [Firebase CLI](https://firebase.google.com/docs/cli#install-cli-windows) et se connecter en local

## Choix du gitlab-ci.yml

Vous désirez une CI/CD pour une application Android ?

--> Récupérer le contenu du fichier .gitlab-ci_android.yml dans votre fichier .gitlab-ci.yml

Vous désirez une CI/CD pour une application Android et iOS ?

--> Récupérer le contenu du fichier .gitlab-ci_android_and_ios.yml dans votre fichier .gitlab-ci.yml

## Android

### Initialiser Firebase

Dans un premier temps il faut se connecter à [Firebase](https://console.firebase.google.com) et créer un projet.

Il est nécessaire d'initialiser une application Android, pour cela :
- Aller dans App Distribution
- Cliquer sur l'icône Android
    - Nom du package : com.enterprise.example_app
    - Nom de mon application : Example App
    - Cliquer sur 'Enregistrer l'application'
    - Puis 'Suivant'
    - 'Suivant'
    - 'Accéder à la console'

Sur la console App Distribution, cliquer sur 'Commencer', vous voici dans l'App distribution de votre application Android

#### Création d'un groupe de testeur dans Firebase

Dans l'app distribution, aller dans l'onglet 'Testeurs et groupes', puis :
- Cliquer sur "Ajouter un groupe"
- Saisir le nom de groupe : qa-team
- Puis ajouter autant d'adresses mail que nécessaire

#### Initialiser firebase dans le projet

Créez le projet dans Firebase. A la racine du projet exécutez cette commande :

```bash
$ firebase init
```

- Remplir par 'y' pour "You are initializing in an existing Firebase project directory"
- Choisir 'Hosting'
- Choisir 'Use an existing project'
- Choisir votre application
- Appuyer sur 'entrée' pour 'public'
- Remplir par 'N' pour 'Configure as a single-page app (rewrite all urls to /index.html)?'
- Supprimer le dossier 'public'

#### Générer une clé api pour GitLab

Sur votre poste, exécuter cette commande :

```bash
$ firebase projects:list
```

Récupérer l'id de votre PROJET puis:

```bash
$ firebase use id_de_votre_projet
$ firebase login:ci
```

Connectez-vous, puis, dans le terminal vous devez récupérer la clé générée. Elle commence par 1/*****.

Coller cette clé dans une variable d'environnement de GitLab sur votre Projet:

* Nom de la variable : FIREBASE_TOKEN_CLI
* Value de la variable : 1/*****


#### Ajouter le projet ID Firebase de l'application Android dans le gitlab-ci

Aller dans la console Firebase puis dans l'onglet 'Vue d'ensemble du projet':
- Cliquer sur votre application Android (L'icône Android)
- Puis aller dans les paramètres récupérer l'ID de l'application commençant par 1:****
- Créer une variable dans GitLab en remplissant comme suit :
    - key = APP_ID_FIREBASE_ANDROID
    - value = 1:****

## iOS

Seulement si vous avez choisi le fichier gitlab-ci_android_and_ios

### Pré-requis supplémentaire pour iOS

- Un Mac ou un serveur Mac (ex : [Mac stadium](https://www.macstadium.com/))
- Une [licence Apple developper](https://developer.apple.com/programs/) (pour la compilation)
- [Créer un runner GitLab CI sur votre mac](https://docs.gitlab.com/runner/install/osx.html#manual-installation-official) ATTENTION : Pour l'étape 'Register the runner', sélectionner l'executor ssh
- [Installer sur votre mac Flutter](https://flutter.dev/docs/get-started/install/macos)
- [Installer sur votre mac Firebase](https://firebase.google.com/docs/cli#install-cli-mac-linux)

### Configuration runner GitLab

 Associez votre Runner Mac sur votre projet GitLab. Pour cela, allez sur votre projet GitLab et suivez ces instructions :
  - Cliquer sur 'Settings' de votre projet Gitlab
  - Onglet 'CI / CD'
  - Déplier 'Runners'
  - Sélectionner votre runner en cliquant sur 'Enable for this project'
  - Cliquer sur 'Disable shared Runners' pour désactiver tous les autres runners

### Créer une application iOS dans Firebase

Dans un premier temps, il faut se connecter à [Firebase](https://console.firebase.google.com)

Pour cela, il est nécessaire d'initialiser une application iOS :
- Aller dans 'Vue d'ensemble du projet'
- Cliquer sur '+ Ajouter une application'
- Cliquer sur l'icon IOS
    - Nom du package : com.enterprise.example_app
    - Nom de mon application : Example App
    - Cliquer sur Enregistrer l'application
    - Puis 'Suivant'
    - 'Suivant'
    - 'Accéder à la console'
- Cliquer sur 'App Distribution'
- Choisir l'application iOS
- Cliquer sur 'Commencer'

Vous voici dans l'App distribtuion de votre application IOS

#### Ajouter le projet ID Firebase de l'application iOS dans le gitlab-ci

Aller dans la console Firebase puis dans l'onglet 'Vue d'ensemble du projet':
- Cliquer sur votre application iOS (L'icône iOS)
- Puis aller dans les paramètres récupérer l'ID de l'application commençant par 1:****
- Créer une variable dans GitLab en remplissant comme suit :
    - key = APP_ID_FIREBASE_IOS
    - value = 1:****
