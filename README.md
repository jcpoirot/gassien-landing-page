# Gassien · Landing pages Meta

Landing pages des campagnes Meta, intégrées dans WordPress via un bloc **HTML personnalisé** (modèle pleine page).

Documentation complète (structure, design system, intégration WordPress, médias, suivi, règles de contenu, procédures) : [docs/documentation.md](docs/documentation.md).

| Langue | Fichier | Page en ligne | `lp_page` (suivi) |
|---|---|---|---|
| FR | `gassien-landing-meta.html` | https://www.gassien.com/donnez-vie-a-vos-murs/ | `landing_meta` |
| EN | `gassien-landing-meta-en.html` | https://www.gassien.com/en/bring-your-walls-to-life/ | `landing_meta_en` |
| DE | `gassien-landing-meta-de.html` | https://www.gassien.com/de/waende-zum-leben-erwecken/ | `landing_meta_de` |

## Mettre à jour une page

1. Modifier le fichier de la langue concernée.
2. Copier tout son contenu dans le bloc HTML personnalisé de la page WordPress correspondante (traduction WPML).
3. Ne pas laisser de ligne vide dans le fichier : WordPress pourrait y insérer des `<p>` parasites.

Toute évolution de structure ou de style doit être reportée dans les trois fichiers.

## Points techniques

- **Autonome** : un `<style>`, le contenu et un `<script>`, sans `<html>`, `<head>` ni `<body>`. Toutes les classes sont préfixées `lp-` et tout le CSS est rattaché à `.lp-wrapper`.
- **Thème** : les tailles sont en `px` (le thème Shapely fixe `html { font-size: 10px }`). La page s'étend en pleine largeur et neutralise les styles de tableau et de typographie du thème.
- **Médias** : hébergés dans la médiathèque WordPress (`/wp-content/uploads/2026/10/`). Pour remplacer un média, garder exactement le même nom de fichier.
- **Suivi** : chaque clic envoie un événement dans le `dataLayer` (GTM) et, après consentement tarteaucitron, au Meta Pixel (`click_configurator` et `click_samples` → `Lead`, `click_contact` → `Contact`). Réglable via `META_STANDARD` dans le script.
- **SEO** : pages en `noindex, follow` (réglé dans Yoast).

## Contenus à surveiller

- Prix des exemples (264 €, 627 €, 896 €) : à mettre à jour si le tarif change.
- Livraison offerte dès 150 € : France métropolitaine et Belgique uniquement (la version DE ne l'affiche que dans le bandeau d'engagements, avec cette restriction).
- Photos clients du carrousel : crédits Instagram affichés, accord des clients obtenu.
