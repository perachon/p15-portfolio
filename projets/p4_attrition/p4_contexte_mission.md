(P4) Projet 4

Classifiez automatiquement des informations


Qu’allez-vous apprendre dans ce projet ?
 

Ce nouveau projet va se concentrer sur la classification supervisée, tout en consolidant davantage vos compétences en analyse exploratoire et en Machine Learning (préparation de la donnée, statistiques descriptives, feature engineering, évaluation des modèles). Vous aurez également une initiation au langage SQL en fin de projet. 

Dans un premier temps, vous allez découvrir comment réaliser un modèle de classification supervisée et comment l’évaluer avec des métriques simples (rappel, précision, matrice de confusion). Ensuite, vous allez peaufiner votre approche de modélisation en découvrant comment composer avec un déséquilibre des classes et avec des méthodes d’évaluation plus poussées. 

En quoi ces compétences sont-elles importantes pour votre carrière ?
La classification supervisée et la régression supervisée sont les deux faces de la même pièce qu’est l’apprentissage supervisé. Celui-ci est omniprésent dans les projets de Machine Learning en entreprise.

Comment allez-vous procéder ?
Ce projet est découpé en 4 activités dont les 2 dernières sont optionnelles. Vous ne serez donc évalué que sur la partie 1 de la mission.

Cours : Vous reprendrez éventuellement certains chapitres du cours "Initiez-vous au Machine Learning" que vous avez vu lors du projet précédent. Vous allez également vous focaliser sur certains chapitres du cours "Maîtrisez l’apprentissage supervisé".

Mission - partie 1 : 

Vous allez réaliser votre première approche de classification supervisée dans un contexte RH. 

Vous terminerez en complétant la fiche d’autoévaluation qui servira de base de discussion avec votre mentor avant la soutenance. 

Optionnel :

Cours : "Requêtez une base de données SQL"
Mission - partie 2 : À partir du même contexte projet, initiez-vous à la gestion des bases de données et au langage SQL.
À l’issue de ce projet, vous présenterez les livrables de la première partie de la mission à un mentor évaluateur lors d’une soutenance.

Cela vous permettra de valider les compétences visées par ce projet.


 

Objectifs pédagogiques
Configurer l’environnement de travail nécessaire à l’exploitation des données
Entraîner un modèle d’apprentissage
Évaluer le modèle d'apprentissage
Mettre en place un processus de nettoyage afin d’améliorer la qualité des données
Préparer et transformer des données afin de les adapter au modèle d’apprentissage.

----

Mission  - Identifiez les causes d'attrition au sein d'une ESN

Vous êtes mandaté en tant que Consultant Data Scientist par le département RH de votre client. Il s’agit de l'ESN TechNova Partners, spécialisée dans le conseil en transformation digitale et la vente d’applications en SaaS.

Ils font face à un turnover plus élevé que d'habitude et ils souhaitent identifier les causes racines potentielles derrière ces démissions.

Suite à votre onboarding, vous avez reçu ce mail de la part du responsable SIRH :

De : Augustin Moser

À : Moi

Objet : [HR Analytics] Analyse des causes de démission chez TechNova Partners

Bonjour,

Je te souhaite la bienvenue chez TechNova, on attendait ton arrivée avec impatience ! Comme tu le sais, nous faisons face à un taux de démission particulièrement élevé en ce moment et nous voulons en identifier les causes potentielles, ainsi que des leviers actionnables pour remédier à cela.

Après échange avec les employés démissionnaires, nous avons quelques intuitions concernant les causes, mais nous sommes convaincus qu’il faut passer par une analyse des données pour avoir une vision objective et exhaustive de la situation.

Pour te faciliter les choses, nous avons compilé 3 fichiers de données, chacun issu d’une source différente :

Un extrait de notre système SIRH où l’on va trouver des informations sur la fonction qu’occupe un employé chez nous, son âge, salaire, ancienneté etc.) ainsi que ses informations sociodémographiques.

Un extrait d’un autre SI que l’on utilise spécifiquement pour les évaluations annuelles de performance. On y retrouve des informations telles que les notes de ces évaluations et des notes de satisfaction données par les employés.

Enfin, nous demandons à tous nos employés de remplir annuellement un sondage qui nous aide à mettre en place des actions pour leur bien-être. Voici à disposition l’extrait du sondage le plus récent. Nous y avons ajouté un témoin pour indiquer si l’employé a quitté l’entreprise ou non.

A très bientôt pour un premier point d’étape.

Cordialement,

Augustin Moser

Responsable SIRH – TechNova Partners

Après avoir pris connaissance du mail d’Augustin, vous en recevez un autre de la part d’Amandine votre Manager Data Scientist :

De : Amandine Rousset

À : Moi

Objet : RE : [HR Analytics] Analyse des causes de démission chez TechNova Partners

Hello,

Comme tu peux le voir, l’attente est assez élevée du côté des RH ! Essayons de décomposer ensemble la demande d’Augustin en étapes d’analyse pour répondre à leur demande.

Sans surprise, la première chose à faire sera de mener une analyse exploratoire des différents fichiers pour faire ressortir des différences clés entre les employés ayant quitté l’entreprise et ceux qui y sont encore. Ce seront des insights importants à formaliser pour les RH !

Ensuite, il faudra que tu réalises un modèle de classification. L’algorithme apprendra à partir des fichiers de données à scorer la probabilité de démission de chaque employé. C’est ce modèle qui nous servira à extraire des causes potentielles de démission. Pour cela, tu utiliseras le package SHAP, après avoir stabilisé ton approche de modélisation !

Je te laisse prendre connaissance de la donnée, on fera le point bientôt pour passer en revue tes trouvailles.

Bon courage !

Amandine

Quelques minutes plus tard
Après lecture de ce mail, vous planifiez vos tâches et décidez de produire les éléments suivants : 

Un fichier pyproject.toml où l’on trouve : 

les versions de Python compatibles avec le projet ;

les dépendances nécessaires à installer pour exécuter les notebooks et les éventuels scripts Python.

Un ensemble de Jupyter Notebooks (fichier au format .ipynb) et/ou de scripts Python (fichiers .py) contenant le code de nettoyage, d'exploration de la donnée ainsi que les modélisations.

Un support de présentation de votre travail sur ce projet à destination d’Amandine. 

Vous vous mettez directement au travail !