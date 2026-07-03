# Loulou — site web

Le site de Loulou (restaurant · brunch · café, à Sousse). Une seule page, pas de
framework, pas de build : c'est du HTML/CSS/JS. On ouvre `index.html` et ça tourne
(il faut internet pour les polices et les librairies d'animation).

L'idée de départ : on arrive sur la façade de nuit, et dès qu'on scrolle la caméra
rentre par la porte dans la salle. Après ça, 4 cartes : Réserver, Carte, Adresse, Avis.
Le site est en français par défaut, avec un bouton FR/EN en haut à droite.

## Ce qui est déjà branché
- Bouton « Réserver » → Instagram (loulourestaurant.tn)
- Adresse / itinéraire → fiche Google « Loulou Restaurant Sousse »
- La carte de localisation → OpenStreetMap (Google bloque l'intégration sans clé API,
  du coup je suis passé par OSM qui marche partout)

## Les images de la scène 3D
La façade et sa carte de profondeur sont intégrées directement dans
`assets/images/scene-data.js`. C'est fait exprès : comme ça le WebGL marche partout,
même quand on ouvre le fichier en local. Si un jour je change la photo de façade, il
faut regénérer ce fichier (photo + depth map, faite sur immersity.ai ou Depth Anything).

## Le menu
Carte complète en onglets (Restaurant / Café & Boissons), avec les vrais plats et les
prix en dinar tunisien.

## Les avis Google
Pour l'instant ce sont de vrais avis affichés en dur (les cartes façon Google). Pour les
passer en direct (mis à jour tout seuls) :

1. compte gratuit sur featurable.com
2. connecter la fiche Google « Loulou Restaurant Sousse »
3. créer un widget et copier son Widget ID
4. le coller dans `index.html` → `CONFIG.googleReviewsWidget`

À partir de là le widget prend la place des cartes en dur. Je passe par un widget parce
que l'API Google directe demande une clé + un serveur et ne renvoie que 5 avis max.

Pour le lien « laisser un avis » direct, il suffit de mettre le Place ID de la fiche dans
`CONFIG.googleReview`.

## Sécurité
Quelques trucs auxquels j'ai fait attention :

- SRI sur les librairies CDN (three.js, gsap, lenis) : si un CDN se fait pirater, le code
  modifié ne s'exécute pas.
- Fichier `_headers` (CSP, anti-clickjacking, referrer, permissions, HSTS) — appliqué
  automatiquement sur Netlify / Cloudflare Pages. Sur GitHub Pages ces en-têtes ne
  s'appliquent pas (Pages ne gère pas les en-têtes custom).
- `rel="noopener noreferrer"` sur tous les liens externes.
- La langue stockée en localStorage est validée (fr/en seulement).

Pas de formulaire, pas de cookie, rien de sensible côté client.

## Mise en ligne
C'est un site statique, ça se met en ligne à peu près n'importe où :

- Netlify Drop (glisser le dossier) — garde le fichier `_headers`.
- GitHub Pages — `git push` puis activer Pages sur la branche `main`.
