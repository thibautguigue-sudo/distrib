# 🚀 Guide de déploiement — Distribution Centre-ville

## Ce que contient ce projet

- `public/index.html` → Page de pilotage : carte temps réel + dashboard 15 secteurs
- `public/equipes/equipe-2-a.html` à `equipe-2-o.html` → 15 pages terrain (mobile-first, géoloc auto)
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

> Note : `firebase deploy` déploie à la fois le site ET les règles Firestore d'un coup.
> Si besoin de ne redéployer que les règles : `firebase deploy --only firestore:rules`

---

## Comment ça marche ensuite

### Pour les équipes terrain (15 liens)
Tu leur envoies le lien direct de leur page :
- Équipe 2-A : `.../equipes/equipe-2-a.html`
- Équipe 2-B : `.../equipes/equipe-2-b.html`
- Équipe 2-C : `.../equipes/equipe-2-c.html`
- Équipe 2-D : `.../equipes/equipe-2-d.html`
- Équipe 2-E : `.../equipes/equipe-2-e.html`
- Équipe 2-F : `.../equipes/equipe-2-f.html`
- Équipe 2-G : `.../equipes/equipe-2-g.html`
- Équipe 2-H : `.../equipes/equipe-2-h.html`
- Équipe 2-I : `.../equipes/equipe-2-i.html`
- Équipe 2-J : `.../equipes/equipe-2-j.html`
- Équipe 2-K : `.../equipes/equipe-2-k.html`
- Équipe 2-L : `.../equipes/equipe-2-l.html`
- Équipe 2-M : `.../equipes/equipe-2-m.html`
- Équipe 2-N : `.../equipes/equipe-2-n.html`
- Équipe 2-O : `.../equipes/equipe-2-o.html`

(remplacer `...` par `https://distrib-aix-2026.web.app` ou ton domaine)

Ils ouvrent sur leur téléphone :
1. La carte s'affiche avec les points de distribution
2. La géolocalisation se lance automatiquement (popup d'autorisation à la 1ère visite uniquement)
3. Ils cochent les adresses → tout se sauvegarde automatiquement
4. Le point sur la carte passe au vert ✅

### Pour toi (pilotage)
Ouvre la page d'accueil : `https://distrib-aix-2026.web.app`

- Carte complète avec les 1 839 adresses (points qui passent au vert en direct)
- Avancement global (compteurs + barre de progression)
- Détail secteur par secteur (%, nom de l'équipe, lien vers la fiche)
- Clic sur un secteur = zoom sur la zone

### Indicateurs de connexion
- 🟢 Point vert = synchronisé avec Firestore en temps réel
- 🔴 Point rouge = pas de connexion (les coches fonctionnent en local, synchronisation au retour du réseau)

---

## En cas de problème

**"Permission denied" dans la console du navigateur :**
→ Les règles Firestore ne sont pas déployées. Relance `firebase deploy --only firestore:rules`

**La page se charge mais pas de synchro :**
→ Vérifie que `config.js` contient les bonnes valeurs (pas les placeholders "VOTRE_...")

**La carte ne s'affiche pas :**
→ Recharge la page. Vérifie qu'il n'y a pas de bloqueur de contenu (les tuiles viennent de cartocdn.com)

**La géoloc ne se lance pas :**
→ Le site doit être en HTTPS (c'est le cas avec Firebase Hosting). Vérifie que la permission a été accordée dans les réglages du navigateur.

**Remettre à zéro un secteur :**
→ Console Firebase > Firestore > collection `distribution` > document `2-a` → Supprimer

**Remettre tout à zéro :**
→ Console Firebase > Firestore > collection `distribution` → Supprimer la collection

**Nom de domaine personnalisé :**
→ Console Firebase > Hosting > Ajouter un domaine personnalisé (ex: `distrib.pnyx20.fr`)
