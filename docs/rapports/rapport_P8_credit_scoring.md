# Rapport de conduite de projet AI Engineering — P8 (Credit Scoring API)

_Date : 22 avril 2026_

> Ce document est un **brouillon pré-rempli** basé sur le template OpenClassrooms.
>
> Règle : **ne pas inventer**. Si une info manque, laisser `TBD`.

## 1. Contexte et analyse des besoins

### 1.1 Présentation (organisation et / ou contexte)

- Contexte : système d’aide à la décision pour l’**octroi de crédit** via une API.
- Type d’organisation (scénario OpenClassrooms) : société financière fictive **"Prêt à Dépenser"**, avec un département **"Crédit Express"** qui souhaite traiter des demandes en quasi temps réel.
- Enjeux business : réduire le coût du risque tout en maintenant un taux d’acceptation cible.
- Pourquoi “industrialiser” : un modèle performant en notebook n’a de valeur métier que s’il est **fiable, traçable, monitoré et maintenable** en production.
- Niveau de maturité IA / MLOps : **maturité intermédiaire** — déploiement Docker + CI/tests + logs + monitoring ; pas encore de sécurité/alerting/retraining automatisé.

Contraintes (à adapter) :
- Sécurité : API exposée publiquement en démo, **pas d’authentification** implémentée (POC).
- Données : données financières/socio-démo potentiellement sensibles (RGPD/fairness à cadrer).
- Infra : Docker + déploiement sur Hugging Face Spaces (container).
- Latence : instrumentée et analysée (logs).

### 1.2 Collecte et analyse du besoin métier

- Besoin : estimer $P(\text{défaut})$ et produire une décision métier `ACCEPTED/REFUSED` à partir d’un seuil `THRESHOLD=0.2`.
- Utilisateurs / parties prenantes (scénario OpenClassrooms) :
  - Lead Data Scientist : **Chloé Dubois** (demande Slack : API Docker-ready + suivi en production).
  - Manager (Partie 1) : **Michaël** (attentes MLOps : MLflow tracking/UI/registry/serving + cadrage coût FP/FN).
  - Métier : département **Crédit Express** + chargé d’études (utilisation du score pour décider).
  - IT/Prod : équipe de déploiement (Docker/CI/CD).
  - Conformité : **non cadré juridiquement dans le POC** (pas de DPIA, pas de politique formalisée d’accès/rétention). POC exposé publiquement et sans auth/rate-limit → points de vigilance à traiter en prod.
- Recueil du besoin (scénario OpenClassrooms) : message Slack (priorité “API d’ici fin de semaine prochaine” + “dashboard/rapport de suivi”) + consignes écrites (email) sur les objectifs MLOps et les métriques business/techniques.

Objectifs :
- Objectif business (scénario OC) : permettre au département Crédit Express de traiter les nouvelles demandes en quasi temps réel, tout en maîtrisant le risque (coût FP/FN, taux de défaut, taux d’acceptation).
- Objectifs techniques :
  - API stable (`/health`, `/predict`) et reproductible.
  - Traçabilité : logs structurés de requêtes et de prédictions.
  - Monitoring : latence + détection de dérive.
  - Optimisation : analyser et réduire les latences anormales (ex : cold start).

Axes de réalisation (structuration du projet) :
- Exposer le modèle via une API stable.
- Automatiser les tests et le déploiement.
- Mettre en place du monitoring.
- Analyser et optimiser les performances.

Hiérarchisation (exemple) :
- Must-have : prédiction + décision + déploiement + logs.
- Should-have : monitoring drift/latence + runbook.
- Could-have : auth, rate-limit, retraining automatisé.

## 2. Audit de la solution data existante (ou proposée si absence de solution existante)

### 2.1 Solution actuelle ou proposée

Solution actuelle (POC) :
- API FastAPI avec endpoints : `GET /health`, `POST /predict`.
- Validation d’entrée : Pydantic.
- Modèle : **chargé au démarrage (startup)** et conservé en mémoire, artefact joblib téléchargé depuis Hugging Face Hub (`perachon/credit-scoring-model`, fichier `lightgbm_api_pipeline.pkl`) avec cache Hugging Face.
- Déploiement : Docker, Uvicorn écoute en container sur port `7860`.
- CI/CD : GitHub Actions (pytest → build Docker → push HF Space).
- Monitoring :
  - Logs JSONL (latency_ms, status_code, request_id, events prediction/validation_error).
  - Analyses : baseline latence + drift (Evidently, KS).

Démo : https://perachon-credit-scoring-api-v2.hf.space/docs

Note de conception :
- Logging vs monitoring : le logging stocke les événements ; le monitoring analyse ces événements pour détecter des anomalies.

### 2.2 Évaluation de l’adéquation aux besoins

Critères d’analyse (et état dans le POC) :
- Performance ML : comparatif multi-modèles avec métriques CV (AUC/Recall/F1) disponible (artefact MLflow) ; AUC autour de ~0.77 selon modèles.
- Robustesse API : validation Pydantic + tests automatisés (`pytest`).
- Sécurité : manque d’auth/rate-limit (écart).
- Coût : **estimation non réalisée (POC)**. Drivers principaux : compute CPU (API + inférence), cold start/téléchargement du modèle depuis le Hub, stockage/rotation des logs, hébergement (Hugging Face Spaces). Chiffrage (€/mois) : `TBD` (dépend du plan et du trafic).
- Maintenance : CI/CD présent ; version applicative `v1` ; dépendances séparées API/dev.
- Monitoring : latence + drift présents ; plan d’action (drift) explicité en 5.1 (analyse cause → suivi KPI → éventuel retraining/recalibration → revalidation → redéploiement CI/CD).

Écarts / limites à documenter :
- Justification business du seuil 0.2 (coût FP/FN) : dans la mission OpenClassrooms, on peut **supposer** qu’un faux négatif (mauvais client prédit bon) coûte **10×** un faux positif (bon client refusé). Le seuil doit donc être **optimisé** sur un score métier (coût d’erreur) plutôt que fixé à 0.5.
- Gouvernance data : **non formalisée dans le POC** (pas de data contract, pas de contrôles qualité systématiques, pas d’analyse fairness documentée). Schéma d’entrée défini via Pydantic ; vigilance sur les variables sensibles potentielles (ex. genre) et les biais. Droits/licence exacts de la source du dataset : `TBD` si non documenté.
- Observabilité (dashboard, alerting) : partiel.

## 3. Identification d’une solution technique cible

Comparatif d’approches (déjà initié) :
- Modèles testés : LogisticRegression, RandomForest, GradientBoosting, XGBoost, LightGBM.
- Résumé du comparatif (CV) :
  - AUC mean max ~0.769 (XGBoost/GBM/LightGBM proches)
  - Recall mean : LightGBM ~0.67, LogisticRegression ~0.69, RF ~0.64
  - F1 mean : LightGBM ~0.282 (meilleur du tableau fourni)

Décision technique (POC) :
- Modèle servi : pipeline LightGBM dans un artefact joblib.
- Serving : FastAPI + Docker.
- Versioning : MLflow runs + modèle sur Hugging Face Hub.

Schéma d’architecture cible (à dessiner) :
- Client → API Gateway (option) → FastAPI → validation → pipeline → décision seuil → réponse
- Logs → stockage (JSONL / centralisation) → dashboard latence/drift → alerting

## 4. Stratégie de mise en œuvre et d’industrialisation

### 4.1 Proposition de démarche projet

Roadmap proposée (exemple, à ajuster selon timing) :
1) Cadrage KPI & seuil (coûts FP/FN, objectifs risk) + exigences conformité.
2) Data governance : schéma, qualité, droits dataset, fairness checks.
3) Durcissement API : auth, rate limiting, redaction logs, rétention.
4) Observabilité : dashboard + alerting (latence/drift/erreurs).
5) Retraining : déclencheur drift → pipeline training → validation → promotion modèle.

### 4.2 Aide à la prise de décision

Risques & opportunités :
- Risques : biais (genre), RGPD, dérive data, sécurité API, cold start/latence.
- Opportunités : automatisation décisionnelle, réduction temps analyste, standardisation.

## 5. Contrôle et suivi du projet

### 5.1 Tableau de bord de pilotage

Éléments déjà disponibles :
- Latence observée (42 requêtes /predict) : mean 262.70 ms ; median 0.92 ms ; min 0.45 ; max 2599.65.
- Note latence : les 42 observations proviennent des lignes de logs ; on observe 40 `request_id` uniques (présence de doublons dans le fichier).
- Dérive (data drift) : comparaison de distributions via test KS sur les probabilités prédites.
  - Résultat (Evidently, K‑S) : drift détecté sur `probability_default` — p-value = 0.015873… (< 0.05), `dataset_drift = true` (cf. `drift_report.json`).
  - Interprétation (posture prod/portfolio) : c’est un **signal** à investiguer (taille d’échantillon, drift sur scores uniquement, impact sur métriques) qui déclenche un plan d’action.
  - Plan d’action (si drift confirmé/impactant) : analyser les features contributrices, suivre les KPI techniques, puis éventuel retraining/recalibration → revalidation → redéploiement via CI/CD.

## 6. Conclusion & recommandations

Résumé des choix clés :
- API FastAPI containerisée, modèle LightGBM (pipeline), CI/CD vers Spaces.
- Logs structurés + monitoring latence et drift.

## 7. Annexes (exemples)

- Démo : https://perachon-credit-scoring-api-v2.hf.space/docs
- Code (GitHub) : https://github.com/perachon/p6-8-MLOps
- Repo (Spaces) : https://huggingface.co/spaces/perachon/credit-scoring-api-v2/tree/main
- Modèle HF Hub : `perachon/credit-scoring-model`
