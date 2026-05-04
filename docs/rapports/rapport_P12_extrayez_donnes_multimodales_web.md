# Rapport — P12 Extrayez des données multimodales de sites web (pipeline fake news)

## 1. Contexte et objectif

- Organisation (scénario) : **CheckIt.AI**, start-up de détection automatique de désinformation.
- Besoin : construire un pipeline d’acquisition de données **multimodales (texte + image)** pour enrichir un moteur d’analyse “fake news”.
- Objectif : automatiser le parcours de la donnée de l’**extraction** au **stockage**, avec **maintenabilité** et **reproductibilité**.

## 2. Stratégie d’extraction (extraits)

- Identifier au moins **3 sources multimodales** (API / sites / datasets / RSS…)
- Pour chaque source : préciser le type de données (texte, image), le format (CSV/JSON/API…), la langue, la qualité des labels (vrai/faux) et la méthode d’extraction (API REST, Scrapy, Selenium…).
- Points d’attention :
  - Vérifier les droits d’usage et la légalité du scraping.
  - Ne pas confondre opinions controversées et désinformation.
  - Vérifier l’association texte‑image dans la même entrée.
  - Prendre en compte les quotas/clefs des APIs.

## 3. Scripts d’extraction automatisée (extraits)

- Livrables : scripts Python (ou notebook) **modulaires**, exécutables sans intervention manuelle.
- Stockage : structure cohérente (ex. JSON, CSV ; formats évoqués : parquet possible).
- Bonnes pratiques attendues :
  - Code structuré en fonctions (connexion, parsing, nettoyage, sauvegarde).
  - Logs et gestion d’erreurs (try/except).
  - Paramètres configurables.

## 4. Transformation et schéma de données (extraits)

- Pipeline modulaire : lecture → traitement → export.
- Transformations attendues : nettoyage, normalisation, mapping, génération de colonnes.
- Exigence : pipeline reproductible et journalisé.
- Schéma conceptuel (PDF ou Mermaid) : champs, types et rôle (ex. titre, contenu, URL image, métadonnées).

## 5. Orchestration ETL avec Apache Airflow (extraits)

- Construire un DAG Airflow pour orchestrer extraction → transformation → chargement.
- Tâches distinctes, exécution locale, logs/captures comme preuves d’exécution.
- Recommandation : réutiliser les fonctions du pipeline de transformation dans des `PythonOperator` simples.

## 6. Évaluation et monitoring (extraits)

- Définir des KPI (qualité des données, rapidité, coût).
- Tableau de bord de visualisation + plan de monitoring pour la production.

---

Cas d’étude OpenClassrooms — synthèse du projet.
