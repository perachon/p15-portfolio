Système de recommandation d’événements basé sur un RAG


Projet dont l’objectif est de concevoir et déployer un système de recommandation d’événements basé sur une architecture RAG, c’est-à-dire Retrieval-Augmented Generation

L’idée est de permettre à un utilisateur de poser une question en langage naturel, par exemple sur des événements à venir dans une zone géographique donnée, et d’obtenir une réponse pertinente, contextualisée et sourcée.

Le projet couvre toute la chaîne : depuis la préparation des données, la recherche vectorielle, jusqu’à l’exposition du système via une API et sa conteneurisation pour une démonstration locale.


----

Au niveau du contexte utilisateur
Il existe de nombreux événements disponibles, que ce soit des conférences, expositions, spectacles, etc .
Mais l’information est souvent dispersée, peu lisible, et difficile à exploiter efficacement.

Les moteurs de recherche classiques fonctionnent surtout par mots-clés, ce qui n’est pas toujours adapté à des questions naturelles comme : ‘qu’est-ce qu’il y a à faire ce week-end près de chez moi ?’

L’enjeu métier est donc de permettre une recherche plus intuitive, plus contextuelle, et surtout plus pertinente pour l’utilisateur final

---


L’objectif du projet est de proposer une solution qui permet à un utilisateur de poser une question libre, en langage naturel, et d’obtenir une réponse utile et fiable.

Contrairement à une simple recherche par mots-clés, le système doit être capable de comprendre l’intention de la question, de retrouver les événements pertinents, et de générer une réponse synthétique à partir de ces informations.

Un point important est aussi la traçabilité : les réponses sont basées sur des données identifiées et sourcées.

Enfin, le système est exposé via une API REST afin de pouvoir être testé facilement et intégré dans d’autres applications.

----

Principe d’un système RAG (Retrieval-Augmented Generation)

Un système RAG combine deux éléments complémentaires.

- D’abord, une phase de recherche : le système va retrouver, dans une base de documents, les éléments les plus pertinents par rapport à la question posée.
- Ensuite, une phase de génération : un modèle de langage utilise uniquement ces documents pour produire une réponse en langage naturel.

L’avantage principal, c’est que le modèle ne se base pas uniquement sur ses connaissances générales, mais sur des données spécifiques et contrôlées.
Cela permet d’obtenir des réponses plus fiables, plus pertinentes, et surtout traçables, puisqu’on peut identifier les sources utilisées.

On peut voir le RAG comme un assistant qui va d’abord chercher l’information avant de répondre.

----

Les données utilisées dans ce projet proviennent de la plateforme OpenAgenda et plus précisément d’un agenda particulier pris en exemple sur l’université Paris-Saclay, ça concerne des événements culturels, scientifiques et associatifs autour de la zone Paris-Saclay.

Chaque événement contient des informations structurées comme le titre, la description, la date de début et la localisation.

Avant d’être utilisées par le système RAG, ces données sont nettoyées et normalisées afin d’assurer une bonne qualité d’indexation et de recherche.

Le périmètre géographique est volontairement limité afin de garantir la cohérence des réponses et d’éviter des recommandations hors contexte

----

Une fois les données collectées et nettoyées, elles passent par un pipeline en plusieurs étapes.

- Les descriptions des événements sont d’abord découpées en segments de taille contrôlée pour améliorer la qualité de la recherche.
- Chaque segment est ensuite transformé en vecteur numérique à l’aide d’un modèle d’embeddings.
- Ces vecteurs sont stockés dans une base vectorielle FAISS, ce qui permet d’effectuer des recherches sémantiques rapides et efficaces au moment des requêtes utilisateur.
- Les documents retrouvés sont ensuite fournis au modèle de langage pour générer une réponse contextualisée.

----

Les données d’événements sont d’abord collectées et transformées en embeddings, puis stockées dans une base vectorielle FAISS locale.
Tout ça c’est OFFLINE, tout ce qui se passe avant en amont.

Et maintenant qu’est-ce qu’il se passe ONLINE, au moment où un utilisateur pose une question via l’API, la chaîne RAG commence par interroger FAISS pour récupérer les documents les plus pertinents.

Ces documents sont ensuite fournis au modèle de langage, qui génère une réponse contextualisée.
L’ensemble du système est exposé via une API FastAPI et conteneurisé avec Docker afin d’être exécutable localement en une seule commande.

Cette architecture permet de séparer clairement les responsabilités : données, recherche, génération et exposition API, et exploitation, tests et maintenance.

La recherche par similarité sémantique est effectuée par FAISS à partir des embeddings.Le modèle de langage Mistral n’intervient qu’après, uniquement pour générer la réponse à partir des documents récupérés.

----

Le système RAG est exposé via une API REST développée avec FastAPI.

FastAPI génère automatiquement une documentation interactive accessible via Swagger, ce qui facilite les tests et les démonstrations.

L’endpoint principal /ask permet de poser une question en langage naturel et de recevoir une réponse augmentée par les documents pertinents.

Un endpoint /rebuild permet de reconstruire la base vectorielle si les données évoluent, sans modifier le reste du système.

Cela permet aussi à des équipes non techniques de tester facilement la solution.

----

Afin de rendre le système facilement exécutable plus pour les équipes techniques, l’ensemble de l’application a été conteneurisé avec Docker.

Le conteneur inclut l’API FastAPI ainsi que l’accès à l’index FAISS stocké localement.

Le déploiement se fait via Docker Compose, ce qui permet de lancer l’API en une seule commande, sans configuration complexe.

Ca permet de tester le projet sur n’importe quelle machine disposant de Docker.

----

Le système a été testé à l’aide d’un jeu de questions représentatif, par exemple des recherches d’événements par thème ou par zone géographique.

Les tests permettent de vérifier que le système retourne des réponses pertinentes, cohérentes avec la question posée, et basées sur les documents indexés.

Un point important est la présence des sources dans les réponses, ce qui permet de vérifier et d’expliquer les résultats retournés par le système.

L’API a également été testée de bout en bout afin de garantir son bon fonctionnement.


----

Comme tout système basé sur un modèle de langage, cette solution présente certaines limites :

- La génération des réponses dépend d’un LLM externe, ce qui implique une dépendance à une connexion Internet et à un service tiers.
- La qualité des réponses est directement liée à la qualité et à la couverture des données indexées.
- Temps de réponse variable selon la requête.
- Enfin, le périmètre géographique est volontairement restreint afin de garantir des recommandations pertinentes et cohérentes.

Ces limites sont connues et assumées dans le cadre du projet.

----

Améliorations possibles

Plusieurs axes d’amélioration peuvent être envisagés pour faire évoluer ce système :

- Tout d’abord, les données pourraient être enrichies et mises à jour automatiquement afin de garantir des informations toujours à jour.
- Le mécanisme de retrieval pourrait être amélioré, par exemple en affinant les filtres ou en ajoutant une étape de re-ranking (affinement de la pertinence)
- Des métriques d’évaluation plus avancées pourraient également être intégrées pour suivre la qualité des réponses dans le temps, dans le sens de mesurer automatiquement si les réponses restent pertinentes et bien fondées sur les documents, et détecter toute dégradation de la qualité du système dans le temps.
- Enfin, l’API pourrait être sécurisée et déployée dans un environnement plus proche de la production.”

----


Pour conclure, ce projet met en place un système RAG complet permettant de rechercher et recommander des événements à partir de questions en langage naturel.

La solution combine une recherche sémantique via une base vectorielle FAISS et une génération de réponse contrôlée à l’aide d’un modèle de langage.

Le système est exposé via une API REST et conteneurisé avec Docker, ce qui le rend facilement exécutable et démontrable localement.

Je vais maintenant vous proposer une démonstration du système à travers quelques scénarios

----

DEMO (je crois pas avoir de possibilité en ligne => docker local)

Je vais maintenant montrer le fonctionnement du système à travers quelques scénarios concrets.

L’utilisateur interagit avec l’API en posant une question en langage naturel, par exemple sur des événements à venir dans une zone donnée.

Le système effectue alors une recherche sémantique dans la base FAISS, récupère les documents pertinents, puis génère une réponse contextualisée à l’aide du modèle de langage.

La démonstration se fait via l’interface Swagger ou via Postman, ce qui permet de tester facilement différents scénarios.
