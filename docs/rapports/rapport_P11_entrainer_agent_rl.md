# Rapport — P11 Entraînez votre agent RL (Eagle‑1 / LunarLander‑v3)

## 1. Contexte et objectif

- Sujet : **apprentissage par renforcement (RL)** — un agent apprend à prendre des décisions par interaction pour maximiser une récompense cumulative.
- Environnement : **LunarLander‑v3** (atterrisseur “Eagle‑1”).
- Formulation RL (extraits) :
  - Observation : **8 valeurs** (position, vitesse, angle…).
  - Action : **4 actions discrètes** (moteurs).
  - Reward : favorise un atterrissage contrôlé, pénalise crash et consommation de fuel.

Objectif de validation (brief) :

- Métrique : **mean_reward** sur **100 épisodes**.
- Critère de succès : **mean_reward > 200** de manière stable.

## 2. Approche et méthode

Méthode en 3 étapes (extraits) :

1) Explorer l’environnement et établir une **baseline**.
2) Optimiser progressivement, avec évaluation stable sur 100 épisodes.
3) Déployer une démonstration réutilisable : **API + GUI + dashboard + vidéo**.

## 3. Choix d’algorithme

- Démarrage : **DQN** (adapté à un action space discret) pour obtenir une référence.
- Choix final : **PPO** (plus stable et meilleur score observé sur l’environnement).
- Meilleur run conservé : **PPO + gamma = 0.999**.

## 4. Résultats (extraits)

- Objectif : mean_reward > 200 sur 100 épisodes.
- Résultat annoncé : **~267 ± 21** (sur 100 épisodes).
- Pratique : sauvegarde du **best model** et historique des runs.

## 5. Livrables et “mise en usage”

Architecture (extraits) :

- **API** au centre : charge le modèle, calcule l’action, joue un épisode, génère une vidéo, loggue.
- **GUI** : client simple qui appelle l’API pour lancer un run et afficher la vidéo + résumé (reward/steps).
- **Dashboard** : lecture des logs (CSV) avec filtres simples.

Endpoints API (extraits) :

- `POST /predict` : observation → action.
- `POST /play` : lance 1 épisode complet + vidéo + log.
- Documentation : Swagger sur `/docs`.

Vidéo (extraits) :

- Contrainte : vidéo **20–30 s**.
- Réglages : **30 fps**, et répétition de frames (ex. `frame_repeat = 3`) pour obtenir ~**25–30 s** sur un épisode classique.

## 6. Limites et suites (extraits)

- Limite : variabilité selon la seed et les conditions exactes.
- Suites proposées : davantage d’évaluations **multi‑seeds**, analyses sur les actions, monitoring plus fin, packaging.

---

Sources :
- `projets/p11_entrainer_agent_rl/p11_contexte_mission.md`
- `projets/p11_entrainer_agent_rl/p11_diapo.md`
- `projets/p11_entrainer_agent_rl/p11_questions.md`
