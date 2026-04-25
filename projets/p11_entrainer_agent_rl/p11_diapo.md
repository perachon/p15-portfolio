		Entraîner un agent RL		 			Piloter l’atterrisseur lunaire Eagle‑1 (LunarLander‑v3)Objectif : agent RL performant + démonstration (API / GUI / Dashboard)       			 		

L’objectif, c’est d’avoir un agent qui atterrit correctement et pas juste ‘qui survit’.
Et surtout, je ne me suis pas arrêté au notebook: j’ai aussi livré une API pour interagir avec le modèle, une interface graphique pour voir une partie jouée sous forme de vidéo, et un dashboard pour suivre les performances

----

Le problème RL en 20 secondes

Concrètement, à chaque étape l’environnement me donne un état sous forme de 8 valeurs: 

Observation: 8 valeurs (position, vitesse, angle…)

Action: 4 actions discrètes (moteurs)

Reward: encourage un atterrissage stable + pénalise crash / fuel


c’est la position, les vitesses, l’angle… bref ce qui décrit la situation du lander.
L’agent doit répondre par une action parmi 4, donc c’est discret. Et la récompense guide l’apprentissage: elle pénalise les crashes et le gaspillage, et elle favorise un atterrissage contrôlé et stable.

---

Approche globale

Explorer + baseline

Optimiser jusqu’à mean reward > 200 (100 épisodes)

Déployer: API + GUI + Dashboard + vidéo
Je me suis imposé une méthode en 3 étapes. 
D’abord, explorer l’environnement et faire une baseline, juste pour avoir un point de comparaison.
Ensuite, je fais du tuning progressivement, avec une évaluation stable sur 100 épisodes pour éviter les conclusions ‘au hasard’.
Et enfin, je passe au déploiement: l’idée est qu’on puisse utiliser le modèle comme un service, pas seulement dans un notebook.

----

Baseline & critère de validation

Métrique: mean_reward sur 100 épisodes

Validation: objectif > 200 stable

Logging: historique des runs + sauvegarde best model

Le critère de succès du brief, c’est une récompense moyenne au‑dessus de 200 sur 100 épisodes. 

Donc je me suis calé dessus dès le début.
La baseline me sert à vérifier que j’améliore vraiment les choses. Et à chaque expérience, je logge les résultats et je sauvegarde le meilleur modèle, comme ça je peux revenir en arrière et comparer proprement.

----


Choix de l’algo (DQN → PPO)

DQN: ok pour démarrer, mais perf instable ici

PPO: plus stable, meilleur score final

Meilleur run: PPO + gamma=0.999

Au début j’ai testé DQN parce que c’est un choix naturel quand l’action space est discret. Ça marche pour avoir une première référence.
Mais sur cet environnement, je n’ai pas eu la stabilité et le score que je voulais. PPO s’est avéré plus régulier et plus performant dans mes essais.
Et le meilleur résultat que j’ai gardé vient d’un PPO avec gamma=0.999, donc qui valorise un peu plus le long terme.

----

Résultat final (objectif atteint)

Objectif: mean > 200 / 100 épisodes

Résultat: ~267 ± 21 (100 épisodes)

Modèle sauvegardé

Là on arrive au résultat final: on dépasse largement 200 de moyenne sur 100 épisodes, avec un écart‑type raisonnable, donc ce n’est pas juste un épisode ‘chanceux’.
Et surtout: j’ai un modèle sauvegardé qui correspond à ce résultat, donc je peux l’utiliser directement pour l’API et la démo, sans réentraîner à chaque fois.

---

Architecture (API au centre)

API: charge le modèle, calcule l’action, joue l’épisode, génère la vidéo, loggue

GUI: appelle l’API, affiche vidéo + résumé

Dashboard: lit les logs + filtres
Pour l’architecture, j’ai vraiment centralisé la logique RL côté API. 
Ça veut dire: c’est l’API qui charge le modèle, qui interagit avec Gymnasium, et qui produit la vidéo.
La GUI et le dashboard restent très simples: ce sont des clients. Ça colle à la contrainte du projet ‘pas de logique RL côté frontend’, et ça ressemble plus à une architecture réaliste.

------

API: endpoints

POST /predict : observation → action

POST /play : 1 épisode complet + vidéo + log

Swagger: /docs

L’API expose deux endpoints principaux. /predict, c’est le cas d’usage classique: on envoie un état et on récupère une action.
Et /play, je l’ai fait pour la démo: il lance un épisode complet, écrit les infos dans un log, et il renvoie un lien vers la vidéo générée.
Pour montrer tout ça facilement, je passe par Swagger sur /docs.

---

Vidéo 20-30s et fluidité

Contrainte: vidéo 20–30s

Réglage : 30 fps

Objectif: durée OK + lecture plus “fluide”


Un point un peu tricky, c’est la contrainte ‘vidéo 20 à 30 secondes’. 

Un épisode n’a pas forcément assez d’étapes pour faire 20–30 secondes à 30 FPS.
Du coup, pour garder une lecture agréable, j’encode en 30 FPS mais je répète les frames, ce qui ralentit la vidéo sans exploser la RAM, parce que j’écris au fil de l’eau.
En pratique, (avec frame_repeat=3), je retombe autour de 25–30 secondes sur un épisode classique.

---

GUI: ce que je montre

Bouton “Jouer via API”

Vidéo + résumé reward/steps

Paramètres: seed, deterministic, fps/repeat

La GUI, c’est la partie ‘présentation’ : en un clic je lance la partie via l’API et j’affiche la vidéo.
J’ai aussi laissé quelques paramètres simples: seed et deterministic pour sécuriser la démo, et les paramètres vidéo pour respecter la contrainte 20–30 secondes.

---

Dashboard métriques + filtres

Training: mean_reward (eval_history)

Démo: total_reward par run (episode_runs)

Filtres: env / algo

Le dashboard sert à voir rapidement deux choses. D’un côté, l’historique des évaluations pendant l’entraînement: c’est ce qui justifie l’atteinte du critère >200.
De l’autre, les runs joués via l’API pendant la démo: ça permet de voir si en pratique, l’agent garde une bonne performance.
Et j’ai mis des filtres simples pour pouvoir isoler un env ou un algo si besoin.

----

Conclusion & limites

Objectif atteint (>200 stable)

Livrables complets (notebook + vidéo + API + GUI + dashboard)

Limites / suite: multi‑seeds, monitoring actions, packaging

Pour conclure, j’ai bien atteint le critère principal de performance, et j’ai livré tout ce qui était demandé autour du modèle.
Si je devais aller plus loin, je ferais plus d’évaluations multi‑seeds et je rajouterais des analyses plus fines sur les décisions de l’agent selon les situations.
Mais pour le cadre du projet, je suis resté sur quelque chose de simple, démontrable, et réutilisable.
