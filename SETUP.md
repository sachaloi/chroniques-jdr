# Chroniques — Guide de configuration Firebase

Chroniques fonctionne en **mode local** par défaut (données non partagées).
Pour activer la synchronisation en temps réel entre joueurs, suivez ce guide.
C'est gratuit, sans carte bancaire, et prend environ 5 minutes.

---

## Étape 1 — Créer un projet Firebase

1. Allez sur [console.firebase.google.com](https://console.firebase.google.com)
2. Connectez-vous avec un compte Google
3. Cliquez sur **Ajouter un projet**
4. Donnez un nom au projet (ex. : `chroniques-jdr`)
5. Désactivez Google Analytics si vous ne le souhaitez pas
6. Cliquez sur **Créer le projet**

---

## Étape 2 — Activer la Realtime Database

1. Dans le menu de gauche, cliquez sur **Build → Realtime Database**
2. Cliquez sur **Créer une base de données**
3. Choisissez l'emplacement le plus proche (ex. : `europe-west1` pour la France)
4. Sélectionnez **Commencer en mode test** (vous avez 30 jours pour configurer les règles)
5. Cliquez sur **Activer**

---

## Étape 3 — Récupérer la configuration

1. Cliquez sur l'icône ⚙️ (roue crantée) en haut à gauche → **Paramètres du projet**
2. Faites défiler jusqu'à **Vos applications**
3. Cliquez sur l'icône `</>` (Web)
4. Donnez un surnom à l'app (ex. : `chroniques-web`), cliquez sur **Enregistrer l'application**
5. Firebase affiche un bloc `firebaseConfig` — copiez les valeurs

---

## Étape 4 — Coller la configuration dans index.html

Ouvrez `index.html` avec un éditeur de texte (Notepad, VS Code, etc.)
et cherchez ce bloc en haut du fichier :

```javascript
const FIREBASE_CONFIG = {
  apiKey:            "VOTRE_API_KEY",
  authDomain:        "VOTRE_PROJET.firebaseapp.com",
  databaseURL:       "https://VOTRE_PROJET-default-rtdb.europe-west1.firebasedatabase.app",
  projectId:         "VOTRE_PROJET",
  storageBucket:     "VOTRE_PROJET.appspot.com",
  messagingSenderId: "VOTRE_SENDER_ID",
  appId:             "VOTRE_APP_ID"
};
```

Remplacez chaque valeur par celles copiées depuis Firebase. Exemple :

```javascript
const FIREBASE_CONFIG = {
  apiKey:            "AIzaSyABCDEF...",
  authDomain:        "chroniques-jdr.firebaseapp.com",
  databaseURL:       "https://chroniques-jdr-default-rtdb.europe-west1.firebasedatabase.app",
  projectId:         "chroniques-jdr",
  storageBucket:     "chroniques-jdr.appspot.com",
  messagingSenderId: "123456789",
  appId:             "1:123456789:web:abcdef"
};
```

Sauvegardez le fichier.

---

## Étape 5 — Héberger le fichier (pour partager avec les joueurs)

**Option A — GitHub Pages (gratuit, recommandé)**
1. Créez un compte sur [github.com](https://github.com)
2. Créez un nouveau dépôt public, uploadez `index.html`
3. Allez dans *Settings → Pages → Source → main branch*
4. Votre app est accessible à : `https://VOTRE-PSEUDO.github.io/NOM-REPO/`

**Option B — Glitch.com (encore plus simple)**
1. Allez sur [glitch.com](https://glitch.com) → **New Project → Import from GitHub**
2. Ou créez un projet vide et collez le contenu de `index.html`
3. Glitch donne une URL publique immédiatement

**Option C — Ouvrir localement (même réseau uniquement)**
Ouvrez simplement `index.html` dans votre navigateur.
Partagez-le aux autres joueurs via le même réseau ou un partage de fichier.
Les données se synchroniseront via Firebase tant qu'il y a une connexion internet.

---

## Règles de sécurité Firebase (optionnel mais recommandé)

Après 30 jours, le mode test expire. Pour continuer, allez dans
**Realtime Database → Règles** et collez :

```json
{
  "rules": {
    "rooms": {
      "$roomId": {
        ".read":  true,
        ".write": true
      }
    }
  }
}
```

Cela autorise la lecture/écriture sur tout salon. Suffisant pour un usage privé entre amis.

---

## Utilisation de l'application

### Créer une session (MJ)
1. Ouvrez `index.html` dans votre navigateur
2. Saisissez un nom de campagne et cliquez **Créer la session**
3. Un **code à 6 lettres** apparaît en haut — partagez-le aux joueurs

### Rejoindre (joueurs)
1. Ouvrez la même URL dans votre navigateur
2. Saisissez le code de salon et cliquez **Rejoindre**
3. Cliquez sur votre personnage (ou créez-en un nouveau)

### Fiche de personnage
- **Stats 3×3** : cliquez sur les cercles pour modifier les valeurs (1–5). Cliquez deux fois sur le même rang pour revenir en arrière.
- **PV** : utilisez les boutons −/+ pour modifier les points de vie en direct
- **Consommables** : cliquez **Utiliser** pour décrémenter, **↺** pour restaurer manuellement, **Nouvelle session** pour tout remettre à zéro

### Vue MJ
Cliquez sur **Vue MJ** en haut à droite pour voir tous les personnages, leurs PV et leurs consommables en temps réel.
