# Rapport — P5 Déploiement d’un modèle ML (Futurisys)

## 1. Contexte et analyse des besoins

- Contexte (scénario) : mission freelance pour **Futurisys** (demande du directeur technique Aurélien) afin de rendre un modèle ML **opérationnel** et accessible via une **API performante**.
- Objectif : mettre en place une solution de déploiement prête pour un usage “type production” (POC), incluant versioning, tests, CI/CD, et traçabilité.

## 2. Périmètre et livrables

Livrables attendus (extraits) :

- Dépôt Git structuré : code, `requirements.txt`, historique de commits, branches, tags/releases.
- API FastAPI (ou équivalent) : endpoints documentés via Swagger/OpenAPI.
- Tests unitaires/fonctionnels : suite Pytest + rapport de couverture (pytest-cov).
- Base de données PostgreSQL : tables, script de création (SQL/Python), exemples d’entrées, scripts d’interaction.
- Pipeline CI/CD : automatisation des tests et du déploiement + gestion des secrets.

## 3. Solution (POC) et architecture

### 3.1 Stack (extrait)

- Python 3.9
- API : FastAPI
- Validation : Pydantic (validation stricte)
- DB : PostgreSQL + SQLAlchemy (avec fallback SQLite selon environnement)
- CI/CD : GitHub Actions
- Déploiement : Hugging Face Spaces (SDK Docker)
- Tests : Pytest + couverture (~85% annoncé)

### 3.2 Endpoints et documentation

- `/docs` : Swagger UI auto-généré
- Endpoints (extraits) :
  - `GET /health` : check API + base de données
  - `POST /predict` : endpoint “simple” (test chaîne CI/CD)
  - `POST /p4/predict` : exposition du modèle d’attrition (artefacts issus du projet 4)

### 3.3 Modèle et artefacts

- Intégration via des **artefacts** (séparation code / modèle) :
  - un fichier `.joblib` contenant le pipeline
  - un `input_schema.json` décrivant la structure attendue (ordre des features)
- Chargement au démarrage : l’API charge les artefacts et garantit l’alignement entre requête et modèle.

### 3.4 Traçabilité et base de données

- Tables (extraits) :
  - `employees` : données issues du dataset RH
  - `prediction_logs` : journalisation des requêtes/réponses et métadonnées (version modèle, latence, timestamp…)
- Objectif : **traçabilité** complète et reproductibilité des prédictions.

## 4. CI/CD, versioning et qualité

### 4.1 Workflow Git (extraits)

- Trunk-based development : branche principale `main` + branches courtes `feat/...`
- Commits atomiques
- PR avec CI obligatoire
- Tags/Releases pour marquer les versions (exemples donnés : `v0.3.0`, `v0.4.0`, `v0.5.0`)

### 4.2 CI/CD (extraits)

- Installation dépendances
- Exécution Pytest
- Calcul couverture
- Déploiement (si tests OK) vers Hugging Face
- Gestion des secrets : `HF_TOKEN` stocké dans GitHub Secrets

## 5. Tests & couverture (extraits)

Stratégie de tests (extraits) :

- Unitaires : validation Pydantic (types stricts, champs manquants → 422) + loader artefacts (fichiers manquants, clé `feature_order`…)
- Fonctionnels : tests endpoints (200 / 400 / 422 / 503) avec mocks ciblés
- Intégration : API ↔ DB, vérification qu’un appel crée une nouvelle ligne dans `prediction_logs`

Couverture : ~85% (extrait).

## 6. Limites & next steps (extraits)

- Ajouter des tests de performance (latence max)
- Tests de non-régression du modèle sur jeu de référence versionné
- Évolutions possibles : endpoint `/logs/last`, export logs vers dashboard, migration DB managée (ex. Supabase)

## 7. Liens

- Démo Hugging Face Spaces (extrait) : https://huggingface.co/spaces/perachon/p5-ml-deploy-poc-dev

---

Sources : `projets/p5_deploiement_modele_ml/p5_contexte_mission.md` et `projets/p5_deploiement_modele_ml/p5_diapo.md`.
