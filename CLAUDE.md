# CLAUDE.md

Landing pages des campagnes Meta de Gassien Paris (aménagement mural modulable), en FR, EN et DE. Chaque fichier est un bloc HTML autonome collé dans un bloc « HTML personnalisé » WordPress. Documentation complète : `docs/documentation.md`.

Répondre en français.

## Fichiers

- `gassien-landing-meta.html` (FR), `gassien-landing-meta-en.html` (EN), `gassien-landing-meta-de.html` (DE).
- Les trois ont **la même structure, le même CSS et le même JS**. Toute modification de style, de structure ou de script doit être appliquée aux trois. Seuls diffèrent : textes, `alt`/`aria-label`, liens (`/en/`, `/de/`), objet du mailto, `lp_page` (`landing_meta`, `landing_meta_en`, `landing_meta_de`).

## Règles techniques (le thème Shapely casse tout sinon)

- **Tailles en `px` uniquement, jamais en `rem`** : le thème fixe `html { font-size: 10px }`.
- Toutes les classes préfixées `lp-`, toutes les règles CSS rattachées à `.lp-wrapper` (`.lp-wrapper .lp-xxx`).
- Ne pas retirer : le reset `font-family/font-weight: inherit`, la pleine largeur via `--lp-sbw`, le reset `!important` du tableau `.lp-compare`.
- **Aucune ligne vide** dans les fichiers (WordPress injecte des `<p>`).
- Pas de `<html>`, `<head>`, `<body>`. JS vanilla, encapsulé dans une IIFE.
- Médias : `https://www.gassien.com/wp-content/uploads/2026/10/NOM.webp|mp4`. Garder `width`/`height` égaux aux dimensions réelles. Cadres 4:5 (pièces, particularités) et carrés (carrousel, prix).
- Boutons vers le configurateur : `data-lp-event="click_configurator"`, un `data-lp-pos` unique, et `data-lp-cta` s'ils doivent masquer la barre mobile.

## Règles de contenu (validées par le client)

- Ne pas écrire « étagère » (shelf, Regal) : parler d'usages et de « composition » ou de « système mural modulable ».
- Pas de prix « à partir de ». Mettre en avant le « prix affiché en direct » et les 3 exemples chiffrés (264 €, 627 €, 896 €).
- « Le sur-mesure, sans ses contraintes » (pas « sans le prix »).
- Bois : « bois massif issu de forêts gérées durablement » ; chêne massif, bouleau, hêtre laqué noir/blanc. Métal : noir, blanc, laiton.
- Livraison offerte dès 150 € : France métropolitaine et Belgique uniquement. En DE, seulement dans le bandeau d'engagements, avec la restriction.
- Pas de faux avis ni de chiffres inventés ; toute nouvelle promesse produit est à faire confirmer par le client.

## Vérifier une modification

- Tester le rendu **avec le CSS du thème**, pas seulement en local : charger les feuilles de style de gassien.com autour du bloc, puis contrôler à 1440, 390 et 320 px (tailles de police, pleine largeur, pas de défilement horizontal, tableau sans bordures, aucune erreur console).
- En headless, le Meta Pixel ignore les navigateurs automatisés : pour tester le suivi, se faire passer pour un navigateur normal (user agent, `navigator.webdriver = false`) et accepter les cookies tarteaucitron (`#tarteaucitronAllAllowed`).
- Dans zsh, écrire `"${f}[0]"` et non `"$f[0]"` avec ImageMagick ; les noms de fichiers commençant par `@` doivent être copiés sous un nom neutre avant traitement.

## Git

- Ne jamais committer `202610/` (brief, photos clients), `medias-wordpress/`, les aperçus `apercu-local*.html`, `modeles/`, `gassien-landing.html`.
