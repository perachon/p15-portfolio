(P12) Projet 12

Extrayez des données multimodales de sites web




Qu’allez-vous apprendre dans ce projet ?
 

Dans ce projet, vous allez découvrir une compétence essentielle : l’industrialisation des traitements de données. 

Vous découvrirez comment automatiser l’extraction de données multimodales (texte, images), en les récupérant depuis des sources variées comme des APIs ou des datasets, tout en assurant leur pertinence pour entraîner un modèle d’IA.

En parallèle, vous transformerez des données brutes en formats exploitables, en apprenant à nettoyer, normaliser et structurer ces données pour qu’elles soient prêtes à l’emploi. 

Vous maîtriserez également l’orchestration de flux ETL avec Apache Airflow, en configurant des tâches planifiées avec dépendances pour garantir un pipeline fluide et automatisé. 

Enfin, vous explorerez les notions de schéma de données, de reproductibilité et de planification, afin de construire un système robuste et maintenable.

En créant un dataset bien structuré, vous comprendrez pourquoi cette étape est cruciale pour entraîner des modèles d’IA performants. Un dataset de qualité, constamment mis à jour grâce à l’automatisation, permet de garantir que le modèle reste pertinent face à l’évolution des données. 

Grâce à ce projet, vous serez capable d’automatiser l’ensemble du parcours d’une donnée, depuis son extraction jusqu’à son stockage, tout en assurant sa maintenabilité et son actualité, des aspects clés pour des systèmes d’IA en production.

 
En quoi ces compétences sont-elles importantes pour votre carrière ?
Ces compétences sont au cœur des métiers de la data engineering et du machine learning, notamment dans des domaines comme la classification de contenus ou l’analyse prédictive. En maîtrisant la création de datasets automatisés, vous répondrez à un besoin critique des entreprises : fournir des données fiables et à jour pour entraîner des modèles d’IA performants. Un pipeline ETL robuste, avec une gestion rigoureuse des erreurs, des logs détaillés et une structure modulaire, garantit non seulement la fiabilité des données, mais aussi la scalabilité des systèmes, essentielle pour gérer de grands volumes de données. 

Ces pratiques, très demandées en entreprise, vous permettront de contribuer efficacement à des projets d’envergure, tout en démontrant votre autonomie. En somme, savoir concevoir et maintenir un pipeline automatisé vous positionnera comme un atout précieux dans la construction de solutions d’IA évolutives et performantes.

 

Comment allez-vous procéder ?
Ce projet est découpé en 2 activités principales :

Ressource pédagogique : Vous consulterez une ressource sur Apache Airflow.

Mission : Vous construirez un script d’extraction automatisée à destination d’un système de détection de "fake news".

Vous terminerez en complétant la fiche d’autoévaluation, qui servira de base de discussion avec votre mentor avant la session de bilan.

Cela vous permettra de montrer que vous êtes prêt à travailler sur des projets concrets d’ingestion de données en IA.

 
Prêt à démarrer votre projet ?
Lancez-vous dans la première section Ressource pédagogique.

À l’issue de ce projet, vous aurez une session de bilan avec votre mentor pour discuter de votre projet.

Objectifs pédagogiques
Charger des données afin de les stocker dans un emplacement adapté
Définir des indicateurs de performance pertinents
Extraire des données issues de toutes sources confondues pour les traiter ou les déplacer
Transformer des données afin de les adapter à leur utilisation finale


Mission - Concevez une stratégie d'extraction de données multimodales pour entrainer un detecteur de fake news

Vous êtes Ingénieur Data junior chez CheckIt.AI, une start-up qui développe des outils de détection automatique de désinformation.

CheckItAI conçoit des solutions d’intelligence artificielle pour lutter contre les fake news. Pour enrichir son moteur d’analyse, elle souhaite créer un pipeline d’acquisition de données multimodales (texte + image).

Votre mission: créer un système d’extraction automatisée permettant de récupérer des publications (articles ou posts) incluant à la fois du texte et des images, à partir d’une ou plusieurs sources accessibles (API, site web…).

 

Etape 1 Explorez et qualifiez les sources de données
Identifiez au moins 3 (plus étant mieux) sources multimodales (bases de données, APIs, réseaux sociaux, images, vidéos, PDF) pertinentes pour entraîner un détecteur de fake news. Pour chaque source, décrivez le type de données (texte, image), le format (CSV, JSON, API), la langue, la qualité des labels (vrai/faux) et proposez une méthode d’extraction (API REST, Scrapy, Selenium).

Prérequis
Avoir les bases en appel d’API et scrapping via python pour récupérer texte + image.

Avoir identifié des cas typiques de fake news multimodales.

Résultat attendu
Rapport d’exploration de sources (Markdown ou PDF) : Document listant les sources identifiées, leurs caractéristiques (format, langue, labels, modalités) et les méthodes d’extraction proposées.

Recommandations
Identifier les éléments indispensables à chaque publication (ex. : texte, image, date).

Explorez des sources comme News API, Reddit, Kaggle ainsi que les sites de news que vous lisez habituellement.

N’oubliez pas les flux RSS.

Vérifiez les droits d’usage et la fiabilité des labels.

Préférez les canaux officiels en premier lieu, le scrapping étant une pratique peu appréciée par les détenteurs d’un site web…

Définir le format de sortie (JSON, CSV, parquet…)

Points de vigilance
Ne pas confondre opinions controversées et désinformation.

opinions controversées : points de vue ou jugements qui divisent l’opinion publique. Elles peuvent choquer, heurter ou être en désaccord avec les normes sociales ou scientifiques établies, mais elles relèvent d’une liberté d’expression. Caractéristiques : Relèvent du subjectif.

désinformation : fausses informations diffusées intentionnellement pour tromper, manipuler, semer le doute ou nuire. Caractéristiques : Objectivement fausses.

Ne pas ignorer les champs secondaires utiles (ex. : URL, fiabilité déclarée, nom de domaine)

Vérifier que les champs image et texte sont bien associés dans la même entrée.

Outils
Google Dataset Search, 

Hugging Face Datasets, 

Kaggle, 

public-apis.io.

Ressources
Article “Multimodal Fake News Detection: A Survey”.

Dataset FakeNewsNet

API NewsData.io

Etape 2 Développez des scripts d'extraction automatisée

Créez des scripts Python pour extraire des données multimodales (texte + image) à partir d’une ou plusieurs sources sélectionnées. Les scripts doivent être modulaires, exécutables sans intervention manuelle. Sauvegardez les données dans une structure cohérente (JSON, CSV).

 

Prérequis
 

Avoir défini les données ciblées (étape 1)

 

Résultat attendu
Scripts d’extraction automatisée (.py ou .ipynb) pour extraire des données structurées/semi-structurées.

Recommandations
Vérifier la présence de liens d’images exploitables (format, accessibilité).

Tester les réponses JSON ou HTML pour en valider la faisabilité.

Structurez le code en fonctions : connexion, parsing, nettoyage, sauvegarde.

Ajoutez des logs, une gestion des erreurs (try/except) et des paramètres configurables.

Points de vigilance
Ne pas ignorer les limites d’appel des APIs (quotas, clé d’accès).

Vous assurer de la légalité du scraping si vous ciblez un site web.

Si vous faites appel à l’IA, prenez le temps de reconstruire et de comprendre votre code. Vous devrez être capable d’expliquer votre cheminement et vos décisions techniques. Décortiquez, reformulez, expliquez votre code à votre mentor lors des sessions, cela favorisera votre apprentissage.

Outils possibles
Beautiful Soup

Selenium

Scrapy

Requests

Feedparser

Etape 3 Implémentez un pipeline de transformation

Développez un pipeline Python modulaire pour transformer les données extraites en un format exploitable (nettoyage, normalisation, mapping, génération de colonnes).

Le pipeline doit être reproductible et journalisé. Vous documenterez ensuite les données extraites à l’aide d’un schéma conceptuel. 

Le schéma doit préciser les champs (ex. : titre, contenu, URL image, métadonnées), leurs types, et leur rôle dans le cas d’usage (classification, NLP).

Prérequis
Avoir extrait des données brutes (Étape 2).

 

Résultats attendus
Pipeline de transformation reproductible (.py ou .ipynb) : Script exécutable appliquant nettoyage, renommage, vérifications, et exportation des données.

Schéma de données finalisé (PDF ou Mermaid) : Modèle conceptuel décrivant les champs, types et relations, aligné sur l’usage IA.

Recommandations
Structurez en étapes :

lecture,

traitement,

export.

Modularisez les fonctions (ex. : nettoie_texte(), valide_image()).

Utilisez Logging pour journaliser chaque transformation.

Utilisez Mermaid ou draw.io pour visualiser les colonnes et relations.

Points de vigilance
Ne pas confondre schéma technique (SQL) et modèle conceptuel.

Le schéma physique des données, également connu sous le nom de modèle technique, décrit précisément la manière dont les données sont stockées et structurées dans un système informatique. Il se concentre sur les détails techniques tels que les tables, les colonnes, les types de données, les index, les clés primaires et étrangères, et est spécifique à une technologie ou une plateforme particulière, comme un système de gestion de base de données (SGBD).

En revanche, le modèle conceptuel des données représente la signification des données dans un langage métier, indépendant de toute considération technique. Il se concentre sur les entités, les relations, les attributs et les contraintes d'intégrité, fournissant une vue abstraite et compréhensible des données pour les parties prenantes, sans se soucier de la manière dont elles seront physiquement stockées ou gérées.

Ne pas confondre exploration et transformation.

Vérifier l’association texte-image.

Etape 4 - Orchestrezz le pipeline avec Airflow

Créez un flux ETL avec Apache Airflow pour automatiser l’extraction, la transformation et le chargement des données multimodales dans une base adaptée (SQL/NoSQL). Le DAG doit inclure des tâches distinctes pour chaque étape et être exécutable en local.

 

Prérequis
Avoir finalisé les scripts d’extraction et de transformation (Étapes 2 et 3).

Avoir un environnement Airflow fonctionnel (avoir terminé un des guides d’installation)

Résultat attendu
Démonstration de flux ETL (Airflow) (.py) : DAG exécutable simulant un flux ETL, avec preuves d’exécution (logs, captures d’écran Airflow UI).

Recommandations
Reprendre les fonctions de votre script de l’étape 3 et les inclure directement dans le DAG.

Points de vigilance
Bien importer les opérateurs nécessaires.

Évitez les tâches trop longues ou non modulaires.

Utiliser desPythonOperatorles plus simples possible.

Le choix du type de base de données est-il pertinent ?

Assurez-vous que la base de données est sécurisée: 

méthode d'authentification mise en place, 

limiter les accès aux rôles autorisés, 

chiffrage des données sensibles.

Outils
Python

Airflow

Ressources
Airflow Python package Doc

Quickstart : Apache Airflow avec Docker

Tuto DAG PythonOperator

Building an ETL Pipeline with Airflow

Etape 5 Evaluez et visualisez les performances

Définissez des indicateurs de performance (KPI) pour évaluer l’efficacité du pipeline (précision des données, rapidité, coût). 

Créez un tableau de bord pour visualiser ces KPI et un plan de monitoring pour suivre le pipeline en production.

 

Prérequis
Avoir finalisé le pipeline ETL (Étape 4).

Résultats attendus
Tableau de bord KPI de l’ETL (.py ou .ipynb) : Visualisation des KPI (ex. : % de données valides, temps d’exécution).

Plan de monitoring (Markdown ou PDF) : Stratégie pour surveiller le pipeline (logs, alertes, fréquences de vérification).

Recommandations
Mesurez la précision (ex. : % d’entrées valides), la rapidité (temps par tâche), et le coût (ressources).

Regardez les librairies graphiques recommandées par Streamlit.

Incluez dans le plan de monitoring : seuils d’alerte, gestion des erreurs, fréquence des vérifications.

Points de vigilance
Assurez-vous que le tableau de bord est lisible pour un public non technique.

Vérifiez que le plan de monitoring est en accord avec vos automatisations.

Outils
Streamlit

Ressources
Streamlit Doc

Streamlit: vos premiers pas.