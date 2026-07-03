# LOULOU — site web

Site une page, cinématographique. Langue par défaut : **français** (bouton FR/EN en haut à droite).
On arrive sur la **façade de nuit**, on défile pour **entrer par la porte dans la salle**, puis 4 cartes apparaissent : **Réserver · Carte · Adresse · Avis**.

Pas de build. Ouvrir `index.html` dans un navigateur (connexion internet requise pour les animations + polices).

Liens déjà branchés : Instagram `loulourestaurant.tn`, Google Maps (Loulou Restaurant Sousse).

---

## 1. Photos — à déposer dans `assets/images/`

- `exterior.jpg` → la façade de nuit
- `interior.jpg` → la salle

### Effet 3D « on entre dans le restaurant »
Pour le vrai effet de profondeur, va sur **https://immersity.ai**, importe la photo de la façade,
puis exporte **la carte de profondeur** sous le nom `exterior-depth.png` (à mettre aussi dans `assets/images/`).
Dis-moi quand c'est fait et je remplace la scène par une vraie scène WebGL (Three.js) où le défilement
fait avancer la caméra *à travers* la porte. (Sans ce fichier, l'effet actuel — zoom + ouverture de la porte — fonctionne déjà.)

## 2. Menu ✅ fait
La carte complète (Restaurant + Café & Boissons) est intégrée avec les vrais plats et prix,
en onglets, prix en dinar tunisien. Pour ajouter/modifier un plat, dis-le moi.

## 3. Avis Google réels et mis à jour automatiquement
Les faux avis ont été retirés. Pour afficher les **vrais** avis Google (mis à jour tout seuls) :

1. Va sur **https://featurable.com** (gratuit) → crée un compte.
2. Connecte ta fiche **Google Business « Loulou Restaurant Sousse »**.
3. Crée un widget d'avis → copie son **Widget ID** (un identifiant type `a1b2c3d4-...`).
4. Colle-le dans `index.html` : `CONFIG.googleReviewsWidget = "TON_WIDGET_ID"`.

Les vrais avis s'afficheront automatiquement à la place du message d'attente.
(Pourquoi pas l'API Google directement : elle exige une clé secrète + un serveur, et ne renvoie
que 5 avis max — un widget est gratuit, complet et sans serveur.)

## 4. Lien « Laisser un avis » direct (optionnel)
Pour ouvrir directement la fenêtre d'avis Google, récupère ton **Place ID** ici :
https://developers.google.com/maps/documentation/places/web-service/place-id
puis remplace `YOUR_PLACE_ID` dans `CONFIG.googleReview` (en bas de `index.html`).

---

## 🔒 Sécurité (déjà appliquée)
- **SRI** sur toutes les librairies CDN (Three.js, GSAP, ScrollTrigger, Lenis) → un CDN piraté ne peut pas injecter de code.
- **`_headers`** : CSP, anti-clickjacking, anti-MIME-sniffing, Referrer-Policy, Permissions-Policy, HSTS.
  → Ces en-têtes s'appliquent **automatiquement sur Netlify / Cloudflare Pages**. Sur un autre
  hébergeur, configure-les côté serveur (ou dis-le moi).
- **`rel="noopener noreferrer"`** sur tous les liens externes (anti « reverse tabnabbing »).
- Langue lue depuis `localStorage` **validée** (fr/en uniquement).
- Aucune donnée sensible côté client, aucun formulaire, aucun cookie.

*Note : la CSP autorise `'unsafe-inline'` pour les scripts car le JS est inline dans la page.
Il n'y a aucune entrée utilisateur (donc pas de risque XSS), mais si tu veux le durcir davantage,
on peut sortir le JS dans un fichier séparé et retirer `'unsafe-inline'`.*

## Mettre en ligne (gratuit)
- **Netlify Drop** : https://app.netlify.com/drop → glisser tout le dossier. En ligne en quelques secondes.
- GitHub Pages / Vercel marchent aussi (site statique).
