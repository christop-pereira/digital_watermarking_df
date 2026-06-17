# Évaluation de la robustesse et de la détectabilité des watermarks numériques

**Auteur :** PEREIRA TOMAS Christopher
**Date :** 29/10/2025

## Objectif

Ce mini-projet étudie la robustesse de watermarks (filigranes numériques) invisibles face à différentes transformations d'image, ainsi que leur détectabilité selon les conditions. L'objectif est de mettre en évidence le compromis entre **imperceptibilité** (le watermark ne doit pas être visible) et **résilience** (le watermark doit survivre aux modifications de l'image).

## Méthodes testées

Le projet s'appuie sur la bibliothèque [invisible-watermark](https://github.com/ShieldMnt/invisible-watermark) et compare trois techniques d'insertion de watermark :

- **DCT + DWT** — combinaison de transformée en cosinus discrète et transformée en ondelettes discrète
- **DCT + DWT + SVD** — ajout d'une décomposition en valeurs singulières
- **RivaGAN** — méthode basée sur un réseau de neurones génératif adversarial

## Transformations / attaques appliquées

Chaque image watermarkée est soumise aux dégradations suivantes afin de tester la robustesse du watermark :

- Compression JPEG (qualités 90, 70, 50)
- Ajout de bruit gaussien (σ = 5, 10, 20)
- Modification de luminosité / contraste
- Flou gaussien
- Mise à l'échelle (scaling)
- Rotation
- Conversion en niveaux de gris

## Métriques utilisées

**Imperceptibilité (qualité visuelle) :**
- PSNR (Peak Signal-to-Noise Ratio)
- SSIM (Structural Similarity Index)

**Robustesse / détectabilité :**
- BER (Bit Error Rate) — taux d'erreur binaire entre le message original et le message extrait
- NCC (Normalized Cross-Correlation) — corrélation croisée normalisée
- Score de confiance de détection (pour RivaGAN)

## Structure du notebook

1. Exemples d'encodage / décodage de watermark (CLI et API Python)
2. Génération des versions dégradées des images watermarkées pour chaque méthode (DCT+DWT, DCT+DWT+SVD, RivaGAN)
3. Calcul du PSNR et du SSIM entre image originale et image watermarkée
4. Extraction du message depuis chaque image dégradée et calcul du BER / NCC
5. Synthèse des résultats sous forme de tableau récapitulatif

## Résultats : résumé

Les tests montrent que :
- La méthode **DCT+DWT+SVD** est globalement la plus robuste, le message restant lisible après la plupart des attaques (compression, bruit léger, flou).
- La méthode **DCT+DWT** seule est la plus fragile : le message est souvent totalement perdu dès que l'image est compressée, bruitée ou redimensionnée.
- **RivaGAN** tolère mieux certaines attaques géométriques (rotation, redimensionnement) mais introduit davantage d'erreurs bit à bit même sur l'image de base.
- Les transformations géométriques (rotation, scaling) et la forte compression (JPEG 50) sont les attaques les plus destructrices pour l'ensemble des méthodes.

## Prérequis

```bash
pip install invisible-watermark opencv-python numpy torch scikit-image
```

## Utilisation

1. Cloner le dépôt [invisible-watermark](https://github.com/ShieldMnt/invisible-watermark) (les images de test utilisées proviennent de `test_vectors/`).
2. Ouvrir le notebook `PEREIRA_TOMAS_Christopher_mini-project.ipynb` dans Jupyter.
3. Exécuter les cellules dans l'ordre pour reproduire l'embedding, les attaques et le calcul des métriques.

## Référence

- Dépôt de référence : [ShieldMnt/invisible-watermark](https://github.com/ShieldMnt/invisible-watermark)