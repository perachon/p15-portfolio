# Rapport — P15 (projet personnel à dominante technique)

**Projet mis en avant : P8 — Credit Scoring API**

_Date : 03 mai 2026_

> Objectif : fournir un document court, directement aligné avec les **5 compétences évaluées** au P15, en s’appuyant sur le projet P8 comme projet personnel à dominante technique.
>
> Règle : **ne pas inventer**. Si une information n’est pas disponible dans les livrables existants, la laisser `TBD`.

## 1) Contexte P15 et choix du projet

Le P15 demande de réaliser un **projet personnel à dominante technique** autour de problématiques d’IA en production (MLOps, versioning, monitoring des dérives, etc.), puis de le valoriser dans un **portfolio**.

Pourquoi **P8 (Credit Scoring API)** :

- Dominante technique “IA en production” : **API**, **Docker**, **CI/CD**, **tests**, **logs structurés**, **monitoring** (latence + drift).
- Projet facilement “défendable” en soutenance : démonstration claire (`/health`, `/predict`) + preuves (logs, scripts de monitoring, analyses).
- Bon support pour parler de compromis et risques : sécurité (auth/rate-limit), gouvernance data (provenance / fairness), observabilité (alerting), drift et stratégie de retraining.

## 2) Résumé de la solution (P8)

- Service : **FastAPI** avec `GET /health` et `POST /predict`.
- Modèle : pipeline **LightGBM** sérialisé (joblib), récupéré depuis **Hugging Face Hub**, chargé au démarrage.
- Packaging : **Docker** (déploiement démo sur Hugging Face Spaces).
- Qualité : tests **Pytest** exécutés en CI ; validation d’entrée via **Pydantic**.
- Observabilité (POC) : logs JSONL des requêtes + analyses de **latence** et **drift** (Evidently).

Référence : le rapport détaillé du P8 est dans `docs/rapports/rapport_P8_credit_scoring.html`.

---

## 3) Compétences évaluées (P15) — preuves et positionnement

### 3.1 Collecter les besoins métiers et analyser le contexte de l’organisation

**Ce qui est fait**

- Besoin métier cadré : estimer $P(\text{défaut})$ et produire une décision `ACCEPTED/REFUSED` à partir d’un seuil métier.
- Contexte et parties prenantes (scénario OpenClassrooms) : département “Crédit Express”, DS lead, manager, usage quasi temps réel.

**Preuves / éléments vérifiables**

- Endpoint `POST /predict` + schéma d’entrée Pydantic.
- Décision par seuil `THRESHOLD=0.2` (tel qu’utilisé dans le POC).

**Points à améliorer (posture prod)**

- Formaliser la collecte des exigences non-fonctionnelles : SLO latence, sécurité, conformité, politique de rétention des logs.
- Mieux relier le seuil à une optimisation “coût FP/FN” (score métier) et définir un protocole de revue.

### 3.2 Auditer la solution data afin d’en déterminer l’adéquation avec des besoins identifiés

**Ce qui est fait**

- Audit “POC vs prod” explicite : robustesse OK (validation + tests), observabilité partielle (monitoring sans alerting), sécurité à renforcer.
- Analyse de la performance : latences observées et investigation des goulots (profiling).

**Preuves / éléments vérifiables**

- Latence /predict mesurée via logs (extrait : moyenne, médiane, max).
- Drift détecté via test K‑S sur distribution de scores (`dataset_drift=true`) ; plan d’action proposé.

**Points à améliorer (posture prod)**

- Chiffrage coût (hébergement, trafic, stockage/rotation logs) : `TBD`.
- Gouvernance data : provenance/licences, data quality, fairness (non documenté dans le POC).

### 3.3 Identifier une solution technique afin de répondre aux besoins

**Ce qui est fait**

- Choix technique adapté à un service temps réel : FastAPI + modèle en mémoire + Docker.
- Versioning et reproductibilité : artefact modèle (Hub) + runs (MLflow) selon livrables.

**Preuves / éléments vérifiables**

- Démo publique Swagger `/docs` (Spaces).
- Pipeline CI/CD (tests → build image → déploiement Spaces).

**Points à améliorer (posture prod)**

- Sécuriser l’exposition : auth, rate-limit, redaction/PII dans les logs.
- Stratégie de retraining : triggers, pipeline, validation, promotion (registry).

### 3.4 Apporter un appui stratégique et méthodologique pour faciliter la prise de décision

**Ce qui est fait**

- Identification des risques (sécurité, conformité, drift, biais) et proposition d’une trajectoire d’industrialisation.
- Capacité à expliciter les compromis : qualité ML vs latence, monitoring vs effort, POC vs prod.

**Preuves / éléments vérifiables**

- Sections “écarts / limites” et “risques & mitigation” dans le rapport P8.
- Recommandations : dashboard/alerting, durcissement sécurité, gouvernance data.

**Points à améliorer**

- Formaliser un “Go/No-Go” : critères d’acceptation (KPI métier + KPI techniques), et calendrier.

### 3.5 Contrôler et analyser le projet data en termes de délais, coûts, livrables et performance

**Ce qui est fait**

- Livrables contrôlables : API fonctionnelle, CI/tests, logs structurés, scripts de monitoring.
- Performance : analyse latence + détection drift ; investigations via profiling.

**Preuves / éléments vérifiables**

- Logs JSONL ; mesures de latence ; rapport de drift.
- Tests automatisés exécutés en CI.

**Points à améliorer**

- Coûts : chiffrage et suivi (€/mois) selon trafic attendu : `TBD`.
- Délais : planification plus explicite (jalons, risques, buffer) : `TBD`.

---

## 4) Conclusion

Le P8 est un bon “projet personnel à dominante technique” car il démontre un chemin complet vers la production : exposition via API, packaging, automatisation, observabilité, et une approche rigoureuse des risques (drift, sécurité, gouvernance data).

## 5) Liens

- Rapport P8 (HTML) : `docs/rapports/rapport_P8_credit_scoring.html`
- Démo : https://perachon-credit-scoring-api-v2.hf.space/docs
- Repo (Spaces) : https://huggingface.co/spaces/perachon/credit-scoring-api-v2/tree/main
