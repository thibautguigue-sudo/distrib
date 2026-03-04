# 🚀 Guide de déploiement — Distribution Centre-ville

## Ce que contient ce projet

- `public/index.html` → Tableau de bord temps réel (avancement des 17 équipes)
- `public/equipes/equipe-2-a.html` à `equipe-2-o.html` → 15 pages terrain
- `public/config.js` → **À compléter** avec tes identifiants Firebase
- `firebase.json` → Configuration hébergement
- `firestore.rules` → Règles de sécurité base de données

---

## Étape 1 — Créer le projet Firebase (~3 min)

1. Va sur **https://console.firebase.google.com**
2. Clique **"Ajouter un projet"**
3. Nom du projet : `distrib-aix-2026` (ou ce que tu veux)
4. Désactive Google Analytics (pas besoin ici), puis **"Créer le projet"**

## Étape 2 — Activer Firestore (~1 min)

1. Dans le menu à gauche, clique **"Firestore Database"** (ou "Cloud Firestore")
2. Clique **"Créer une base de données"**
3. Choisis **"Mode production"**
4. Région : **eur3 (Europe)** → Valider

## Étape 3 — Enregistrer une appli web (~1 min)

1. Dans les paramètres du projet (roue dentée en haut à gauche → "Paramètres du projet")
2. En bas de la page, section **"Vos applications"**, clique l'icône **</>** (Web)
3. Nom : `distribution` → **"Enregistrer l'application"**
4. Firebase affiche un bloc de code avec `firebaseConfig`. **Copie les valeurs.**

## Étape 4 — Remplir config.js (~30 sec)

Ouvre le fichier `public/config.js` et remplace les valeurs par celles de l'étape 3 :

```js
const firebaseConfig = {
  apiKey: "AIzaSy...",              // ← ta vraie clé
  authDomain: "distrib-aix-2026.firebaseapp.com",
  projectId: "distrib-aix-2026",
  storageBucket: "distrib-aix-2026.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef"
};
```

## Étape 5 — Installer Firebase CLI (~2 min)

Si tu ne l'as pas déjà :

```bash
npm install -g firebase-tools
```

Puis connecte-toi :

```bash
firebase login
```

## Étape 6 — Déployer (~1 min)

Depuis le dossier du projet :

```bash
cd firebase-distrib
firebase use distrib-aix-2026
firebase deploy
```

C'est tout. Firebase affiche l'URL du site, du type :
**https://distrib-aix-2026.web.app**

## Étape 7 — Déployer les règles Firestore

```bash
firebase deploy --only firestore:rules
```

---

## Comment ça marche ensuite

### Pour les équipes terrain
Tu leur envoies le lien direct de leur page, par exemple :
- `https://distrib-aix-2026.web.app/equipes/equipe-2-a.html`
- `https://distrib-aix-2026.web.app/equipes/equipe-2-b.html`
- etc.

Ils ouvrent sur leur téléphone, cochent les adresses au fur et à mesure. 
Tout se sauvegarde automatiquement.

### Pour toi (suivi)
Tu ouvres la page d'accueil :
- `https://distrib-aix-2026.web.app`

Tu vois en temps réel l'avancement de chaque équipe, avec barres de progression.

### Indicateur de connexion
- 🟢 Point vert = synchronisé avec la base
- 🔴 Point rouge = pas de connexion (les coches marchent quand même en local, elles se synchroniseront au retour du réseau)

---

## En cas de problème

**"Permission denied" dans la console du navigateur :**
→ Les règles Firestore ne sont pas déployées. Relance `firebase deploy --only firestore:rules`

**La page se charge mais pas de synchro :**
→ Vérifie que `config.js` contient les bonnes valeurs (pas les placeholders)

**Tu veux remettre à zéro un secteur :**
→ Console Firebase > Firestore > collection `distribution` > document `2-a` → Supprimer

**Tu veux un nom de domaine personnalisé :**
→ Console Firebase > Hosting > Ajouter un domaine personnalisé (ex: `distrib.pnyx20.fr`)
