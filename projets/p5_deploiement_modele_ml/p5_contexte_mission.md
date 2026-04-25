(P5) Projet 5

Déployez un modèle de Machine Learning


Qu’allez-vous apprendre dans ce projet ?
 

Dans ce projet, vous allez consolider vos compétences en déployant un modèle dans un environnement prêt pour la production.

Vous allez découvrir :  

le développement d'API avec FastAPI, 

les tests unitaires avec Pytest, 

et la gestion des versions avec Git. 

Ces notions sont essentielles pour déployer des modèles de machine learning dans un contexte professionnel. Vous allez également vous plonger dans la conteneurisation et les pipelines CI/CD pour garantir que votre modèle est évolutif et maintenable.

En quoi ces compétences sont-elles importantes pour votre carrière ? 
Le déploiement de modèles de machine learning est une compétence cruciale pour les data scientists et les ingénieurs ML. Ces pratiques sont très recherchées par les recruteurs dans des domaines comme la science des données, l'ingénierie machine learning, et le développement logiciel. Maîtriser ces compétences fera de vous un collaborateur précieux dans toute équipe travaillant sur des projets AI/ML, car elles démontrent votre capacité à transformer des modèles théoriques en solutions concrètes et opérationnelles.

Comment allez-vous procéder ? 
Ce projet est découpé en 4 activités.

Cours : Vous suivrez deux cours.

Gérez du code avec Git et GitHub pour vous familiariser avec les commandes de base de Git et apprendre à corriger les erreurs courantes sur GitHub.

Devenez un expert de Git et GitHub pour comprendre les enjeux de la collaboration efficace et de la gestion de versions dans un environnement de développement moderne.

Ressource pédagogique : 

Vous consulterez la ressource pédagogique Hugging Face Spaces Overview pour découvrir comment déployer rapidement des modèles de machine learning et les rendre accessibles via une API, tout en utilisant des outils de versioning comme Git.

Mission : Vous réaliserez la mission principale. 

À l’issue de ce projet, vous présenterez les livrables de la mission à un mentor évaluateur lors d’une soutenance.

Cela vous permettra de valider les compétences visées par ce projet.

Objectifs pédagogiques
Configurer l'environnement de travail
Définir et formaliser les processus de traitement et de stockage des données
Établir et exécuter un processus de test du SGDB
Installer et paramétrer un système de gestion de base de données et un outil d’extraction
Mettre en place un système d'authentification afin de garantir la sécurité des données
Modéliser une infrastructure compatible avec le SI
Structurer l’architecture des données et concevoir des BDD

-------

Mission - Déployez votre modèle de Machine Learning

Comment allez-vous procéder ?

Vous êtes freelance spécialisé en machine learning et vous venez de recevoir une demande de la part de votre client Futurisys, une entreprise innovante qui souhaite rendre ses modèles de machine learning opérationnels et accessibles via une API performante. Vous êtes chargé de déployer un modèle de machine learning en production.

 

Le directeur technique de Futurisys, Aurélien, vous formule une demande impliquant de : 

créer une API avec FastAPI (ou équivalent) pour exposer le modèle ;

écrire des tests unitaires avec Pytest pour garantir sa fiabilité ; 

et gérer la version du code avec Git pour une collaboration fluide.

 

L'objectif ? Rendre le modèle utilisable en production tout en respectant les meilleures pratiques de l'ingénierie logicielle. À la fin du projet, vous aurez un Proof of Concept (POC) fonctionnel dont vous pourrez être fier !

 

Tout en réfléchissant, vous dressez la liste des livrables que vous aurez à construire et présenter :

Un dépôt Git structuré contenant :

L'ensemble du code source.

Un requirements.txt (ou équivalent).

Un historique de commits clair, avec des branches dédiées aux fonctionnalités et l’utilisation de tags pour la gestion des versions.

Un README complet présentant le projet, notamment les instructions d’installation, d’utilisation, de déploiement, d’authentification et de sécurisation.

Une API fonctionnelle et déployée développée avec FastAPI (ou équivalent) exposant le modèle de machine learning accompagnée d’une documentation intégrée (par exemple via Swagger/OpenAPI) pour décrire les endpoints, les schémas de données et les exemples d’appels.

Des scripts de tests unitaires et fonctionnels associés à : 

Un ensemble de tests écrits en Pytest couvrant les cas critiques et les scénarios d’erreur.

Un rapport de couverture de tests (par exemple via pytest-cov) afin de démontrer la robustesse du code.

Une base de données PostgreSQL fonctionnelle:

Un script SQL (.sql) ou Python (create_db.py) pour la création de la base de données et des tables.

Un modèle de données/de la documentation expliquant la structure des tables (ex: schéma UML).

Des exemples d’entrées en base (SQL ou CSV contenant des inputs et outputs du modèle de ML).

Des scripts pour interroger les données et interagir avec le modèle ML.

Une configuration du pipeline CI/CD capable de gérer les environnements  (dev test, prod) et intégrer la gestion des secrets. 

Un fichier YAML (par exemple pour GitHub Actions) qui automatise les tests et le déploiement

Vous planifiez dans votre agenda de présenter l’ensemble de votre projet à Aurélien, à l’aide d’un support de présentation, et vous vous mettez au travail.