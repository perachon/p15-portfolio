
Q/R


1) 

Q: “Pourquoi LunarLander ?”
R: “C’est un environnement standard RL, assez riche mais accessible, et il permet de juger la stabilité de l’agent.”

Q: “C’est quoi ‘utilisable’ ici ?”
R: “Pas juste un notebook: on peut appeler un endpoint pour obtenir une action, lancer un épisode via l’API, et suivre des métriques.”

Q: “C’est quoi l’idée du projet en une phrase ?”
R: “Entraîner un agent RL >200 et le rendre utilisable via API + démo.”

Q: “Tu vas montrer une démo ?”
R: “Oui, via l’endpoint /play et la GUI.”



2) 

Q: “Tu as modifié l’état ou les rewards ?”
R: “Non, je suis resté sur l’environnement standard.”

Q: “Pourquoi c’est difficile ?”
R: “Il faut gérer un compromis: stabilité, précision, carburant.”



3)

Q: “Pourquoi tu déploies alors que c’est un projet RL ?”
R: “Parce que le livrable demande API/GUI/dashboard, et ça montre que le modèle est réellement réutilisable.”



4) 

Q: “Pourquoi regarder aussi l’écart‑type ?”
R: “Pour éviter un score moyen haut mais très instable.”

Q: “Tu logs où ?”
R: “Dans des CSV (eval training + runs de démo).”



5) 

Q: “PPO c’est pas plutôt continu ?”
R: “Il fonctionne aussi en discret, SB3 le gère.”

Q: “Tu as essayé d’autres hyperparams ?”
R: “Oui, mais j’ai fait des changements un par un pour garder une comparaison propre.”



6) 

Q: “C’est reproductible ?”
R: “La moyenne sur 100 épisodes est stable, et pour la démo j’utilise deterministic + seed.”

Q: “Tu as gardé le meilleur checkpoint comment ?”
R: “Sauvegarde automatique du best model pendant le training.”


7) 

Q: “Pourquoi séparer comme ça ?”
R: “Pour éviter de dupliquer le modèle/les dépendances dans l’UI et pour mieux contrôler l’exécution.”

Q: “Le dashboard appelle aussi l’API ?”
R: “Non, il lit les logs (plus simple), mais il pourrait.”


8)

Q: “Pourquoi /play plutôt qu’un streaming temps réel ?”
R: “C’était plus simple et suffisant pour la soutenance.”

Q: “Qu’est-ce qui est loggé ?”
R: “Reward total, steps, seed, modèle, URL de la vidéo.”


9)

Q: “Ça fausse la démo ?”
R: “Non, c’est juste un ralentissement visuel; les décisions restent les mêmes.”

Q: “Pourquoi pas baisser à 10 FPS ?”
R: “Ça marche, mais visuellement c’est plus saccadé.”


10)

Q: “La GUI marche sans l’API ?”
R: “Non, c’est volontaire, elle consomme l’API.”

Q: “Tu peux lancer plusieurs runs ?”
R: “Oui, et ça alimente le CSV des runs.”


11) 

Q: “Pourquoi séparer training vs démo ?”
R: “Parce que ce ne sont pas les mêmes données ni le même usage.”

Q: “Tu pourrais afficher des stats sur les actions ?”
R: “Oui, mais je suis resté focus sur les métriques demandées.”


12)

Q: “Le point le plus fragile ?”
R: “La variabilité selon la seed et les conditions exactes, même si la moyenne sur 100 épisodes est bonne.”

Q: “Tu peux expliquer ton choix final en une phrase ?”
R: “PPO parce que c’est celui qui m’a donné la meilleure performance stable.”