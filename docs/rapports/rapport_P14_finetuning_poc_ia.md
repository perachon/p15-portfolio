# Rapport — P14 Fine-tunez votre propre LLM (POC triage médical — CHSA)

## 1. Contexte et objectif

- Mission (scénario) : Centre Hospitalier Saint‑Aurélien (CHSA) — surcharge aux urgences.
- Besoin : POC d’un agent de **triage médical** pour assister le personnel soignant.
- Attendus fonctionnels (extraits) :
  - Questionnaire intelligent adaptatif pour collecter les symptômes.
  - Évaluation du niveau de priorité (urgence maximale / modérée / différée) selon protocoles.
  - Explications claires, traçabilité des interactions pour audit.
  - Intégration possible au SIH.

Contraintes :

- POC à livrer en **4 semaines**.
- Méthodologie rigoureuse et validation (sécurité, hallucinations, recommandations dangereuses).

## 2. Stratégie en phases (extraits)

Phase 1 — Validation conceptuelle

- Modèle : **Qwen3‑1.7B‑Base** (compact) pour valider rapidement.

Phase 2 — Optimisation ciblée

- **SFT (Supervised Fine‑Tuning)** + **LoRA** (entraînement économe en GPU).
- Alignement par préférences : **DPO** (mention possible : DPO / GRPO dans les attendus).

Phase 3 — Projection industrielle

- Possibilité d’aller vers des modèles plus grands (32B+) en cas de validation.

## 3. Données et gouvernance (extraits)

- Corpus médicaux cités : MediQA, FrenchMedMCQA, MedQuAD, UltraMedical‑Preference.
- Objectif SFT : ~**5 000 paires** instruction‑réponse.
- DPO : constitution d’un dataset de paires préférentielles (réponses validées / non validées).
- Bilingue : français / anglais.
- RGPD : anonymisation + justification du processus ; recommandation d’outil **Presidio**.
- Format/stockage : JSONL / Hugging Face Datasets, métadonnées documentées.

Points de vigilance cités : ne pas mélanger entraînement et évaluation, conserver une trace des transformations (auditabilité).

## 4. Entraînement et suivi expérimental (extraits)

- Outils cités : PyTorch, Hugging Face Transformers, PEFT (LoRA) ; tracking via **MLflow** (ou W&B).
- Bonnes pratiques : petits runs LoRA au départ, checkpoints, documentation des versions (hyperparams + seed).
- Évaluation : métriques cliniques et seuils d’acceptation, contrôles de sécurité.

## 5. API, sécurité et traçabilité (extraits)

- API de triage : recommandation “API plutôt qu’un notebook” (testable, intégrable).
- Garde‑fous de sécurité (extraits) : logique de **red flags** (ex. douleur thoracique, malaise, saignement important) ; priorité forcée et réponse prudente, en court‑circuitant le génératif.
- Traçabilité (extraits) : audit des interactions stocké en **SQLite** avec endpoint dédié ` /audit/{interaction_id}`.
- Démonstration soutenance (ordre conseillé) :
  1) `GET /health`
  2) `POST /triage` cas standard
  3) `POST /triage` cas avec red flag
  4) `GET /audit/{interaction_id}`

## 6. Déploiement, performance et CI/CD (extraits)

- Déploiement : conteneurisation Docker + exposition via FastAPI.
- Inférence optimisée : **vLLM** (trajectoire d’industrialisation).
- CI/CD : GitHub Actions (qualité + tests + build Docker) ; lint via **Ruff**, tests via **Pytest**.

Mesures de latence (extraits vocabulaire) :

- Local réel : **P50 ~13,5 s** (vrai modèle local).
- Cloud stub : **P50 ~0,23 s** (backend léger).

## 7. Livrables attendus (extraits)

- Dataset médical bilingue (HF Datasets / JSONL), versionné.
- Modèle spécialisé (SFT + LoRA + alignement DPO) et poids finaux.
- Endpoint de démonstration (cloud) optimisé (vLLM).
- Pipeline CI/CD (GitHub Actions).
- Rapport technique + recommandations stratégiques (20 pages max).

---

Sources :
- `projets/p14_finetuning_poc_ia/p14_contexte_mission.md`
- `projets/p14_finetuning_poc_ia/p14_livrables_soutenance.md`
- `projets/p14_finetuning_poc_ia/p14_questions.md`
- `projets/p14_finetuning_poc_ia/p14_vocabulaire.md`
