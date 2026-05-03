# Carte mentale — P15 (portfolio AI Engineer)

_Date : 03 mai 2026_

> Objectif : formaliser une **carte mentale / carte conceptuelle** (Étape 2 du P15) qui regroupe :
> - compétences (techniques + “delivery”),
> - réflexivité (axes d’amélioration),
> - soft skills,
> - et mise en valeur du projet personnel (P8).
>
> Format : ici, une représentation textuelle structurée (hiérarchique), lisible et versionnée.

## Centre — Portfolio AI Engineer (P15)

### A) Projet personnel (dominante technique) — P8 Credit Scoring API

- Problème
  - Exposer un modèle de scoring crédit via une API exploitable
  - Produire une décision `ACCEPTED/REFUSED` à partir d’un seuil
- Solution (POC)
  - API FastAPI : `GET /health`, `POST /predict`
  - Modèle : pipeline LightGBM (joblib) chargé au démarrage
  - Déploiement : Docker → Hugging Face Spaces
- Industrialisation
  - Tests : Pytest
  - CI/CD : GitHub Actions (tests → build → déploiement)
  - Observabilité : logs JSONL + monitoring latence/drift
  - Performance : profilage et optimisation des goulots
- Risques / limites (POC)
  - Sécurité : pas d’auth, pas de rate-limit
  - Gouvernance data : provenance/licences, fairness (non documenté)
  - Observabilité “prod” : pas de dashboard / alerting

### B) Compétences techniques (transverses)

- API & backend
  - FastAPI, Pydantic, endpoints, validation, erreurs
- Packaging / déploiement
  - Docker, (Compose selon projets), release reproductible
- Qualité logicielle
  - Tests, CI, robustesse, gestion d’erreurs
- Observabilité
  - Logging structuré, métriques latence, drift, rapports
- Data / retrieval
  - RAG : embeddings, FAISS, chunking, réponses sourcées
- LLM engineering (selon projets)
  - SFT/LoRA/DPO, garde-fous, audit

### C) Compétences “delivery” / conduite de projet

- Cadrage POC
  - périmètre, critères de succès, Go/No-Go
- Décision
  - arbitrages coût/latence/qualité
  - posture “POC vs prod” (écarts, trajectoire)
- Communication
  - documentation claire, démo, transparence des limites

### D) Soft skills mises en avant

- Rigueur et sens du détail (tests, logs, métriques)
- Autonomie (end-to-end : dev → CI → déploiement)
- Analyse et esprit critique (drift, limites, plan d’action)
- Communication (rapports, synthèses, présentation)

### E) Réflexivité (axes d’amélioration)

- Sécurité
  - auth, rate-limit, secrets management, durcissement
- Gouvernance data
  - data contracts, qualité, licences, anonymisation
- Observabilité
  - dashboards, alerting, runbooks, SLO
- Évaluation
  - jeux de tests métier (gold set), suivi qualité dans le temps
- Cycle de vie modèle
  - triggers drift → retrain → validation → promotion

## Liens

- Portfolio : `docs/index.html`
- Rapport P15 (projet perso) : `docs/rapports/rapport_P15_projet_perso_credit_scoring.html`
- Rapport P8 : `docs/rapports/rapport_P8_credit_scoring.html`
