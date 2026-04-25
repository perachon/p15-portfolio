# Rapport — P10 Labellisez et appliquez des approches semi-supervisées en traitement d'images (BrainScanAI)

## 1. Contexte et objectif

- Organisation (scénario) : **CurelyticsIA**, startup e-santé.
- Cas d’usage R&D : exploration d’une détection automatisée **tumeur vs normal** sur des images IRM.
- Contrainte clé : très peu d’images labellisées (annotation coûteuse) et un grand volume non labellisé.
- Enjeu méthodologique : **éviter la fuite de données** (test strict, décisions prises sur train/val).

> Note : l’objectif présenté est une **classification binaire** (démonstration technique). Les sources précisent que ce n’est pas un diagnostic médical complet.

## 2. Données (extraits)

- Total : **1506 images**
- Labellisées : **100** (50 normal / 50 cancer)
- Non labellisées : **1406**
- Split : train/val/test sur le sous-ensemble labellisé, avec **test strict** réservé à l’évaluation finale.

## 3. Pipeline (vue d’ensemble)

1) Exploration & contrôle qualité (QC)
- Statistiques simples par image (taille, intensité, dispersion, percentiles).
- Détection d’images atypiques (outliers) et de quasi-doublons via la similarité dans l’espace des embeddings.
- Règle de sécurité : ne pas supprimer automatiquement les images labellisées.

2) Prétraitement + extraction de features (embeddings)
- Prétraitement : conversion en **RGB (3 canaux)**, redimensionnement **224×224**, normalisation **ImageNet**.
- Extraction : **ResNet50 pré-entraîné** utilisé en extracteur figé.
- Représentation : **1 vecteur de 2048 valeurs par image** (matrice 1506×2048).

3) Analyse non supervisée : clustering
- Méthodes testées : **KMeans** (principal) + **DBSCAN** (comparaison).
- Objectif : explorer la structure de l’espace des embeddings.
- Évaluation sans fuite : mesure du signal sur le labellisé train/val via **ARI (Adjusted Rand Index)**.

4) Semi-supervisé : pseudo-labels puis entraînement
- Mapping cluster → classe appris uniquement sur le train labellisé.
- Génération de pseudo-labels sur le non labellisé.
- Réduction du bruit : filtrage de confiance (conserver les pseudo-labels “les plus clairs”).
- Modèles comparés :
  - Baseline supervisée : CNN entraîné uniquement sur le labellisé.
  - Semi-supervisé : pré-entraînement sur pseudo-labels puis fine-tuning sur vrais labels.
- Robustesse : répétitions multi-seeds (moyenne / écart-type).

## 4. Résultats (extraits)

- Métriques annoncées : **F1 macro** + matrice de confusion (accuracy en complément).
- Pseudo-labels : pseudo-labellisation de 1406 images non annotées puis filtrage (≈ **un quart** gardé, autour de **350** images).
- Test final (extraits) :
  - Supervisé : **~95% accuracy**, **F1 macro ~0.95**.
  - Semi-supervisé : **~90% accuracy**, **F1 macro ~0.90**.
- Message clé présenté : performances **comparables** dans l’exercice, et **stabilité accrue** (variance plus faible) en semi-supervisé filtré sur plusieurs seeds.

## 5. Contraintes budget / passage à l’échelle (extraits)

- Budget courant mentionné pour le dataset : **300 €**.
- Question “passage à l’échelle” : **5 000 €** pour **4 millions d’images** à labelliser.
- Recommandation : pipeline batch relançable, traçabilité (index, versions, métriques/artefacts) et monitoring simple (dérive + QC).

## 6. Points d’attention et suites possibles (extraits)

- Risques : biais, outliers/doublons, fuite de données.
- Suites proposées : normalisation plus spécifique IRM, calibration, validation externe, annotation progressive (active learning).

---

Sources :
- `projets/p10_labellisez_semi_supervisees_traitement_images/p10_contexte_mission.md`
- `projets/p10_labellisez_semi_supervisees_traitement_images/p10_diapo.md`
- `projets/p10_labellisez_semi_supervisees_traitement_images/p10_questions.md`
