# Rapport — P2 Requêtez des services IA (ModeTrends)

## 1. Contexte et objectif

- Organisation (scénario) : **ModeTrends**, agence de conseil en marketing digital dans la mode.
- Projet global : « Fashion Trend Intelligence » (analyse automatisée de tendances sur les réseaux sociaux).
- Votre périmètre : **segmentation vestimentaire** — identifier et isoler chaque pièce vestimentaire dans une image.

## 2. Approche

- Modèle : **SegFormer** (segmentation sémantique basée Transformer) ; encoder MiT + decoder léger.
- Deux modes d’exécution évalués :
  - Traitement local via `transformers` (sans requête API Hugging Face).
  - Appel distant via Hugging Face Inference API (serverless).

## 3. Méthode de validation

- Métrique : **IoU (Intersection over Union)**.
- Principe : comparer masques prédits vs masques « ground truth » (par classe et global).

Résultat (extrait) :

- IoU global ≈ **74%** (bonne qualité globale, classes fines plus difficiles).

## 4. Coût (extrait)

- Local : 0€ (hors coût machine/disque).
- Hugging Face Inference API : traitement à distance (~1s/image) ; plan Basic à 0.10$/minute au-delà des 30 premières minutes ; estimation donnée ≈ **800$** (≈ 140h de calcul) pour le volume évoqué.

## 5. Défis rencontrés et solutions (extraits)

- Alignement masques prédits / réels : attention au nommage (ex. `image_0.png` ↔ `mask_0.png`).
- Pré-traitement images : éviter la déformation (redimensionnement + padding).
- Chargement formats : gestion dynamique des extensions (`.png`, `.jpg`, …) via `.env`.
- Évaluation : nécessité d’une comparaison systématique aux masques ground truth.

## 6. Pistes d’amélioration (extraits)

- Filtrage par classe : ignorer classes rares/faible confiance (ex. ceinture).
- Lisibilité : overlay, contours, légendes dynamiques.

## 7. Applications potentielles (extraits)

- Essayage virtuel / segmentation automatique.
- Recherche visuelle (retrouver un vêtement en photo).
- Personnalisation marketing.
- Analyse de tendances (agrégation à grande échelle).

---

Cas d’étude OpenClassrooms — synthèse du projet.
