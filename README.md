# Market Watch – Dashboard de cotations en temps réel

## 🚀 Présentation

**Market Watch** est une application web de suivi de cotations en temps réel, développée avec **Angular 17** pour le frontend et **.NET 8** pour le backend.

Le projet simule un flux continu de prix de marché (actions, FX, crypto) diffusé en temps réel via **SignalR**, avec un système d'alertes par seuil déclenchées côté serveur et notifiées instantanément à l'utilisateur concerné.

Conçu à l'origine comme une application de messagerie instantanée, le projet a été entièrement repensé pour explorer une problématique plus proche du calcul et de la diffusion de données financières : comment maintenir un flux de valeurs cohérent, le pousser en temps réel à plusieurs clients, et déclencher des notifications ciblées sans recharger la page.

## 🛠️ Technologies utilisées

### Backend
- **.NET 8** avec **ASP.NET Core Web API**
- **SignalR** pour la diffusion temps réel des cotations
- **JWT** pour l'authentification sécurisée
- **BackgroundService** pour le moteur de simulation (random walk)

### Frontend
- **Angular 17** (standalone components)
- **SignalR client** pour la réception des mises à jour
- **RxJS** (`BehaviorSubject`) pour la gestion d'état réactive
- **SCSS** pour le design

### DevOps
- **Azure Pipelines** pour la CI/CD

## 🎯 Fonctionnalités principales

### 1. Simulation de cotations en temps réel
Un service en arrière-plan (`PriceSimulatorService`) génère des variations de prix par **random walk** sur plusieurs actifs (actions, FX, crypto) et les diffuse à tous les clients connectés via un hub SignalR, une fois par seconde.

### 2. Tableau de bord live
Le frontend affiche les cotations dans un tableau qui se met à jour en continu, avec un effet visuel (flash vert/rouge) indiquant les hausses et les baisses.

### 3. Alertes par seuil
Chaque utilisateur peut définir une alerte sur un actif donné (ex: "BTC > 65000"). Le serveur évalue la condition à chaque tick et notifie uniquement le client concerné lorsque le seuil est franchi — sans diffusion globale.

### 4. Authentification JWT
Connexion sécurisée, token stocké côté client, vérification d'expiration, protection des endpoints et du hub SignalR.

## 💻 Installation et lancement

### Backend
```bash
cd backend
dotnet restore
dotnet build
dotnet run
```

### Frontend
```bash
cd frontend
npm install
ng serve
```

Accéder à l'application : `http://localhost:4200`

## 🌐 Architecture temps réel

- Le hub SignalR (`PriceHub`) gère :
  - La diffusion des mises à jour de prix à tous les clients connectés
  - L'enregistrement des alertes par utilisateur (`SetAlert`)
  - L'envoi de notifications ciblées uniquement au client concerné

- Le frontend utilise un service singleton (`PriceService`) pour :
  - Se connecter au hub avec le token JWT
  - Écouter les mises à jour de prix et les alertes déclenchées
  - Exposer l'état via des `Observable` consommés par les composants

## 📈 Pistes d'évolution

- Historique des cotations et graphique d'évolution
- Plusieurs alertes simultanées par utilisateur (actuellement : une alerte à la fois par actif)
- Connexion à une source de données réelle (API de marché) en complément de la simulation
- Persistance des alertes en base de données (actuellement en mémoire)

## 📂 Contact / Auteur

- **Auteur** : Ndongo Medoune NDIAYE
- **Email** : ndongomedoune.ndiaye@gmail.com
- **LinkedIn** : https://linkedin.com/in/nmndiaye
