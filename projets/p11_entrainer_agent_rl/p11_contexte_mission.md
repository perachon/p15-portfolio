(P11) Projet 11

Entrainez votre agent RL



Qu’allez-vous apprendre dans ce projet ?
 

Vous avez acquis des concepts fondamentaux en Intelligence Artificielle (IA) et en Machine Learning. Vous avez probablement déjà exploré des méthodes d'apprentissage supervisé et non supervisé. Ce projet va étendre vos connaissances à un domaine où les machines apprennent à prendre des décisions autonomes par interaction. 

 

Vous allez pratiquer les concepts fondamentaux de l'apprentissage par renforcement (RL). Cela permet de construire des agents qui apprennent à prendre des décisions par interaction avec un environnement pour maximiser une récompense (reward) cumulative. Vous plongerez aussi dans des notions comme l'état (observation), l'action, la politique (policy), la fonction de valeur (value function) et le modèle (model), ainsi que l'utilisation de bibliothèques comme Stable Baselines 3 (SB3) pour simplifier l'application.

 

 

 

En quoi ces compétences sont-elles importantes pour votre carrière ? 
 

Ces compétences sont utiles pour développer des systèmes autonomes et intelligents capables de résoudre des problèmes complexes dans des environnements dynamiques. Elles sont cruciales pour des applications allant de la robotique (ex: faire marcher un robot), à des véhicules autonomes (ex: piloter des hélicoptères, ou l'atterrissage de modules spatiaux) aux jeux vidéo (ex: jouer à des jeux Atari) et à l'optimisation de processus industriels (ex: contrôler une centrale électrique). 

Ces pratiques sont recherchées par les recruteurs en Intelligence Artificielle, Machine Learning et Robotique, car elles permettent aux machines d'apprendre et de s'adapter sans programmation explicite, en gérant des situations imprévues et des "inconnues inconnues".

 

 

Comment allez-vous procéder ? 
 

Ce projet est découpé en 6 activités.

Ressources pédagogiques : 2 vidéos vous sont fournies afin de vous initier aux concepts fondamentaux de l’apprentissage par renforcement aussi bien d’un point de vue théorique que pratique. 

Exercice : Vous réaliserez 3 exercices qui vous proposeront de prendre en main différents environnements virtuels. Vous utiliserez différents types d’agent afin d’explorer ces environnements.

Mission : Vous réaliserez un pilote automatique pour le nouveau module d'atterrissage "Eagle-1" en appliquant les concepts et algorithmes de RL appris. Vous terminerez en complétant la fiche d’autoévaluation qui servira de base de discussion avec votre mentor avant la session de bilan.

 

À l’issue de ce projet, vous aurez une session de bilan avec votre mentor pour discuter de votre projet.

Cela vous assurera que vous êtes sur la bonne voie avant de passer à la suite.

 
 Objectifs pédagogiques
Construire des tableaux de bord interactifs afin de visualiser les données
Définir les flux de données afin de sélectionner une API adaptée aux échanges
Formater les données et configurer une API
Identifier et choisir un système d’exposition des résultats

---

Exercice 1 - Découvrez les blocs de construction de l'Apprentissage par Renforcement

Dans cet exercice, vous allez interagir pour la première fois avec un environnement d'apprentissage par renforcement. L'objectif est de vous familiariser avec le cycle fondamental "observation -> action -> récompense" qui est au cœur de cette discipline. 

Etape 1 - Créez et explorez un environnement

Vous allez d'abord créer une instance de l'environnement classique "CartPole-v1" à l'aide de la bibliothèque Gymnasium. Ensuite, vous explorerez ses propriétés fondamentales :

l'espace d'observation c’est-à-dire ce que l'agent "voit"
l'espace d'action c’est-à-dire ce que l'agent "peut faire".
Prérequis 
 

Avoir installé les librairies suivantes : 

gymnasium pour les environnements,

stable-baselines3 pour les algorithmes pré-implémentés,

matplotlib pour la visualisation.


Résultat attendu
Des cellules de code qui affichent la structure de l'espace d'observation et de l'espace d'action, ainsi que des exemples.

 

Recommandations 
Utilisez gym.make("NomDeLEnvironnement") pour créer l'environnement.

Accédez aux attributs .observation_space et  .action_space de l'objet environnement créé.

Utilisez la méthode .sample() sur ces espaces pour voir à quoi ressemble une observation ou une action aléatoire.

 
Points de vigilance 
Noter le type de chaque espace. CartPole a un espace d'observation Box (continu) et un espace d'action Discrete (discret). 

---

Etape 2 Implémentez une politique aléatoire

Maintenant que vous comprenez l'environnement, vous allez coder une boucle simple qui fait interagir un agent avec cet environnement. Pour l'instant, l'agent sera très simple : il choisira ses actions de manière complètement aléatoire. Vous ferez tourner cette simulation pour 10 "épisodes" et afficherez la récompense totale pour chaque épisode.

 

Prérequis 
Avoir créé un environnement et exploré ses espaces.


Résultat attendu 
Une boucle for qui exécute 10 épisodes complets. 

Pour chaque épisode, une boucle while interne doit s'exécuter jusqu'à ce que l'épisode se termine, en choisissant et en appliquant une action aléatoire à chaque étape. 

La récompense totale de chaque épisode doit être affichée.

 

Recommandations 
Commencez chaque épisode par env.reset(). Cette fonction réinitialise le jeu et vous donne la toute première observation.

Dans la boucle while, utilisez env.action_space.sample() pour choisir une action aléatoire.

Passez cette action à env.step(action). Stockez les 5 valeurs qu'elle retourne.

La boucle while doit continuer tant que l'épisode n'est ni terminated ni truncated.

N'oubliez pas d'ajouter la reward reçue à une variable total_reward à chaque étape.

À la fin de votre script, utilisez env.close() pour fermer l'environnement proprement.

 
Points de vigilance 
Assurez-vous de bien réinitialiser les variables (comme total_reward, terminated, truncated) au début de chaque nouvel épisode dans la boucle for.

Outils 
Google Colab

Gymnasium

----

Exercice 2 - Entraînez un agent avec une table de décision (Q-table)

Dans cet exercice, vous allez implémenter de zéro votre premier véritable algorithme d'apprentissage : le Q-Learning. Vous allez construire une "table de qualité" (Q-table) qui guidera les décisions de votre agent pour résoudre l'environnement "FrozenLake-v1".

Etape 1 Préparez l'environnement et la Q-table

Commencez par créer l'environnement "FrozenLake-v1". Contrairement à CartPole, cet environnement a un nombre fini d'états, ce qui le rend idéal pour le Q-Learning. Ensuite, initialisez votre Q-table sous la forme d'un tableau NumPy rempli de zéros, avec une ligne pour chaque état et une colonne pour chaque action possible.

 
Prérequis
Avoir installé les librairies suivantes: 

Avoir fini le premier exercice.

Connaître les bases de la bibliothèque NumPy.


Résultat attendu  
Une Q-table (tableau NumPy) initialisée avec les bonnes dimensions ((16, 4) pour FrozenLake) et remplie de zéros. 

 

Recommandations 
Utilisez env.observation_space.n et env.action_space.n pour obtenir dynamiquement les dimensions nécessaires pour votre Q-table.

Utilisez np.zeros() pour créer le tableau.

Outils 
Numpy

Gymnasium

Etape 2 Imlémentez la boucle de Q-Learning

C'est le cœur de l'exercice. Vous allez coder la boucle d'entraînement principale. À chaque pas de temps, votre agent devra décider s'il explore (action aléatoire) ou s'il exploite ses connaissances (meilleure action de la Q-table). Après avoir exécuté l'action, vous mettrez à jour la valeur correspondante dans la Q-table en utilisant la formule de mise à jour du Q-Learning.

 

Prérequis 
Avoir initialisé la Q-table.

Avoir défini les hyperparamètres (learning_rate, discount_factor, epsilon, etc.).


Résultat attendu  
Une Q-table remplie de valeurs apprises après des milliers d'épisodes d'entraînement.

 

Recommandations  
Pour la stratégie epsilon-greedy, générez un nombre aléatoire entre 0 et 1. S'il est inférieur à epsilon, explorez. Sinon, exploitez.

Pour exploiter, utilisez np.argmax(q_table[state, :]) pour trouver l'index (l'action) de la plus grande valeur Q pour l'état actuel.

Traduisez la formule de Bellman en code Python : nouvelle_valeur = ancienne_valeur + lr * (recompense + gamma * max_q_futur - ancienne_valeur).

N'oubliez pas de réduire epsilon à la fin de chaque épisode pour que l'agent explore moins à mesure qu'il apprend.

 
Points de vigilance  
Assurez-vous de bien utiliser new_state pour trouver la valeur Q future maximale (np.max(q_table[new_state, :])).

La mise à jour se fait sur la paire (state, action) qui a été utilisée pour la transition.

Etape 3 - Evaluez votre agent entrainé

Maintenant que votre agent est entraîné, il est temps de mesurer ses performances. Vous allez exécuter un certain nombre d'épisodes d'évaluation (par exemple, 100) en utilisant la Q-table apprise. Cette fois, l'agent ne doit jamais explorer ; il doit toujours choisir la meilleure action qu'il connaît. Calculez son taux de réussite.

 
Prérequis 
Avoir une Q-table entraînée à l'étape précédente.


Résultat attendu 
Un script qui calcule et affiche le taux de réussite de l'agent sur un grand nombre d'épisodes, par exemple : "Taux de réussite sur 100 épisodes : 72%".

 

Recommandations  
Créez une nouvelle boucle d'évaluation, similaire à la boucle d'entraînement.

La grande différence : pour choisir l'action, utilisez toujours action = np.argmax(q_table[state, :]). Il n'y a plus de epsilon.

Incrémentez un compteur de victoires (total_wins) chaque fois qu'un épisode se termine avec une récompense de 1.0.

 
Points de vigilance  
Ne modifiez plus la Q-table pendant l'évaluation ! La phase d'apprentissage est terminée.

---

Exercice 3 - Remplacez la Q-table par un cerveau neuronal (DQN)

Dans cet exercice, vous implémenterez un Deep Q-Network (DQN) pour "CartPole-v1" de deux manières :

 

Manuellement avec PyTorch, pour comprendre les rouages du DQN (réseau de neurones, Experience Replay, Target Network).

Avec Stable-Baselines3, pour découvrir comment les professionnels accélèrent le développement avec des bibliothèques de haut niveau.

Cet exercice est entièrement guidé.
Suivez les étapes ci-dessous.

Etape 1 Définissez le réseau de neuronnes et le replay buffer (mnanuel)

Créez deux classes Python :

DQN (héritant de torch.nn.Module) pour le réseau de neurones.

ReplayBuffer pour stocker et échantillonner les transitions (état, action, récompense, nouvel état).

 
Prérequis
Connaissance conceptuelle des réseaux de neurones et PyTorch.


Résultat attendu 
Classes DQN et ReplayBuffer fonctionnelles.

Code instanciant un DQN et affichant son architecture.

 

Recommandations
Pour la classe DQN :

Définissez les couches (nn.Linear) dans le constructeur __init__.

Définissez le passage des données à travers ces couches dans la méthode forward, en utilisant une fonction d'activation comme F.relu entre elles.

Pour la classe ReplayBuffer :

Utilisez une collections.deque(maxlen=...) pour stocker efficacement les expériences avec une capacité fixe.

La méthode push ajoute un tuple d'expérience au buffer.

La méthode sample utilise random.sample() pour extraire un batch aléatoire.

Etape 2 Comprenez et assemblez la boucle d'entrainement DQN

Votre tâche n'est donc pas de réécrire la boucle, mais de l'analyser, de la comprendre et de la faire fonctionner. Lisez attentivement les commentaires pour saisir la logique de chaque bloc :

la sélection d'action,
le calcul des valeurs Q,
le calcul de la valeur Q cible (avec le target network),
le calcul de la perte
et l'étape d'optimisation.
 
Prérequis
Avoir défini les classes DQN et ReplayBuffer.


Résultat attendu 
L'exécution complète de la cellule d'entraînement du DQN, affichant la progression des récompenses tous les 50 épisodes, et un graphique final montrant la courbe des récompenses par épisode.

 

Recommandations 
Concentrez-vous sur la fonction optimize_model(). C'est là que la "magie" opère.

Repérez où le policy_net est utilisé pour prédire les Q-values des actions prises.

Repérez où le target_net est utilisé pour estimer la valeur maximale des états suivants. C'est un point clé.

Comprenez comment la loss est calculée entre ces deux quantités.

 
Points de vigilance  
Portez une attention particulière à la gestion des tenseurs PyTorch. Remarquez les conversions (torch.FloatTensor, torch.LongTensor) et les manipulations de forme (.unsqueeze(1), .gather(1, ...), .max(1)[0]).

Le target_net est mis à jour périodiquement en copiant les poids du policy_net. Trouvez la ligne de code qui effectue cette opération.

Outils 
Google colab

Gymnasium

Etape 3 Entrainez et évaluez avec Stable-Baselines3

Utilisez la classe DQN de Stable-Baselines3 pour entraîner, évaluer et sauvegarder un agent sur "CartPole-v1".

 
Prérequis
Avoir réalisé les étapes 1 et 2 pour apprécier la simplification.

Avoir installé stable-baselines3.


Résultats attendus 
Un modèle DQN.

L’avoir entraîné sur au moins 5000 pas (25 000, c’est mieux).

Récompense moyenne évaluée sur 100 épisodes.

Modèle sauvegardé (.zip).

 

Recommandations 
Importez DQN et evaluate_policy depuis stable_baselines3.

Instanciez : model = DQN('MlpPolicy', env, verbose=1, tensorboard_log="./logs/").

Entraînez : model.learn(total_timesteps=25000).

Évaluez : mean_reward, std_reward = evaluate_policy(model, env, n_eval_episodes=100).

Sauvegardez : model.save("dqn_cartpole").

 
Points de vigilance  
Distinguez le DQN manuel du DQN de SB3.

total_timesteps = nombre d’interactions avec l’environnement, ce n’est pas le nombre d’épisodes.

Outils
SB3

----------------

Mission - Pilotez l'atterrisseur lunaire Eagle - 1

Vous êtes un ingénieur Machine Learning junior spécialisé en apprentissage par renforcement dans l’entreprise AstroDynamics.

L’entreprise est un leader dans le développement de systèmes autonomes pour l'exploration spatiale.

Elle souhaite automatiser les procédures d'atterrissage automatique de ses modules afin d'améliorer la sécurité et d'optimiser la consommation de carburant, un facteur critique pour la durée des missions.

 

Vous êtes chargé de développer le pilote automatique pour le nouveau module d'atterrissage "Eagle - 1". Votre objectif est de garantir un atterrissage en douceur et précis sur une zone cible simulée.

 

Vous recevez un brief de mission de votre chef de projet R&D.

 

Vous dressez une liste de vos futurs livrables :

Un notebook Colab (.ipynb) propre et commenté, présentant votre démarche complète : 

exploration, 

choix de l'algorithme, 

entraînement, 

optimisation des hyperparamètres,

évaluation finale du modèle.

Une vidéo (.mp4) d'une durée de 20 à 30 secondes, montrant une performance réussie de votre pilote automatique.

Une API (code source) : Interface pour interagir avec le modèle RL.

Une interface graphique (GUI, code source) : Visualisation d’une partie jouée par l’agent.

Un tableau de bord (Looker Studio/Streamlit/Gradio) : Suivi interactif des performances de l’agent.

Etape 1 Explorez l'environnement et établissez une base de référence

Avant de chercher la performance, vous devez comprendre votre terrain de jeu et établir un point de départ. Familiarisez-vous avec l'environnement "LunarLander-v3" et entraînez un premier agent avec les paramètres par défaut pour mesurer une performance initiale.

 
Prérequis
Avoir réalisé les parties 1 à 4 du notebook, en particulier l'utilisation de Stable-Baselines3.

 
Résultat attendu 
Un premier modèle est entraîné et sa récompense moyenne est calculée et documentée dans le notebook.

 

Recommandations 
Commencer par explorer les espaces d'action et d'observation de LunarLander-v3 pour comprendre ce que l'agent peut faire et voir.

Adaptez l’algorithme en fonction de l’espace d’action : 

PPO pour les espaces de contrôle continus, 

DQN pour les discrets.

Utiliser la fonction evaluate_policy sur au moins 50 épisodes pour obtenir une mesure de performance de base fiable.

 

Points de vigilance 
Ne passez pas trop de temps à essayer d'obtenir un bon score à cette étape. 

L'objectif est de mettre en place rapidement un pipeline fonctionnel.

Etape 2 Optimisez les hyperparamètres de votre agent

C'est le cœur de votre mission. En vous basant sur votre performance initiale, vous devez maintenant expérimenter pour améliorer votre agent. L'objectif est de dépasser de manière constante une récompense moyenne de 200 points, le seuil d'un atterrissage réussi.

 
Prérequis 
Avoir établi un modèle de référence et sa performance à l'étape 1.

 
Résultat attendu  
Un agent entraîné qui atteint une récompense moyenne supérieure à 200 sur 100 épisodes d'évaluation.

 

Recommandations 
Ne modifier qu’un seul hyperparamètre à la fois pour isoler son effet (ex : learning_rate, gamma, n_steps).

Utiliser TensorBoard (ou équivalent) pour visualiser et comparer les courbes de récompense de vos différentes expériences.

Documenter chaque expérience dans votre notebook : quels paramètres ont été modifiés, quel a été le résultat ?

Sauvegarder les modèles qui montrent des signes prometteurs pour ne pas perdre votre travail.

 

Points de vigilance 
L'entraînement peut prendre du temps. Soyez patient et planifiez vos expériences.

Le score cible de 200 doit être une moyenne stable (avec un écart-type faible), pas un coup de chance sur un seul épisode.

Etape 3 Déployez une API, une interface graphique et un tableau de bord

Développez une API pour interagir avec le modèle RL, une interface graphique (GUI) pour visualiser une partie jouée, et un tableau de bord interactif pour suivre les performances de l’agent.

 
Prérequis 
Avoir optimisé un agent avec une récompense moyenne >200.

Avoir sauvegardé le meilleur modèle.


Résultats attendus  
Une API fonctionnelle, 

Un GUI affichant une partie jouée, 

Un tableau de bord interactif montrant les métriques de performance sur les différentes parties jouées.

 

Recommandations  
Développer une API avec FastAPI ou Flask : créer un endpoint (ex. : /play) acceptant un état et renvoyant une action.

Créer un GUI avec Streamlit ou Gradio pour visualiser une partie jouée par l’agent (ex. : animation de l’atterrissage).

Construire un tableau de bord (Looker Studio, Streamlit, ou Gradio) affichant les courbes de récompense, moyenne, écart-type, les décisions prises par l’IA en fonction du type de circonstance etc...

Documenter le code source et les instructions d’utilisation dans le notebook.

 

 

Points de vigilance  
Toute la logique RL doit être côté API (backend) et non côté GUI (frontend).

 Utilisez des générateurs ou des batchs plus petits en cas de saturation de la RAM.

 
Outils 
Streamlit

Gradio

Looker Studio

FastAPI