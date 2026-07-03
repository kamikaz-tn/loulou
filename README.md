# Loulou — website

The website for Loulou (restaurant · brunch · café, in Sousse). Single page, no
framework, no build step — it's just HTML/CSS/JS. Open `index.html` and it runs
(needs internet for the fonts and the animation libraries).

The idea: you land on the night façade, and as soon as you scroll the camera moves
through the door into the dining room. After that, 4 cards: Reserve, Menu, Location,
Reviews. The site is in French by default, with a FR/EN switch top right.

## What's already wired up
- "Reserve" button → Instagram (loulourestaurant.tn)
- Address / directions → Google listing "Loulou Restaurant Sousse"
- The location map → OpenStreetMap (Google blocks its iframe without an API key, so I
  went with OSM, which works everywhere)

## The 3D scene images
The façade and its depth map are embedded directly into
`assets/images/scene-data.js`. That's on purpose: it makes the WebGL work everywhere,
even when opening the file locally. If I ever change the façade photo, I have to
regenerate that file (photo + depth map, made on immersity.ai or Depth Anything).

## The menu
Full menu in tabs (Restaurant / Café & Drinks), with the real dishes and prices in
Tunisian dinar.

## Google reviews
For now these are real reviews shown as static cards (the Google-style ones). To make
them live (auto-updating on their own):

1. free account on featurable.com
2. connect the Google listing "Loulou Restaurant Sousse"
3. create a widget and copy its Widget ID
4. paste it into `index.html` → `CONFIG.googleReviewsWidget`

From there the widget takes the place of the static cards. I went with a widget because
the direct Google API needs a key + a server and only returns 5 reviews max.

For the direct "leave a review" link, just drop the listing's Place ID into
`CONFIG.googleReview`.

## Security
A few things I paid attention to:

- SRI on the CDN libraries (three.js, gsap, lenis): if a CDN gets hacked, the modified
  code won't run.
- `_headers` file (CSP, anti-clickjacking, referrer, permissions, HSTS) — applied
  automatically on Netlify / Cloudflare Pages. On GitHub Pages these headers don't apply
  (Pages doesn't support custom headers).
- `rel="noopener noreferrer"` on all external links.
- The language stored in localStorage is validated (fr/en only).

No forms, no cookies, nothing sensitive on the client side.

## Going live
It's a static site, so it can be hosted just about anywhere:

- Netlify Drop (drag the folder) — keeps the `_headers` file.
- GitHub Pages — `git push`, then enable Pages on the `main` branch.
