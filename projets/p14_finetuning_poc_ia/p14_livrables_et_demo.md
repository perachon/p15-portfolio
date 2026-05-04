# Livrables & guide de démo (POC)

Ce document récapitule les livrables du POC et propose un déroulé de démo/révue technique réutilisable en entretien.

## Activités (périmètre)

- Préparation et structuration des données
- Entraînement initial par fine-tuning supervisé (SFT)
- Alignement du modèle par préférences (DPO)
- Déploiement et validation

## Livrables

- Dataset médical bilingue (Hugging Face Datasets / JSONL), versionné
- Modèle spécialisé et optimisé (SFT + LoRA, puis DPO)
- Rapport technique + recommandations (format PDF, ~20 pages max)
- Endpoint de démo déployé (cloud) avec inférence optimisée (vLLM)
- Pipeline CI/CD (GitHub Actions)

## Déroulé de démo (≈ 20–30 min)

### 1) Démonstration (10–15 min)

- Vérifier la disponibilité : `GET /health`
- Exécuter un cas “standard” : `POST /triage`
- Exécuter un cas “red flag” : montrer le court-circuit des garde-fous
- Montrer la traçabilité : `GET /audit/{interaction_id}` (données, décision, logs)

### 2) Revue technique (10 min)

- Données : provenance, préparation, limites, reproductibilité
- Modèle : choix (taille/latence), SFT + LoRA, alignement DPO
- Sécurité : garde-fous, périmètre du POC, risques résiduels
- Performance : latence, approche stub vs modèle réel, trajectoire d’optimisation

### 3) Conclusion (2–5 min)

- Résumer la valeur du POC (faisabilité + garde-fous + traçabilité)
- Proposer les prochaines étapes (évaluation plus robuste, multi-seeds, données réelles, conformité)