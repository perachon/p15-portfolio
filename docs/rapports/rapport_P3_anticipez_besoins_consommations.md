# Rapport — P3 Anticipez les besoins en consommations de bâtiments (Seattle)

## 1. Contexte et objectif

- Contexte : mission pour la ville de **Seattle** visant la neutralité carbone à horizon 2050.
- Problème : les relevés de consommation/émissions sont coûteux ; ils existent pour une campagne de mesures (2016) sur des bâtiments non résidentiels.
- Objectif : prédire **la consommation totale d’énergie** et **les émissions de CO2** pour des bâtiments non destinés à l’habitation non mesurés, à partir de variables structurelles (taille, usage, année de construction, localisation…).

## 2. Démarche (guidée)

### 2.1 Analyse exploratoire

- Comprendre le contenu des colonnes, identifier bâtiments aberrants et valeurs incohérentes.
- Conserver une trace des volumes avant/après filtrage.
- Éviter de supprimer massivement les lignes contenant des valeurs manquantes.

### 2.2 Feature engineering

- Créer des variables pertinentes (localisation, temporalité, structure bâtiment…).
- Gérer les catégorielles (OneHotEncoder vs LabelEncoder selon le cas).
- Éviter le **data leakage** (ne pas utiliser des variables “énergie” qui dépendent de la target).

### 2.3 Préparation pour la modélisation

- Scaling/normalisation selon les algorithmes.
- Gestion des outliers (IQR/z-score ou quantiles si besoin).
- Choix d’une target (parmi les colonnes candidates) pour le reste du projet.

### 2.4 Comparaison de modèles

- Séparation train/test, validation croisée.
- Évaluation via métriques de régression (exemples cités : R2, MAE).
- Fixer un `random_state` pour la reproductibilité.

### 2.5 Optimisation et interprétation

- Optimisation via GridSearchCV (grille raisonnable, exécution longue).
- Interprétation : feature importance (après optimisation).

## 3. Livrables attendus (extraits)

- Notebook template complété : EDA → feature engineering → préparation → comparaison modèles → optimisation/interprétation.
- Synthèse des résultats : insights EDA + modèles testés + facteurs principaux impactant le modèle retenu.

---

Source : `projets/p3_anticipez_besoins_consommations/p3_contexte_mission.md`.
