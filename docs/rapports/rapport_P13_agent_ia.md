# Rapport — P13 Mettez en place un Agent IA (ouvertures aux échecs — FFE)

## 1. Contexte et objectif

- Sujet : mise en place d’un **agent IA conversationnel** capable d’interagir avec une base de connaissances et des outils externes.
- Architecture et concepts cités : **LangGraph** (orchestration par graphe), **RAG** avec base vectorielle (ex. **Milvus**), intégration d’APIs externes et **MCP (Model Context Protocol)**.

Mission (scénario) :

- Client : Fédération Française des Échecs (FFE).
- Objectif : POC d’un agent d’apprentissage des **ouvertures aux échecs** pour des jeunes joueurs.
- Contraintes : POC à livrer en **2 semaines**.

## 2. Fonctionnalités attendues (extraits)

- Proposer les meilleurs coups théoriques issus de la théorie.
- Donner du contexte d’ouverture via des données enrichies (parties historiques).
- Afficher des vidéos explicatives pertinentes (YouTube API).
- Évaluer la position (ex. Stockfish) lorsque la partie sort de la théorie.

## 3. Stack et intégrations (extraits)

- Frontend : interface Angular avec échiquier interactif (ngx-chessboard / material-chessboard).
- Backend/agent : LangGraph + FastAPI.
- Données : Milvus (recherche vectorielle) + MongoDB.
- Outils et APIs :
  - Position identifiée par **FEN**.
  - **API Lichess** (bibliothèque d’ouvertures, parties de référence).
  - **Stockfish** (moteur d’échecs).
  - YouTube API (vidéos explicatives).

## 4. Endpoints et étapes proposées (extraits)

- Étape “setup” : dépôt Git, structure `backend/` + `frontend/`, `docker-compose.yml` (FastAPI + Milvus + MongoDB + Angular).
- Recommandation : route FastAPI de healthcheck ` /api/v1/healthcheck`.

Endpoints backend attendus (extraits) :

- `GET /api/v1/moves/{fen}` : coups théoriques (via Lichess).
- `GET /api/v1/evaluate/{fen}` : évaluation Stockfish (ex. centipawns).
- `POST /vector-search` : recherche vectorielle (RAG) dans Milvus sur une ouverture/demande.

Bonnes pratiques citées : validation FEN et légalité des coups via `python-chess`, gestion d’erreurs/timeouts, attention aux limites de requêtes Lichess.

## 5. Livrables attendus (extraits)

- Système développé avec **Lang Graph, FastAPI, Milvus, MongoDB**.
- Code accessible via dépôt Git.
- Démonstration exécutable en local via **Docker Compose**.

---

Source : `projets/p13_agent_ia/p13_contexte_mission.md`.
