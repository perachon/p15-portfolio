# Rapport — P4 Attrition (TechNova Partners)

## 1. Contexte et analyse des besoins

### 1.1 Contexte

- Contexte (scénario) : mission de **HR Analytics** pour l’ESN **TechNova Partners** (turnover plus élevé que d’habitude).
- Demande : identifier des **causes potentielles** de démission et proposer des **leviers actionnables**.
- Approche : analyse exploratoire + modèle de **classification supervisée** pour scorer une probabilité de départ, puis analyse des facteurs clés.

### 1.2 Besoin métier

- Besoin : objectiver les différences entre les employés qui restent vs ceux qui partent.
- Besoin : produire un score de probabilité de départ par employé pour aider à prioriser des actions RH.

## 2. Données (3 sources)

Les données proviennent de trois fichiers (sources différentes) :

- SI RH (informations sociodémographiques / poste / âge / salaire / ancienneté, etc.).
- Évaluations annuelles de performance (notes d’évaluation et de satisfaction).
- Sondage annuel (bien‑être / participation / promotions, etc.) incluant un témoin de départ.

Objectif : regrouper les données en s’assurant d’un **recouvrement** suffisant (correspondance des employés entre fichiers) pour permettre les jointures.

## 3. Préparation et qualité des données

Étapes réalisées (extraits) :

- Nettoyage et préparation.
- Suppression des colonnes non pertinentes (IDs, colonnes constantes).
- Encodage des variables catégorielles (One‑Hot Encoding).
- Gestion de corrélations fortes.

Volumétrie (extraits) :

- 1470 employés.
- 1233 restants.
- 237 démissionnés (≈ 16%).

Découpage : train/test avec **stratification** pour conserver la proportion des classes.

## 4. Modélisation et choix du modèle

### 4.1 Baselines et essais

- Dummy Classifier (baseline) : accuracy ≈ 84% mais incapacité à prédire les départs (classe minoritaire).
- Régression logistique : résultats corrects, recall ≈ 0.68 sur la classe « départ » ; compromis adapté au besoin.
- Random Forest (brut) : surapprentissage (overfitting).

### 4.2 Validation et compromis métier

- Validation croisée : mesurer performance moyenne et stabilité, détecter l’overfit, comparer les modèles.
- Conclusion (extrait) : **modèle retenu = régression logistique**.

Justification métier (extrait) : mieux vaut détecter davantage de départs (recall) quitte à quelques fausses alertes.

### 4.3 Seuil de décision (extraits)

Le seuil de décision permet d’arbitrer précision vs rappel.

Exemples (extraits) :

- Seuil 0.20 (très bas) : recall(1)=0.91 ; precision(1)=0.22.
- Seuil 0.50 (par défaut) : recall(1)=0.68 ; precision(1)=0.38.
- Seuil 0.90 (très haut) : precision(1)=1.00 ; recall(1)=0.17.

Positionnement (extrait) : « mieux vaut alerter trop que rater des départs » → favoriser légèrement le recall.

## 5. Interprétation (extraits)

### 5.1 Importance des variables (logistique)

- Facteurs aggravants (extrait) : surcharge de travail (heures sup, déplacements fréquents), postes exposés (consultant, commercial), célibat.
- Facteurs protecteurs (extrait) : satisfaction dans le travail, implication dans des dispositifs financiers (PEE), postes à responsabilité (manager, direction).

Note (extrait) : le revenu mensuel a un coefficient proche de 0 une fois les autres variables prises en compte (redondance / corrélations possibles).

### 5.2 Analyse des erreurs (extraits)

- Faux négatifs (départs ratés) : profils plus âgés, expérimentés, bien payés, installés.
- Faux positifs (fausses alertes) : profils plus jeunes, en mobilité (déplacements, heures sup), qui restent finalement.

Conclusion (extrait) : le modèle repère surtout les signes visibles de surcharge, mais capte moins les causes « discrètes » liées à la carrière.

## 6. Livrables (attendus de mission)

- Un fichier `pyproject.toml` (versions Python + dépendances).
- Notebooks et/ou scripts : nettoyage, exploration et modélisation.
- Un support de synthèse du travail (slides).

---

Cas d’étude OpenClassrooms — synthèse du projet.
