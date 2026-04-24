Pour information vérifie d'abord dans la partie 2 mais si tu n'as pas les informations manquantes TBD alors tu peux check la partie 1


(P8) Projet 8 - Partie 2

Confirmez vos compétences en MLOps (Partie 2/2)

Qu’allez-vous apprendre dans ce projet ?

Vous avez découvert l'importance de la gestion du cycle de vie d'un modèle ML et pratiqué son initialisation avec MLflow, dans le projet “Initiez-vous au MLOps”.

Vous allez maintenant consolider vos compétences en MLOps en vous concentrant sur le déploiement et le suivi en production de votre modèle de scoring. Votre objectif sera de concevoir un système de suivi du cycle de vie du modèle d'apprentissage. Vous allez découvrir et pratiquer la mise en place d'une API pour servir le modèle, les stratégies de déploiement (par exemple, sur le cloud), et l'importance du monitoring, essentiel pour garantir la performance et la fiabilité du modèle dans le temps. Vous plongerez aussi dans les concepts d'automatisation du pipeline MLOps.
 

En quoi ces compétences sont-elles importantes pour votre carrière ? 
 

La capacité à déployer et à maintenir des modèles de Machine Learning en production est cruciale. Maîtriser le déploiement via API, comprendre les enjeux du monitoring continu, et savoir automatiser les pipelines sont des compétences fondamentales pour tout Data Scientist ou Ingénieur IA désireux d'avoir un impact concret. 

Ces pratiques assurent la pérennité, la scalabilité, et la fiabilité des solutions IA. Ces compétences sont très recherchées par les recruteurs pour des rôles de Data Scientist, ML Engineer et MLOps Engineer.


Comment allez-vous procéder ? 
 

Ce projet est découpé en 2 activités :

Les ressources : Vous découvrirez 2 frameworks populaires pour créer des interfaces web de démonstration en python: Streamlit et Gradio. Le but est d’exposer les résultats de votre travail de façon interactive.

La mission : Vous réaliserez le déploiement de votre modèle de scoring via une API, mettrez en place un système de monitoring de base et réfléchirez à son automatisation. 


Objectifs pédagogiques

Prendre vos premiers pas en ML Engineering
Concevoir un système de suivi du cycle de vie du modèle d'apprentissage

------------

Mission - Déployez et monitorez votre modèle de scoring

Comment allez-vous procéder ? 

Cette mission simule la mise en production d'un modèle de scoring. Suivez les étapes pour réaliser vos livrables. Avant de démarrer, lisez attentivement la mission, consultez les étapes, et préparez vos questions pour la session de mentorat.


Prêt à mener la mission ? 

Vous êtes Data Scientist dans l'entreprise "Prêt à Dépenser". Après avoir développé et versionné un modèle de scoring (Projet Initiez-vous au MLops), vous recevez un message Slack de Chloé Dubois, la Lead Data Scientist :

" Salut ! Excellents résultats sur la dernière version du modèle de scoring ! Le département 'Crédit Express' est très impatient de l'utiliser pour traiter les nouvelles demandes en quasi temps réel. Il nous faut absolument une API fonctionnelle et déployable (Docker Ready!) d'ici la fin de la semaine prochaine. Peux-tu prioriser ça ?  On a aussi besoin d'un dashboard ou rapport de suivi pour vérifier que tout se passe bien une fois en prod (distribution des scores, temps de réponse, ce genre de choses). Tiens-moi au courant de ton plan d'action ! Merci ! "
 

Vous voila donc chargé de piloter la mise en production effective du modèle de scoring. Cela inclut la création d'une API robuste, la conteneurisation pour un déploiement fluide, et la mise en place d'un monitoring proactif pour garantir la performance et la fiabilité du modèle dans le temps.


En structurant vos pensées et en préparant votre to do list, vous rédigez la liste des livrables que vous allez concevoir et présenter à Chloé : 

1. Un historique des versions retraçant la construction du projet que vous rendrez disponible dans votre github en consultant la liste des commits. 

2. Des scripts : 

	- Une API fonctionnelle (vous travaillerez probablement avec Gradio ou FastAPI) qui prend les données d'un client en entrée et retourne un score de prédiction. 
	- Des tests unitaires automatisés.

3. Un dockerfile pour la conteneurisation du code.

4. Une analyse du Data Drift: 

	- Un tableau de bord ou un rapport de monitoring (vous savez que vous pourrez le simuler dans un notebook ou via un outil comme Streamlit voire Dash) montrant des métriques clés (ex.: distribution des scores prédits, latence de l'API, temps d’inférence, etc.)
	- Des screenshots de la solution de stockage des données de production.

5. Un pipeline CI/CD: un fichier YAML (ou équivalent) démontrant l’automatisation de la mise en production et des tests lors d’un push sur la branche principale (à minima) du projet.

6. Une documentation README expliquant comment lancer l'API et interpréter le monitoring.




----------

(P8) Projet 8 - Partie 1 

Initiez-vous au MLOps (partie 1/2)

Qu'allez-vous apprendre dans ce projet ?
Ce projet introduit un problème de modélisation supervisée via une classification binaire. Vous allez vous concentrer sur l’apprentissage d’une compétence centrale : la gestion du cycle de vie d’un modèle de ML. 

 

En effet, après une très brève analyse exploratoire, vous allez entamer vos travaux de modélisation en utilisant un outil de versionning des modèles comme MLFlow, un outil qui a pris une place centrale dans la communauté Data Science. 

 

Pourquoi ces compétences sont-elles importantes pour votre carrière ?
Utiliser MLflow vous permet de documenter chaque expérience, assurant rigueur scientifique et fiabilité des travaux.

Un Data Scientist ou un AI Engineer doivent tester rapidement différentes configurations d'hyperparamètres, d'architectures de modèles, de méthodes de préparation de données.

Automatiser et organiser ces processus :

accélère la recherche de solutions optimales,

réduit le risque d'erreurs dans les essais,

facilite la comparaison entre plusieurs approches.

Ces compétences permettent de :

packager les modèles de manière standard,
simplifier leur déploiement,
assurer un suivi en production
centraliser les informations
répondre à un standard attendu sur le marché.
Cela renforce votre employabilité et vous ouvre des perspectives de carrière plus avancées.

 

Comment allez-vous procéder ? 
Ce projet est découpé en 2 activités.

Ressource pédagogique : Vous consulterez la ressource ML Flow.

Exercice guidé : vous travaillez sur un cas guidé de scoring de crédit avec un dataset fourni. Cette option vous permet de vous exercer dans un cadre structuré, proche d’un cas réel, avec des données simulées.

- préparer et enrichir un jeu de données,
- entraîner et comparer différents modèles,
- tracer vos expérimentations avec un outil de suivi adapté,
- optimiser vos hyperparamètres et ajuster vos seuils de décision,
- justifier vos choix en lien avec des enjeux métiers.

Objectifs pédagogiques
Gérer l'overfitting et l'underfitting avec les techniques adéquates
Identifier les hyperparamètres
Optimiser les fonctions d'activation
Surveiller la qualité des features


Prêt à mener la mission ?

Vous êtes Data Scientist au sein d'une société financière, nommée "Prêt à dépenser", qui propose des crédits à la consommation pour des personnes ayant peu ou pas du tout d'historique de prêt.
 

L’entreprise souhaite mettre en œuvre un outil de “scoring crédit” pour calculer la probabilité qu’un client rembourse son crédit, puis classifie la demande en crédit accordé ou refusé. Elle souhaite donc développer un algorithme de classification en s’appuyant sur des sources de données variées (données comportementales, données provenant d'autres institutions financières, etc.)


Votre mission :

Construire et optimiser un modèle de scoring qui donnera une prédiction sur la probabilité de faillite d'un client de façon automatique.

Analyser les features qui contribuent le plus au modèle, d’une manière générale (feature importance globale) et au niveau d’un client (feature importance locale), afin, dans un soucis de transparence, de permettre à un chargé d’études de mieux comprendre le score attribué par le modèle.

Mettre en œuvre une approche globale MLOps de bout en bout, du tracking des expérimentations à la pré-production du modèle.

Michaël, votre manager, vous incite à sélectionner un ou des kernels Kaggle pour vous faciliter l’analyse exploratoire, la préparation des données et le feature engineering nécessaires à l’élaboration du modèle de scoring. 


De : Michaël

À : moi

Objet : Besoins et conseils pour l’élaboration d’un outil de credit scoring 

Bonjour,

 

Afin de pouvoir faire évoluer régulièrement le modèle, je souhaite tester la mise en œuvre une démarche de type MLOps d’automatisation et d’industrialisation de la gestion du cycle de vie du modèle. 

 

Vous trouverez en pièce jointe la liste d’outils à utiliser pour créer une plateforme MLOps qui s’appuie sur des outils Open Source. 

 

Je souhaite que vous puissiez mettre en oeuvre au minimum les étapes orientées MLOps suivantes : 

Dans le notebook d’entraînement des modèles, générer à l’aide de MLFlow un tracking d'expérimentations.

Lancer l’interface web “UI MLFlow" d'affichage des résultats du tracking.

Réaliser avec MLFlow un stockage centralisé des modèles dans un “model registry”.

Tester le serving MLFlow.

 

J’ai également rassemblé des conseils pour vous aider à vous lancer dans ce projet !

 

Concernant l’élaboration du modèle soyez vigilant sur deux points spécifiques au contexte métier : 

Le déséquilibre entre le nombre de bons et de moins bons clients doit être pris en compte pour élaborer un modèle pertinent, avec une méthode au choix.

Le déséquilibre du coût métier entre un faux négatif (FN - mauvais client prédit bon client : donc crédit accordé et perte en capital) et un faux positif (FP - bon client prédit mauvais : donc refus crédit et manque à gagner en marge).

Vous pourrez supposer, par exemple, que le coût d’un FN est dix fois supérieur au coût d’un FP.

Vous créerez un score “métier” (minimisation du coût d’erreur de prédiction des FN et FP) pour comparer les modèles, afin de choisir le meilleur modèle et ses meilleurs hyperparamètres. Attention cette minimisation du coût métier doit passer par l’optimisation du seuil qui détermine, à partir d’une probabilité, la classe 0 ou 1 (un “predict” suppose un seuil à 0.5 qui n’est pas forcément l’optimum).

En parallèle, maintenez pour comparaison et contrôle des mesures plus techniques, telles que l’AUC et l’accuracy.

D’autre part je souhaite que vous mettiez en œuvre une démarche d’élaboration des modèles avec Cross-Validation et optimisation des hyperparamètres, via GridsearchCV ou équivalent.

Un dernier conseil : si vous obtenez des scores supérieurs au 1er du challenge Kaggle (AUC > 0.82), posez-vous la question si vous n’avez pas de l’overfitting dans votre modèle !

 

Bon courage !

 

Michaël