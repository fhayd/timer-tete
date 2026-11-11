# 🍼 Suivi des tétés

Application web simple pour enregistrer les tétés de bébé et se rappeler (vous et votre proche) 3 heures après le début de la dernière tété.

## Fonctionnalités

- **Compteur principal** : temps écoulé depuis le début de la dernière tété, mis à jour en direct (vert < 2 h, orange 2–3 h, rouge ≥ 3 h).
- **Bouton Démarrer / Terminer** : enregistre l'heure de début, le côté (gauche/droite) et la durée.
- **Synchro temps réel** : les deux téléphones voient les mêmes données instantanément via Firebase Realtime Database.
- **Notifications de rappel** : 3 h après le début de la dernière tété, notification navigateur + bip sonore, répétée toutes les 15 min tant qu'aucune nouvelle tété n'est enregistrée.
- **Historique** : 40 dernières tétés (jour, heure, côté, durée), avec suppression pour corriger une erreur.
- Utilise l'heure **du serveur Firebase** (pas de décalage si les horloges des téléphones diffèrent).

## Installation (une fois, ~10 minutes)

### 1. Créer la base Firebase (gratuit)

1. Aller sur [console.firebase.google.com](https://console.firebase.google.com) et créer un projet.
2. Menu **Realtime Database** → **Créer une base de données** → mode test.
3. **Paramètres du projet** → section « Vos applications » → ajouter une application **Web** → copier l'objet `firebaseConfig`.
4. Dans `index.html`, remplacer le bloc `FIREBASE_CONFIG` (en haut du script) par cet objet.

### 2. Règles de sécurité

Dans **Realtime Database → Règles**, coller :

```json
{
  "rules": {
    "familles": {
      "$famille": { ".read": true, ".write": true }
    }
  }
}
```

La clé de famille dans l'URL sert de mot de passe d'accès : utilisez une clé longue et aléatoire (ex. `#famille=bebe-7f3k9x2m`).

### 3. Héberger la page

Publier `index.html` et `sw.js` sur un hébergement statique :

- **GitHub Pages** : pousser les fichiers dans un dépôt → Settings → Pages → brancher sur `main`.
- Ou Netlify / Firebase Hosting (glisser-déposer).

### 4. Sur chaque téléphone

1. Ouvrir **la même URL complète** (avec le même `#famille=...`).
2. Ajouter à l'écran d'accueil (partage → « Sur l'écran d'accueil ») pour un effet application.
3. Cliquer sur **🔔 Activer les notifications** et accepter.

## Limites connues

- Les notifications ne partent que si l'onglet de l'app est ouvert (en arrière-plan c'est OK). Pour des notifications même app fermée, il faudrait passer aux Push Notifications + un serveur — possible en évolution.
- Sur iPhone, les notifications web nécessitent que l'app soit ajoutée à l'écran d'accueil (iOS ≥ 16.4).
- La clé de famille dans l'URL est une protection simple, pas un système de comptes. Ne partagez l'URL qu'avec votre proche.
