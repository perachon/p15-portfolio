# Rapport de conduite de projet AI Engineering — P7 (Puls-Events RAG OpenAgenda)

_Date : 24 avril 2026_

> Ce document est une **synthèse structurée** (inspirée d’un canevas de la formation), adaptée pour un usage portfolio/entretien.

## 1. Contexte et analyse des besoins

### 1.1 Organisation & contexte

- Contexte : assistant conversationnel (RAG) pour répondre à des questions sur des **événements culturels** (récents < 1 an et à venir) sur un périmètre géographique (Paris-Saclay).
- Type d’organisation (scénario OpenClassrooms) : mission freelance pour **Puls-Events**, entreprise tech qui développe une plateforme de recommandations culturelles.
- Enjeux : améliorer la découverte d’événements pertinents, réduire le temps de recherche, et démontrer la faisabilité d’un chatbot basé sur les données OpenAgenda.
- Niveau de maturité IA / MLOps : **POC technique** — pipeline data + index FAISS + API + Docker ; pas de déploiement “prod”/SLO/alerting.

Contraintes :
- Dépendance à un LLM externe : nécessite une connexion Internet + service tiers (Mistral) ; coût par requête à estimer.
- Qualité : éviter l’hallucination (réponses sourcées/traçables via les documents récupérés).
- Fraîcheur : événements récents (< 1 an) et à venir → reconstruction de l’index si les données évoluent.
- Performance : temps de réponse variable selon la requête.
- Sécurité : secrets via variables d’environnement ; auth API non implémentée (POC).

### 1.2 Collecte et analyse du besoin métier

- Besoin : permettre à un utilisateur de poser une question en langage naturel et d’obtenir une réponse utile, contextualisée et **sourcée** (événements + date/lieu/ville + lien/source).
- Parties prenantes (scénario OC) : **Jérémy** (responsable technique Puls-Events) + équipes **Produit** et **Marketing** (cibles de la démo).
- Attendus (mission) : système RAG (LangChain + Mistral + FAISS) + scripts de (re)construction d’index + API REST + jeu de test annoté + tests unitaires + rapport technique.

## 2. Audit de la solution data existante (ou proposée si absence de solution existante)

### 2.1 Solution actuelle ou proposée

Solution actuelle (POC) :
- Données : OpenAgenda (agenda `universite-paris-saclay`).
- Volumétrie : ~3800 événements bruts ; dataset indexé ~661 (`data/processed/events_index_ready.jsonl`).
- Pipeline : ingestion → nettoyage → indexation FAISS persistée (`data/index/faiss_events/`).
- Architecture : séparation **offline** (collecte/nettoyage/chunking/embeddings/index) vs **online** (retrieval FAISS → génération LLM à partir des documents).
- RAG : LangChain + LLM Mistral (`mistral-small-latest`).
- Embeddings : `sentence-transformers/all-MiniLM-L6-v2` (local).
- Chunking : 800 caractères, overlap 120.
- API : FastAPI (`/ask`, `/rebuild`).
- Docker : démo locale ; exécution facilitée via Docker Compose (POC).

### 2.2 Évaluation de l’adéquation aux besoins

- Performance/qualité : évaluation via gold set (10 questions) et recouvrement d’UID.
  - Résultats : 5/10 correct, 5/10 partial, 0/10 incorrect.
  - Moyennes : precision=0.6183 ; recall=1.0 ; f1=0.69.

Remarques (POC) :
- Les réponses s’appuient sur les documents récupérés (sources) pour améliorer la traçabilité.
- Limites connues : dépendance LLM externe, couverture/qualité des données indexées, latence variable, périmètre géographique volontairement restreint.

## 3. Identification d’une solution technique cible

Architecture cible (à dessiner) :
- Scheduler refresh (quotidien) → ingestion → preprocessing → build index
- API FastAPI `/ask` : validation → retrieval FAISS → filtrage temps/géo → construction contexte → appel LLM → réponse + sources
- Observabilité : logs structurés + métriques latence + coût estimé

## 4. Stratégie de mise en œuvre et d’industrialisation

Roadmap proposée (exemple) :
1) Cadrage KPI métier (utilité, taux d’acceptation réponse) + contraintes coût.
2) Évaluation : enrichir gold set (ex 50–100 questions) + métriques complémentaires.
3) Baseline : BM25/keyword vs FAISS + ablations filtres.
4) Observabilité : logs + métriques (latence p50/p95, no-result rate, erreurs) + coût par requête.
5) CI : lancer tests + évaluation en pipeline.

## 6. Conclusion & recommandations

Résumé :
- POC RAG complet data → index → API → Docker.
- Garde-fous anti-hallucination + évaluation reproductible.

## 7. Liens

- Repo : https://github.com/perachon/p7-puls-events-rag
