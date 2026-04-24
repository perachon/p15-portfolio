(P7) Projet 7

Concevez et déployez un système RAG


Qu’allez-vous apprendre dans ce projet ?
 

Dans ce projet, vous allez continuer d'acquérir des compétences et réaliser un système Retrieval-Augmented Generation (RAG) fonctionnel basé sur LangChain et le modèle Mistral, soutenu par une base vectorielle Faiss.

 

Vous allez découvrir l'intégration de modèles NLP dans un système de type RAG. Vous utiliserez le framework LangChain pour gérer les interactions entre la base de données vectorielle et les modèles NLP.

 

Les intégrations permettront de réaliser un chatbot capable de fournir des réponses augmentées par la recherche dans une base de données vectorielle, améliorant ainsi l’expérience utilisateur en fournissant des informations précises et adaptées.

 

Vous plongerez ainsi dans les processus d’automatisation des flux de données via des scripts Python et la mise en place de tests continus pour suivre les performances du modèle et la qualité des données.

 

En quoi ces compétences sont-elles importantes pour votre carrière ? 
Ces compétences sont utiles pour le domaine de la science des données et de l'ingénierie des données, où la gestion efficace des flux de données et des modèles IA est essentielle. 

 

Elles permettent de concevoir et de déployer des systèmes d'extraction et de génération augmentée performants. Ces pratiques sont recherchées par les recruteurs en data engineering, machine learning et intelligence artificielle, notamment pour la gestion de pipelines de données, l’intégration de modèles NLP, et l’automatisation des processus de traitement des données.

 

Comment allez-vous procéder ?
Ce projet est découpé en 3 activités.

Cours : Vous suivrez le cours Mettez en place un RAG pour un LLM pour comprendre les enjeux de l'extraction augmentée et de l'intégration des modèles NLP avec les bases de données vectorielles.
Ressources pédagogiques :  Vous pourrez les consulter pour une bonne réalisation de votre projet. 
Mission : Vous réaliserez la conception et le déploiement d'un système RAG complet, intégrant des modèles Mistral avec LangChain.



Objectifs pédagogiques
Créer les processus de test
Exposer les résultats aux directions (via une API) en vue de leur exploitation
Identifier ou créer une API compatible et l’intégrer pour permettre l’accès aux résultats
Identifier ou créer un modèle d’apprentissage adapté aux contraintes et aux besoins métier


-----

Mission - Développez un assistant pour la recommandation d'évènements culturels

Prêt à mener la mission ?

Vous êtes data scientist freelance, spécialisé dans le traitement du langage naturel et la création de systèmes intelligents. Vous intervenez en mission pour Puls-Events, une entreprise technologique qui développe une plateforme de recommandations culturelles personnalisées.

Puls-Events souhaite tester un nouveau chatbot intelligent capable de répondre à des questions utilisateurs sur les événements culturels à venir, en s'appuyant sur un système RAG (Retrieval-Augmented Generation) combinant recherche vectorielle et génération de réponse en langage naturel.

 

Votre mission est de livrer un POC (Proof of Concept) complet, avec une API exploitable par les équipes produit et marketing. Ce POC devra démontrer la faisabilité technique, la pertinence métier et la performance du système.

 

Vous recevez un message du responsable technique de Puls-Events : 

 

De : Jérémy

À : moi

Objet : Version fonctionnelle du POC RAG

Salut,

Comme convenu, on a besoin que tu finalises une version fonctionnelle du POC pour notre assistant intelligent.

L’objectif est de démontrer aux équipes produit et marketing que notre plateforme peut intégrer un chatbot capable de répondre aux questions des utilisateurs en s’appuyant sur les données d’événements disponibles via l’API Open Agenda.

Pour ce POC, tu peux cibler une zone géographique de ton choix, tant que les événements sélectionnés sont récents c’est  à dire de moins d’un an.

Voici ce que nous attendons de ta part :

 

Un système RAG fonctionnel intégrant LangChain, Mistral et Faiss, avec des scripts permettant la reconstruction de l’index vectoriel à partir des données.

Une API REST (FastAPI ou Flask) exposant le système : elle doit permettre d’envoyer une question et de recevoir une réponse augmentée, générée à partir des données vectorisées.

Un rapport technique (PDF ou README) documentant l’architecture, les choix technologiques, les modèles utilisés, les résultats observés et les pistes d’amélioration. Je te rajoute un template en pièce jointe. 

Un jeu de test annoté (questions/réponses de référence) pour mesurer la qualité des réponses générées.

Des tests unitaires pour valider l’indexation des données et les performances du système et l'automatisation des métriques d’évaluation.  

Tu peux organiser ton dépôt GitHub de manière claire, avec des dossiers pour les scripts, les tests, l’API, et la documentation.

Le but est de pouvoir tester rapidement ta solution.

Nous attendons donc une démo live en réunion avec un exemple d’utilisation de l’API. Tu pourras conteneuriser le endpoint dans une image Docker et exécuter cette image en local pour la démonstration que le système est prêt pour un déploiement élargi. 

Appuie-toi sur une présentation PowerPoint (10 à 15 slides) pour exposer les objectifs, le fonctionnement du système, les résultats, et les perspectives.

Merci d’avance,

 

Jérémy

Responsable technique Puls-Events