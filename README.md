# QALB — Plateforme d’apprentissage islamique

QALB est un projet francophone qui vise à rendre l’apprentissage de l’islam plus accessible, progressif et attentif à la qualité des sources.

## Version actuelle

Cette version est une interface statique, mobile-first, réalisée en HTML, CSS et JavaScript natifs.

### Fonctionnalités de l’interface
- Navigation responsive avec menu mobile.
- Thème clair/sombre mémorisé dans le navigateur.
- Parcours de découverte par grands domaines.
- Recherche et filtres dans la bibliothèque de ressources.
- Recommandations de premières lectures selon le sujet choisi.
- FAQ accessible et fenêtre d’information sur QALB IA.
- Métadonnées de partage social et structure sémantique de base.

## QALB IA : état et principes

La section QALB IA présente le concept, mais **aucun modèle d’intelligence artificielle n’est encore connecté**. La page n’envoie aucune question à un service externe et ne prétend pas fournir de réponses générées.

Avant une mise en production de l’assistant, le projet devra intégrer :
1. Un backend sécurisé (aucune clé API dans le navigateur).
2. Une base documentaire vérifiée et versionnée : Coran, traductions, tafsirs et recueils de hadiths avec références.
3. Une recherche documentaire (RAG) qui impose des sources pour les réponses factuelles.
4. Des garde-fous : refus de fabriquer des références, signalement des incertitudes et redirection des questions de fatwa vers des personnes qualifiées.
5. Une politique de confidentialité, une limitation de débit, une journalisation respectueuse des données et des tests d’évaluation.
6. Une revue éditoriale et religieuse avant publication.

L’assistant doit être présenté comme un outil pédagogique, jamais comme un savant ou une autorité religieuse.

## Développement local

Aucune compilation n’est nécessaire pour cette version.

- Ouvrir `index.html` dans un navigateur.
- Pour un serveur local : `python3 -m http.server 8000`
- Puis visiter `http://localhost:8000`.

## Déploiement

Le site peut être servi comme un Static Site. Le répertoire de publication est la racine du dépôt (`.`) et aucune commande de build n’est requise.

## Contenus

Les cartes de la bibliothèque sont des sujets à développer, et non des cours complets. Les contenus religieux doivent être ajoutés avec des références précises, des traductions identifiées, le contexte nécessaire et une indication des divergences pertinentes.
