# MEDIAS.md — Plan de production visuelle QALB

Ce document inventorie tous les médias prévus. Les illustrations dans `assets/img/placeholders/` sont des **placeholders SVG locaux**, pas des photographies. Elles restent visibles tant que les vrais fichiers WebP ne sont pas déposés. Aucun média distant n'est appelé par le site.

## Direction photographique

Ambiance éditoriale, cinématographique et contemplative : pierre claire, bois sombre, lumière rasante, ombres profondes, palette vert forêt / ivoire / laiton doux. Cadres épurés, composition avec de l'espace négatif pour le texte. **Aucun visage, aucun corps identifiable, aucun animal ni être vivant dans un cadre religieux.** Pas de représentation du Prophète ﷺ, pas de scènes de prière reconstituées. Préférer architecture, pages de livres sans texte religieux inventé, calligraphie authentique licenciée, motifs géométriques, ciel, lumière et nature sans êtres vivants.

## 1. Vidéo d'ambiance du hero

| Fichier | Dimensions / durée | Format | Poids maximum | Idée visuelle | Recherche Pexels / Pixabay |
|---|---:|---|---:|---|---|
| `assets/video/hero.webm` | 1280×720, 5–8 s, 24 fps | WebM VP9, muet | 850 Ko | Travelling lent dans une arcade d'architecture islamique, lumière du matin, poussière lumineuse discrète, sans personne. Boucle douce. | `islamic architecture mosque arch sunlight cinematic empty interior no people` |
| `assets/video/hero.mp4` | 1280×720, 5–8 s, 24 fps | MP4 H.264, muet | 650 Ko | Même séquence et même cadrage que le WebM, pour compatibilité. | Même recherche |
| `assets/img/hero-poster.webp` | 1280×720 | WebP | 80 Ko | Image fixe tirée de la vidéo, idéalement une image nette du début de la boucle. | Même recherche |

**Budget cumulé WebM + MP4 : 1,5 Mo maximum.** Le site affiche immédiatement le poster SVG local puis le remplace par le poster WebP s'il est présent. La vidéo ne reçoit ses sources qu'après le premier affichage. Elle ne se charge pas si `prefers-reduced-motion`, `navigator.connection.saveData`, 2G ou slow-2G sont détectés. Elle est toujours muette (`muted`) et sans contrôles sonores.

## 2. Photographies principales

Chaque sujet possède trois variantes WebP. Les trois variantes doivent provenir du même original et garder le même cadrage 16:10.

| Ensemble | Fichiers attendus | Dimensions | Poids max par fichier | Idée visuelle | Requête Pexels / Pixabay |
|---|---|---|---:|---|---|
| Accueil éditorial | `assets/img/home-editorial-480.webp`, `home-editorial-960.webp`, `home-editorial-1600.webp` | 480×300, 960×600, 1600×1000 | 45 / 110 / 240 Ko | Arcade de pierre, rayon de lumière dorée, géométrie d'ombre, composition sobre. | `islamic architecture interior sunlight geometric shadows empty no people` |
| Le Coran | `assets/img/coran-hero-480.webp`, `coran-hero-960.webp`, `coran-hero-1600.webp` | 480×300, 960×600, 1600×1000 | 45 / 110 / 240 Ko | Livre ouvert photographié avec respect, lumière douce, détail des pages, sans texte généré ni main/personne. | `open Quran book pages wooden rehal warm natural light close up no people` |
| La Sunna | `assets/img/sunna-hero-480.webp`, `sunna-hero-960.webp`, `sunna-hero-1600.webp` | 480×300, 960×600, 1600×1000 | 45 / 110 / 240 Ko | Manuscrit ancien, bibliothèque silencieuse, étagères et lumière latérale, sans personnes. | `islamic manuscript library bookshelves warm light empty room no people` |
| Foi et pratique | `assets/img/foi-pratique-hero-480.webp`, `foi-pratique-hero-960.webp`, `foi-pratique-hero-1600.webp` | 480×300, 960×600, 1600×1000 | 45 / 110 / 240 Ko | Architecture, lumière de l'aube, tapis uni dans un espace vide sans personne ni représentation. | `empty mosque interior dawn sunlight geometric architecture no people` |

## 3. Miniatures de la vidéothèque

| Ensemble | Fichiers attendus | Dimensions | Poids max par fichier | Idée visuelle | Requête Pexels / Pixabay |
|---|---|---|---:|---|---|
| Coran | `assets/img/video-coran-480.webp`, `video-coran-960.webp`, `video-coran-1600.webp` | 480×300, 960×600, 1600×1000 | 45 / 110 / 240 Ko | Détail d'une page, lumière oblique et motif discret. | `Quran pages closeup warm light macro no people` |
| Sunna | `assets/img/video-sunna-480.webp`, `video-sunna-960.webp`, `video-sunna-1600.webp` | 480×300, 960×600, 1600×1000 | 45 / 110 / 240 Ko | Arche, bibliothèque ou manuscrit ancien. | `islamic library manuscript architecture detail no people` |
| Foi | `assets/img/video-foi-480.webp`, `video-foi-960.webp`, `video-foi-1600.webp` | 480×300, 960×600, 1600×1000 | 45 / 110 / 240 Ko | Ciel d'aube, lumière sur une architecture, tons verts et ivoire. | `mosque dome dawn sky light architecture no people` |

Les miniatures sont des images locales. Leur mouvement au survol est une animation CSS légère (zoom lent et voile), pas un téléchargement vidéo supplémentaire.

## 4. Conversion vidéo avec FFmpeg

Préparer une source de bonne qualité dans `sources/hero-master.mp4`. Installer FFmpeg si nécessaire :

```bash
sudo apt update
sudo apt install ffmpeg
mkdir -p sources assets/video assets/img
```

### WebM VP9 — première passe

```bash
ffmpeg -y -i sources/hero-master.mp4 -t 7 -an \
  -vf "fps=24,scale='min(1280,iw)':-2:force_divisible_by=2,format=yuv420p" \
  -c:v libvpx-vp9 -b:v 500k -maxrate 550k -bufsize 1000k \
  -row-mt 1 -deadline good -cpu-used 4 \
  -pass 1 -passlogfile /tmp/qalb-hero-vp9 -f webm /dev/null
```

### WebM VP9 — seconde passe

```bash
ffmpeg -y -i sources/hero-master.mp4 -t 7 -an \
  -vf "fps=24,scale='min(1280,iw)':-2:force_divisible_by=2,format=yuv420p" \
  -c:v libvpx-vp9 -b:v 500k -maxrate 550k -bufsize 1000k \
  -row-mt 1 -deadline good -cpu-used 4 \
  -pass 2 -passlogfile /tmp/qalb-hero-vp9 assets/video/hero.webm
```

### MP4 H.264

```bash
ffmpeg -y -i sources/hero-master.mp4 -t 7 -an \
  -vf "fps=24,scale='min(1280,iw)':-2:force_divisible_by=2,format=yuv420p" \
  -c:v libx264 -preset slow -crf 30 -maxrate 650k -bufsize 1300k \
  -movflags +faststart assets/video/hero.mp4
```

### Extraire le poster WebP

```bash
ffmpeg -y -ss 00:00:01.5 -i sources/hero-master.mp4 -frames:v 1 \
  -vf "scale=1280:720:force_original_aspect_ratio=increase,crop=1280:720" \
  -an -c:v libwebp -quality 78 -compression_level 6 assets/img/hero-poster.webp
```

### Vérifier la durée, la définition et le budget vidéo

```bash
ffprobe -v error -show_entries format=duration,size \
  -show_entries stream=codec_name,width,height,r_frame_rate \
  -of default=noprint_wrappers=1 assets/video/hero.webm
stat -c '%n : %s octets' assets/video/hero.webm assets/video/hero.mp4
du -ch assets/video/hero.webm assets/video/hero.mp4 | tail -n 1
```

Si le total dépasse 1,5 Mo, réduire progressivement `-b:v` et `-maxrate` (par exemple 450k pour WebM et 550k pour MP4), ou réduire la durée à 5 secondes. `-an` supprime toute piste audio ; `-t 7` limite la séquence à 7 secondes ; `fps=24` limite le nombre d'images ; `scale` limite la largeur à 1280 px sans agrandir les sources ; `yuv420p` améliore la compatibilité ; VP9/H.264 définissent les codecs ; les débits plafonnent le poids ; `-pass 1/2` répartit l'encodage VP9 en deux passes ; `+faststart` place l'index MP4 au début du fichier pour un démarrage progressif.

## 5. Conversion des photos en WebP

Créer les originaux dans `sources/` avec un cadrage 16:10 et les noms suivants : `home-editorial.jpg`, `coran-hero.jpg`, `sunna-hero.jpg`, `foi-pratique-hero.jpg`, `video-coran.jpg`, `video-sunna.jpg`, `video-foi.jpg`.

Lancer cette commande depuis la racine du dépôt :

```bash
for name in home-editorial coran-hero sunna-hero foi-pratique-hero video-coran video-sunna video-foi; do
  for width in 480 960 1600; do
    height=$((width * 10 / 16))
    ffmpeg -hide_banner -loglevel error -y -i "sources/${name}.jpg" \
      -vf "scale=${width}:${height}:force_original_aspect_ratio=increase,crop=${width}:${height}" \
      -frames:v 1 -an -c:v libwebp -quality 78 -compression_level 6 -preset photo \
      "assets/img/${name}-${width}.webp"
  done
done
```

- `scale` redimensionne à la largeur demandée en conservant assez de matière pour remplir le cadre.
- `crop` recadre proprement au ratio 16:10, identique aux attributs `width` et `height` du HTML.
- `-frames:v 1` garantit une image fixe ; `-an` retire toute piste audio éventuelle.
- `libwebp` encode en WebP ; `quality 78` équilibre netteté et poids ; `compression_level 6` privilégie une compression plus poussée ; `preset photo` est adapté aux photographies.

Contrôler les poids après conversion :

```bash
find assets/img -maxdepth 1 -name '*.webp' -printf '%f %k Ko\n' | sort
```

Si une image dépasse le budget indiqué, baisser `-quality 78` vers 72 ou recadrer une source moins détaillée. Ne pas remplacer les images par des URL externes.

## 6. Activation et comportement

- Le HTML contient déjà les chemins WebP dans `data-webp-srcset`. Le JS teste les fichiers locaux ; tant qu'ils ne sont pas déposés, le placeholder SVG reste affiché.
- Les trois tailles sont choisies par `srcset` et `sizes`. Les dimensions intrinsèques sont indiquées pour éviter les sauts de mise en page.
- Les images principales thématiques sont prioritaires ; les autres images sont différées avec `loading="lazy"`.
- La vidéo n'est jamais chargée avec `prefers-reduced-motion` ou `saveData`, et son absence ne casse pas le hero.
- Les SVG placeholders sont temporaires et peuvent être supprimés après vérification des vrais médias.
