# Documentation · Landing pages Meta Gassien

Ce document décrit les trois landing pages des campagnes Meta (FR, EN, DE) : leur contenu, leur fonctionnement technique, leur intégration dans WordPress et la façon de les faire évoluer sans rien casser.

Pour une mise à jour rapide, le [README](../README.md) suffit. Ce document sert quand on modifie la structure, le style, le suivi ou qu'on ajoute une langue.

---

## 1. Vue d'ensemble

| Langue | Fichier | Page en ligne | `lp_page` |
|---|---|---|---|
| FR | `gassien-landing-meta.html` | https://www.gassien.com/donnez-vie-a-vos-murs/ | `landing_meta` |
| EN | `gassien-landing-meta-en.html` | https://www.gassien.com/en/bring-your-walls-to-life/ | `landing_meta_en` |
| DE | `gassien-landing-meta-de.html` | https://www.gassien.com/de/waende-zum-leben-erwecken/ | `landing_meta_de` |

- **Objectif** : convertir le trafic des publicités Meta vers le configurateur (`/maker/`), avec deux objectifs secondaires : la commande d'échantillons et le contact professionnel.
- **Format** : chaque fichier est un bloc autonome collé dans un bloc **HTML personnalisé** WordPress, sur une page en modèle pleine page. Il contient un `<link>` Google Fonts, un `<style>`, le contenu et un `<script>`. Pas de `<html>`, `<head>` ni `<body>`.
- **Les trois fichiers ont la même structure, le même CSS et le même JavaScript.** Seuls changent les textes, les liens (préfixe `/en/` ou `/de/`), l'objet du mail pro, la valeur `lp_page` et, pour l'allemand, la mention de livraison (voir § 8).

---

## 2. Structure de la page

Ordre des sections, avec le commentaire HTML qui les repère dans le code :

| # | Section | Contenu | Fond |
|---|---|---|---|
| — | Barre marque | Logo (non cliquable, volontairement) + bouton configurateur | blanc |
| 1 | Hero | Vidéo 4:5 (mur vide → composition), H1, usages, promesse, « Prix affiché en direct », 2 boutons, « Fabrication française » | beige pâle |
| 2 | Réassurance | 4 engagements avec icônes SVG (échantillons, livraison, paiement, satisfait ou remboursé) | blanc |
| 3 | Comment ça marche | Vidéo concept 16:9 + frise de 5 étapes avec icônes | beige pâle |
| 3 bis | Comparatif | Tableau « sur-mesure classique / Gassien » + 3 exemples chiffrés | blanc |
| 4 | Clients | Carrousel sur une ligne, 17 photos clients avec crédits Instagram | beige |
| 5 | Pièces et murs | 6 cartes « ce que Gassien change dans chaque pièce » + 4 cartes particularités (angle, longueur, hauteur, brique) | blanc |
| 6 | Le beau et l'utile | Texte + grande image | beige pâle |
| 6 ter | Finitions | Pastilles bois (4) et métal (3), lien échantillons | beige |
| 6 bis | Professionnels | Lieux, 3 arguments, gros bouton « Nous contacter » (mailto) | blanc |
| 7 | Simple. Durable. | 8 blocs de valeurs, tous de même hauteur | beige |
| 8 | CTA final | Promesse, 2 boutons, lien pro | bleu nuit |
| — | Mini footer | Mentions légales, CGV, livraison, cookies, contact | bleu nuit |
| — | Barre CTA mobile | Bouton fixe en bas d'écran (mobile et tablette uniquement) | blanc translucide |

Le plan des titres est : un seul H1 (hero), un H2 par section, des H3 pour les sous-parties (étapes, cartes, groupes de finitions et de valeurs).

---

## 3. Design system

### Couleurs (variables CSS sur `.lp-wrapper`)

| Variable | Valeur | Usage |
|---|---|---|
| `--lp-ink` | `#263543` | Texte principal, fonds sombres |
| `--lp-ink-soft` | `#55606b` | Texte secondaire |
| `--lp-gold` | `#8E7A3B` | Boutons principaux, accents |
| `--lp-gold-dark` | `#75652F` | Survol des boutons |
| `--lp-beige` | `#F4F0E9` | Aplats beige |
| `--lp-pale` | `#FAF8F4` | Aplats beige pâle |
| `--lp-white` | `#FFFFFF` | Fonds blancs |
| `--lp-line` | `#E4DED3` | Filets et bordures |

### Typographie

- **Montserrat** (300, 400, 500, 600) : titres, boutons, libellés.
- **Nunito Sans** (400, 600, 700) : texte courant.
- Chargées via Google Fonts, dans le `<link>` en tête de fichier.
- Titres en Montserrat Light (300). Les tailles utilisent `clamp()` pour s'adapter à l'écran (H1 de 36 à 68 px, H2 de 28 à 48 px).

### Points de rupture

| Largeur | Comportement |
|---|---|
| < 375 px | Très petits écrans : texte des blocs de valeurs réduit, pastilles bois sur 2 colonnes |
| < 640 px | Mobile (base du CSS, écrit en mobile first) |
| ≥ 640 px | Tablette : boutons côte à côte, grilles à 2-3 colonnes |
| ≥ 960 px | Desktop : hero en 2 colonnes, frise horizontale, barre CTA mobile masquée |

### Composants réutilisables

- **Boutons** : `.lp-btn` + `.lp-btn-primary` (doré) ou `.lp-btn-ghost` (contour). `.lp-btn-arrow` ajoute une flèche, `.lp-btn-lg` agrandit (bouton contact pro).
- **Cadre photo 4:5** : `.lp-frame`. L'image est en `object-fit: contain` et alignée en bas : le produit n'est jamais recadré.
- **Carte pièce** : `.lp-space` (libellé doré `.lp-space-room`, titre `.lp-h3`, texte `.lp-text`). Horizontale sur mobile, verticale à partir de 640 px.
- **Icônes** : SVG au trait dans le code (`.lp-ico`), couleur `--lp-gold`. Aucun fichier à téléverser.

---

## 4. Intégration WordPress

### Conflits avec le thème (Shapely, basé sur Bootstrap 3) et parades

Ces règles sont indispensables : les retirer casse l'affichage en ligne, même si l'aperçu local reste correct.

| Problème du thème | Parade dans la page |
|---|---|
| `html { font-size: 10px }` : toute taille en `rem` est réduite à 62 % | **Toutes les tailles sont en `px`.** Ne jamais utiliser `rem`. |
| `p, span { font-weight: 400 }` et `body, div, p, span… { font-family: 'Nunito Sans' }` | Le reset impose `font-family: inherit` et `font-weight: inherit` aux éléments de la page |
| Contenu enfermé dans le conteneur Bootstrap (`.container`), donc pas de pleine largeur | `.lp-wrapper` s'étend sur `100vw` en compensant la barre de défilement (variable `--lp-sbw`, calculée en JS) |
| Styles de tableau (bordures, marges, `line-height: 24px !important`) | Reset renforcé avec `!important` sur `.lp-compare` |
| WordPress peut insérer des `<p>` et `<br>` dans le HTML collé | **Aucune ligne vide** dans les fichiers |

### Conventions CSS

- Toutes les classes sont préfixées `lp-`.
- Toutes les règles sont rattachées à `.lp-wrapper` (`.lp-wrapper .lp-xxx`). Cela isole la page du thème et donne la priorité à ses règles.
- Le reset en tête du `<style>` ne s'applique qu'à `.lp-wrapper` et à ses enfants.

### Réglages Yoast par page

| Champ | FR | EN | DE |
|---|---|---|---|
| Slug | `donnez-vie-a-vos-murs` | `bring-your-walls-to-life` | `waende-zum-leben-erwecken` |
| Titre SEO | Donnez vie à vos murs : aménagement mural modulable \| Gassien | Bring your walls to life: modular wall system \| Gassien | Wände zum Leben erwecken: modulares Wandsystem \| Gassien |
| Méta description | Bibliothèque, bureau, dressing, mur végétal : composez en ligne votre aménagement mural modulable, fabriqué en France. Prix affiché en direct. | Bookcase, desk, wardrobe, living wall: design your modular wall system online, made in France. See the price live as you create it. | Bücherregal, Schreibtisch, Ankleide, Pflanzenwand: Gestalten Sie Ihr modulares Wandsystem online, hergestellt in Frankreich. Preis in Echtzeit. |
| Expression clé | aménagement mural modulable | modular wall system | modulares Wandsystem |
| Titre réseaux sociaux | Donnez vie à vos murs | Bring your walls to life | Wände zum Leben erwecken |

- **Indexation** (onglet Avancé) : `noindex, follow` sur les trois pages. Les pages sont absentes du sitemap. **Ne pas bloquer les pages dans `robots.txt`** : le robot de Meta doit pouvoir les lire.
- **Image de partage** : `OG_GASSIEN_SANS_TEXTE_B.jpg` (1200 × 630, sans texte, valable pour les trois langues), à sélectionner en **taille complète**.
- **WPML** relie les trois pages (hreflang `fr`, `en`, `de`, avec le français en `x-default`).

---

## 5. Médias

Tous les médias sont hébergés dans la médiathèque WordPress, à l'adresse `https://www.gassien.com/wp-content/uploads/2026/10/`. Ils sont partagés par les trois langues.

**Pour remplacer un média, garder exactement le même nom de fichier.** Si WordPress ajoute un suffixe (`-1`, `-scaled`…), l'URL change et il faut la mettre à jour dans les trois fichiers. Les dimensions déclarées (`width`/`height`) doivent correspondre au fichier.

| Fichier | Format | Usage |
|---|---|---|
| `VIDEO_HERO.mp4` | 1080 × 1350 (4:5), sans son | Vidéo du hero |
| `IMG_HERO_POSTER_FIN.webp` | 1000 × 1250 | Image d'attente du hero (composition terminée) |
| `VIDEO_CONCEPT.mp4` | 1280 × 720 (16:9), sans son | Vidéo « Comme un jeu de construction » |
| `IMG_CONCEPT_POSTER.webp` | 1280 × 720 | Image d'attente de la vidéo concept |
| `IMG_PRIX_1` à `_3.webp` | 900 × 900 | Exemples chiffrés (petit, moyen, grand) |
| `IMG_CLIENT_01` à `_17.webp` | 800 × 800 | Carrousel clients |
| `IMG_ESPACE_*.webp` (6) | 4:5 | Cartes pièces : CHAMBRE, SALON, CUISINE, ENTREE, DRESSING, PRO |
| `IMG_PARTICULARITE_1` à `_4.webp` | 4:5 | Angle, grande longueur, hauteur sous plafond, mur en brique |
| `IMG_BEAU_UTILE.webp` | 1029 × 1528 | Section « Le beau et l'utile » |
| `IMG_PRO.webp` | 864 × 1080 | Section professionnels |
| `OG_GASSIEN_SANS_TEXTE_B.jpg` | 1200 × 630 | Image de partage (Yoast) |

Formats attendus par les cadres : **4:5** pour les cartes pièces et particularités, **carré** pour le carrousel et les exemples chiffrés. Recadrer en douceur (ciel, sol), jamais dans une composition.

Le logo vient de `https://www.gassien.com/wp-content/uploads/2017/01/logo.png`.

---

## 6. Comportements JavaScript

Le script, en fin de fichier, est encapsulé (aucune variable globale) et ne dépend d'aucune bibliothèque.

| Fonction | Détail |
|---|---|
| Pleine largeur | Calcule la largeur de la barre de défilement (`--lp-sbw`) au chargement et au redimensionnement |
| Vidéo hero | Lecture automatique, en boucle et muette. Désactivée si l'utilisateur a demandé à réduire les animations |
| Vidéo concept | Chargée et lancée seulement quand elle entre à l'écran, mise en pause en sortant |
| Apparition douce | Les blocs `.lp-reveal` apparaissent au défilement (désactivé si mouvement réduit) |
| Carrousel | Défilement natif au doigt. Flèches à partir de 640 px, grisées en début et en fin |
| Barre CTA mobile | Visible tant qu'**aucun** bouton marqué `data-lp-cta` n'est à l'écran |
| Footer | Année courante ; « Gérer mes cookies » ouvre le panneau tarteaucitron (repli : page des mentions légales) |
| Suivi | Voir § 7 |

---

## 7. Suivi et mesure

Chaque élément cliquable suivi porte `data-lp-event` (nom de l'événement) et `data-lp-pos` (emplacement).

| Événement | Déclencheur | Événement Meta standard |
|---|---|---|
| `click_configurator` | Tous les boutons vers le configurateur | `Lead` |
| `click_samples` | Liens et boutons vers les échantillons | `Lead` |
| `click_contact` | Bouton et lien mailto pro | `Contact` |
| `click_pro` | Liens vers l'espace pro | — |
| `scroll_depth` | Défilement à 25, 50, 75 et 90 % | — |

Emplacements (`lp_position`) : `topbar`, `hero`, `reassurance`, `etapes`, `comparatif`, `espaces`, `beau_utile`, `finitions`, `section_pro`, `section_pro_email`, `final`, `sticky_mobile`.

- **Google Tag Manager** (`GTM-M43M5GSN`) : chaque événement est poussé dans le `dataLayer` avec `lp_page` (langue) et `lp_position`.
- **Meta Pixel** (`2379927652067336`) : `fbq('trackCustom', …)` pour l'événement Gassien, plus l'événement standard défini dans `META_STANDARD` en haut du script.
- **Consentement** : le pixel n'est chargé par GTM qu'après acceptation dans tarteaucitron (service `gassien_pub`). Avant consentement, `fbq` n'existe pas et seul le `dataLayer` reçoit les événements.

---

## 8. Règles de contenu

Ces choix ont été validés et doivent être conservés lors des modifications :

- **Ne pas employer le mot « étagère »** (shelf, Regal) : c'est réducteur. Parler d'usages (bibliothèque, bureau, dressing, mur végétal…) et de « composition » ou de « système mural modulable ».
- **Promesse prix** : pas de prix « à partir de » (une petite composition à 100 € n'est pas représentative). On met en avant le « prix affiché en direct » et trois exemples réels chiffrés.
- **Angle sur-mesure** : « le sur-mesure, sans ses contraintes » (et non « sans le prix »), le prix restant la première contrainte citée.
- **Bois** : « bois massif issu de forêts gérées durablement » ; essences : chêne massif, bouleau, hêtre laqué noir, hêtre laqué blanc. Métal : noir, blanc, laiton.
- **Livraison offerte dès 150 €** : uniquement France métropolitaine et Belgique. La version **DE** ne l'affiche que dans le bandeau d'engagements, avec cette restriction (ni dans le hero, ni à l'étape 3).
- **Prix des exemples** (264 €, 627 €, 896 €) : écrits en dur dans les trois fichiers, à mettre à jour si le tarif change. Format : `264 €` (FR, DE) et `€264` (EN).
- **Photos clients** : crédits Instagram affichés, accord des clients obtenu. Les photos `@gassienparis` ne sont pas créditées.
- **Pas de faux avis** : la preuve sociale repose sur les photos clients, pas sur des témoignages inventés.

---

## 9. Procédures

### Modifier un texte

1. Modifier la phrase dans le fichier de la langue concernée, et la traduction équivalente dans les deux autres si le sens change.
2. Vérifier qu'aucune ligne vide n'a été ajoutée.
3. Recoller le fichier dans le bloc HTML personnalisé de la page WordPress.

### Modifier le style ou la structure

1. Appliquer **la même modification aux trois fichiers** (CSS et JS identiques).
2. Respecter les conventions : préfixe `lp-`, règles rattachées à `.lp-wrapper`, tailles en `px`.
3. Tester avec le CSS du thème (voir la checklist ci-dessous), pas seulement en aperçu local.

### Ajouter une langue

1. Copier `gassien-landing-meta.html` en `gassien-landing-meta-xx.html`.
2. Traduire les textes visibles, mais aussi les `alt`, les `aria-label`, l'objet du `mailto` et le texte caché « Étape n : ».
3. Remplacer les liens par ceux de la langue (les retrouver via les liens hreflang des pages FR du site).
4. Changer `lp_page` dans le script (`landing_meta_xx`).
5. Vérifier les conditions commerciales du pays (livraison, paiement) et adapter les promesses.
6. Créer la traduction WPML, coller le bloc, régler Yoast (slug, title, description, noindex, image de partage).

### Checklist de contrôle après publication

- [ ] La page répond, avec `noindex, follow`, et les hreflang relient les trois langues.
- [ ] Le bloc est intact : pas de `<p>` ou `<br>` dans le `<style>` ou le `<script>`, `&&` non transformés dans le JS.
- [ ] Rendu à 1440, 390 et 320 px : H1 à 68 px sur desktop et 36 px sur mobile, pleine largeur, pas de défilement horizontal.
- [ ] Toutes les images et les deux vidéos se chargent ; aucune erreur dans la console.
- [ ] Le tableau comparatif n'a pas de bordures parasites.
- [ ] La bannière cookies s'affiche dans la bonne langue ; aucun pixel avant consentement.
- [ ] Après consentement, Meta reçoit `PageView`, puis `click_configurator` et `Lead` au clic.
- [ ] L'aperçu de partage est à jour (Sharing Debugger de Meta → « Scrape Again »).

---

## 10. Hors dépôt

Volontairement exclus via `.gitignore` :

- le brief et les sources (`202610/` : cahier de passation, maquettes, vidéos et photos originales, photos clients) ;
- les médias exportés (`medias-wordpress/`), déjà hébergés sur WordPress ;
- les aperçus locaux, les copies avec placeholders et la première version de la page (fidèle au brief, non utilisée).
