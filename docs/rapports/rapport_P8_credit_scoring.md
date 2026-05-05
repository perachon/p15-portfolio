# Rapport de conduite de projet AI Engineering — Credit Scoring API

_Date : 22 avril 2026_

> Ce document est une **synthèse structurée** (inspirée d’un canevas de la formation), adaptée pour un usage portfolio/entretien.

## 1. Contexte et analyse des besoins

### 1.1 Organisation & contexte

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

Méthodologie (recueil & analyse) — adaptée à un POC :

- Recueil : analyse documentaire (brief + consignes), clarification des objectifs (business/tech), et revue de la solution cible (API, logs, monitoring) pour identifier les exigences non-fonctionnelles.
- Analyse : formulation des besoins en items testables (fonctionnels + non-fonctionnels) et identification des contraintes (sécurité, conformité, exploitation).
- Justification : un POC “prod-oriented” vise d’abord la **fiabilité** et la **traçabilité** (tests/logs/monitoring) afin de rendre la solution évaluables, puis la sécurité/alerting/retraining en itérations.

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

Backlog de besoins (formalisé) :

| ID | Besoin | Dimension | Type | Priorité | Critère d’acceptation (extrait) |
|---:|---|---|---|---|---|
| B1 | Prédire $P(\text{défaut})$ et retourner une décision `ACCEPTED/REFUSED` via API | Métier | Fonctionnel | Must | `POST /predict` répond 200 + champs attendus + décision par seuil |
| B2 | API stable + documentée | Tech | Non-fonctionnel | Must | Swagger `/docs` + `GET /health` |
| B3 | Validation d’entrée + gestion d’erreurs | Tech | Non-fonctionnel | Must | 422 sur input invalide ; erreurs système séparées |
| B4 | Traçabilité (logs structurés) | Exploitation | Non-fonctionnel | Must | log JSONL par requête : `request_id`, `latency_ms`, `status_code` |
| B5 | Suivi de performance (latence) | Exploitation | Non-fonctionnel | Should | KPI latence calculables (median/mean/max) + détection outliers |
| B6 | Monitoring drift (signal de dérive) | Métier/Exploitation | Non-fonctionnel | Should | test KS sur distribution scores + rapport |
| B7 | Sécurité (auth + rate-limit) | Tech/Organisation | Non-fonctionnel | Could (POC) | endpoints protégés ; limitations d’usage |
| B8 | Conformité (RGPD, rétention logs, minimisation) | Réglementaire | Non-fonctionnel | Should | politique de rétention + redaction PII dans logs |
| B9 | Coûts & capacité (infra, stockage logs) | Business/Tech | Non-fonctionnel | Should | estimation ordre de grandeur + variables de chiffrage |

Priorisation (valeur/effort) — lecture rapide :

| Item | Valeur métier | Effort | Décision |
|---|---|---|---|
| B1–B4 | Élevée | Moyen | 1ère itération (POC) |
| B5–B6 | Élevée | Moyen | 2e itération (observabilité) |
| B8–B9 | Moyenne→Élevée | Moyen | cadrage avant prod |
| B7 | Élevée | Élevé | itération “durcissement” |

## 2. Audit de la solution data existante (ou proposée si absence de solution existante)

### 2.0 Démarche d’audit (méthodologie)

Objectif : évaluer l’adéquation **POC ↔ besoins** sur 4 axes : (1) flux & robustesse API, (2) qualité/traçabilité des données d’entrée & sorties, (3) performance/latence, (4) risques (sécurité, conformité, biais).

Étapes (et outils) :

1) **Définir le périmètre** : endpoints, contrat d’entrée (schéma), décisions métier (seuil).
2) **Audit API/robustesse** : tests (pytest), validation Pydantic, revue des codes d’erreurs.
3) **Audit observabilité** : inspection des logs JSONL (champs, volumétrie, doublons de `request_id`).
4) **Audit performance** : mesures latence à partir des logs + profiling (`cProfile`) pour identifier un goulot.
5) **Audit drift** : Evidently + test KS sur la distribution des probabilités (signal de dérive).
6) **Audit risques** : checklist POC vs prod (auth/rate-limit, rétention logs, DPIA, fairness).

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
- Coût : **estimation partielle (ordre de grandeur)** + méthode de chiffrage (cf. 5.3). Drivers principaux : compute CPU (API + inférence), cold start/téléchargement du modèle depuis le Hub, stockage/rotation des logs, hébergement.
- Maintenance : CI/CD présent ; version applicative `v1` ; dépendances séparées API/dev.
- Monitoring : latence + drift présents ; plan d’action (drift) explicité en 5.1 (analyse cause → suivi KPI → éventuel retraining/recalibration → revalidation → redéploiement CI/CD).

Écarts / limites à documenter :
- Justification business du seuil 0.2 (coût FP/FN) : dans la mission OpenClassrooms, on peut **supposer** qu’un faux négatif (mauvais client prédit bon) coûte **10×** un faux positif (bon client refusé). Le seuil doit donc être **optimisé** sur un score métier (coût d’erreur) plutôt que fixé à 0.5.
- Gouvernance data : **non formalisée dans le POC** (pas de data contract, pas de contrôles qualité systématiques, pas d’analyse fairness documentée). Schéma d’entrée défini via Pydantic ; vigilance sur les variables sensibles potentielles (ex. genre) et les biais. Droits/licence exacts de la source du dataset : non documentés dans les livrables du POC.
- Observabilité (dashboard, alerting) : partiel.

## 3. Identification d’une solution technique cible

### 3.1 Mapping besoins ↔ solution (POC) ↔ écarts

| Besoin | Implémentation POC | État | Commentaire |
|---|---|---|---|
| B1 (prédire + décider) | `POST /predict` + seuil `THRESHOLD=0.2` | OK | seuil métier à relier à un coût FP/FN |
| B2 (API stable) | FastAPI + `/docs` + `/health` | OK | démontrable en public (Spaces) |
| B3 (validation) | Pydantic + tests | OK | distingue erreurs input (422) |
| B4 (logs structurés) | JSONL request-level | OK | prévoir redaction/rotation en prod |
| B5 (latence) | métriques via logs | Partiel | outliers présents → traiter cold start |
| B6 (drift) | Evidently + KS sur scores | Partiel | signal OK ; étendre à drift features + impact perf |
| B7 (sécurité) | non implémentée | Non (POC assumé) | à ajouter avant prod |
| B8 (RGPD/rétention) | non formalisé | Non (POC assumé) | à cadrer (DPIA, rétention, minimisation) |
| B9 (coûts/capacité) | chiffrage fournisseur à renseigner | Partiel | méthode + variables + hypothèses trafic |

### 3.2 Solution technique cible (pour couvrir sécurité, conformité, scalabilité, coûts)

Objectif : proposer une cible “prod-ready” qui réponde aux contraintes au-delà du POC.

- Sécurité : authentification (API key/OAuth), rate limiting, gestion des secrets (vault/CI secrets), durcissement CORS, et limitation des endpoints sensibles.
- Conformité/RGPD : minimisation (ne logger que le nécessaire), redaction/anonymisation, politique de rétention (ex. 30 jours), et analyse d’impact (DPIA) si données personnelles réelles.
- Scalabilité : container stateless + autoscaling, cache artefact modèle (éviter téléchargement au runtime), warmup au démarrage.
- Coûts : capacité dimensionnée à partir du trafic (req/j), du budget de latence, et de la volumétrie des logs ; chiffrage fournisseur à partir des unités (CPU-heures, GB stockés, egress).
- Observabilité : dashboard (latence, erreurs, drift) + alerting + runbook (cadence de revue, seuils, actions).

### 3.3 Cas d’usage (identification, description, priorisation)

| Cas d’usage | Description | Valeur | Complexité | Priorité |
|---|---|---|---|---|
| UC1 — Scoring temps réel | Requête → score + décision `ACCEPTED/REFUSED` | Élevée | Moyenne | Must |
| UC2 — Traçabilité / audit | Conserver des logs structurés (request_id, latence, statut) | Élevée | Moyenne | Must |
| UC3 — Monitoring & itération | Détecter drift/latence anormale et déclencher une revue | Élevée | Moyenne | Should |
| UC4 — Durcissement sécurité | Auth + rate-limit + redaction | Élevée | Élevée | Could (POC) / Should (prod) |

Méthode : priorisation valeur/effort (impact métier + risques), alignée avec le contexte (API exposée, contraintes latence, exigences traçabilité).

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

### 4.3 Jalons, livrables, ressources (estimation)

Jalons (format “livrables vérifiables”) :

- J1 — Cadrage : backlog priorisé (B1–B9) + critères d’acceptation + risques.
- J2 — API & contrat : endpoints + schéma d’entrée + tests critiques.
- J3 — CI/CD : pipeline vert (tests) + build Docker + déploiement.
- J4 — Observabilité : logs structurés + scripts KPI + rapport drift.
- J5 — Revue “Go/No-Go” : KPI vs SLO + plan durcissement (sécurité/conformité).

Ressources (ordre de grandeur, POC) :

- 1 AI Engineer / MLOps : 4–7 jours (API, packaging, CI, logs, monitoring POC).
- 1 Data Scientist (support) : 1–2 jours (KPI ML, seuil, analyse dérive).
- 1 Référent métier : 0,5–1 jour (validation critères business, coût FP/FN).
- 1 Référent conformité/sécurité : 0,5–1 jour (DPIA-lite, exigences d’accès/rétention).

### 4.2 Aide à la prise de décision

Risques & opportunités :
- Risques : biais (genre), RGPD, dérive data, sécurité API, cold start/latence.
- Opportunités : automatisation décisionnelle, réduction temps analyste, standardisation.

## 5. Contrôle et suivi du projet

### 5.1 Tableau de bord de pilotage

KPI (définis & évalués sur les données POC disponibles) :

| KPI | Objectif (SLO / règle) | Valeur observée (POC) | Statut | Notes |
|---|---|---:|---|---|
| Latence médiane /predict | &lt; 50 ms | 0,92 ms | OK | la médiane est excellente |
| Latence moyenne /predict | &lt; 200 ms | 262,70 ms | Partiel | tirée par un outlier (cold start) |
| Latence max /predict | &lt; 1500 ms | 2599,65 ms | Non | outlier à traiter (warmup/cache) |
| Drift scores (KS) | p-value &lt; 0,05 = drift (déclencheur) | 0,015873… | OK | signal de dérive → investigation |
| Qualité modèle (AUC) | AUC ≥ 0,75 (POC) | ~0,77 | OK | selon comparatif CV |

Visualisation (version HTML) : un résumé de la latence est inclus pour rendre le diagnostic lisible en revue projet.

Éléments déjà disponibles :
- Latence observée (42 requêtes /predict) : mean 262.70 ms ; median 0.92 ms ; min 0.45 ; max 2599.65.
- Note latence : les 42 observations proviennent des lignes de logs ; on observe 40 `request_id` uniques (présence de doublons dans le fichier).
- Dérive (data drift) : comparaison de distributions via test KS sur les probabilités prédites.
  - Résultat (Evidently, K‑S) : drift détecté sur `probability_default` — p-value = 0.015873… (< 0.05), `dataset_drift = true` (cf. `drift_report.json`).
  - Interprétation (posture prod/portfolio) : c’est un **signal** à investiguer (taille d’échantillon, drift sur scores uniquement, impact sur métriques) qui déclenche un plan d’action.
  - Plan d’action (si drift confirmé/impactant) : analyser les features contributrices, suivre les KPI techniques, puis éventuel retraining/recalibration → revalidation → redéploiement via CI/CD.

### 5.3 Estimation infra / coûts (ordre de grandeur)

Objectif : rendre le projet **chiffrable** sans inventer de prix fournisseur.

- Compute : 1 service API CPU (container) + mémoire pour charger le modèle.
- Stockage logs (ordre de grandeur) : si 1 ligne JSONL ≈ 1–2 KB et 10 000 requêtes/jour → 10–20 MB/jour → 0,3–0,6 GB/mois.
- Coût “à renseigner” : $\text{coût} = (\text{CPU-heures} \times \text{prix CPU}) + (\text{GB logs} \times \text{prix stockage}) + (\text{egress} \times \text{prix réseau})$.

Décision : en prod, définir une **politique de rétention** (ex. 30 jours) + rotation, et anonymiser/redacter les champs sensibles.

## 6. Conclusion & recommandations

Résumé des choix clés :
- API FastAPI containerisée, modèle LightGBM (pipeline), CI/CD vers Spaces.
- Logs structurés + monitoring latence et drift.

Recommandations décisionnelles (extraits) :

- Formaliser un Go/No-Go : SLO latence (p95) + KPI business (coût FP/FN, taux d’acceptation) + critères de rollback.
- Avant prod : auth + rate-limit, redaction logs + rétention, et checks fairness si variables sensibles.
- Drift : étendre au drift des features et à l’impact sur AUC/recall ; définir un runbook (cadence, alerting, actions).

## 7. Liens

- Démo : https://perachon-credit-scoring-api-v2.hf.space/docs
- Code (GitHub) : https://github.com/perachon/p6-8-MLOps
- Repo (Spaces) : https://huggingface.co/spaces/perachon/credit-scoring-api-v2/tree/main
- Modèle HF Hub : `perachon/credit-scoring-model`
