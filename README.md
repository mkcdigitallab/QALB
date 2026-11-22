# QALB — site statique multi-pages

Site francophone d'apprentissage de l'islam. HTML, CSS et JavaScript natifs, sans framework ni étape de build.

## Arborescence

- `index.html` : accueil
- `parcours/`, `coran/`, `sunna/`, `foi-pratique/`, `videos/`, `bibliotheque/`, `approche/`, `qalb-ia/`, `faq/`, `contact/` : pages autonomes
- `css/site.min.css` : styles centralisés, mobile-first et variables de thème
- `js/app.min.js` : thème, menu accessible et FAQ
- `js/library.js` : recherche et filtres
- `js/recommendations.js` : recommandations du parcours
- `js/site-config.js` : domaine canonique central
- `data/ressources.json` : ressources et recommandations
- `assets/img/` : identité SVG QALB
- `robots.txt`, `sitemap.xml`, `404.html`, `render.yaml` : publication et SEO

## Médias et identité visuelle

Les placeholders, les noms de fichiers photo/vidéo attendus, les recherches de médias et les commandes FFmpeg/WebP sont documentés dans [`MEDIAS.md`](MEDIAS.md). Les médias sont servis localement uniquement ; aucun hotlink d'image ou de vidéo n'est utilisé.

## Prévisualisation locale

Depuis la racine du dépôt :

```bash
python3 -m http.server 8000
```

Puis ouvrir `http://localhost:8000`.

## Changer de domaine

Modifier `siteUrl` dans `js/site-config.js`, puis remplacer l'ancien domaine dans les balises canonical/OG des pages, dans `robots.txt` et dans `sitemap.xml`. Les balises canonical sont présentes dans le HTML pour rester lisibles sans JavaScript.

## Render

Site statique publié depuis la racine (`staticPublishPath: .`), sans commande de build. Les caches et redirections de chemins propres sont décrits dans `render.yaml`.

## Retour arrière

La refonte est isolée sur `refonte`. L'ancien `index.html` reste intact sur `main` et dans l'historique Git. Après une fusion squash approuvée, revenir en arrière avec :

```bash
git revert <SHA_DU_COMMIT_DE_FUSION>
git push origin main
```

Ne fusionner la branche qu'après validation explicite.
