(P2) Projet 2

Requêtez des services IA



Qu’allez-vous apprendre dans ce projet ?
 

Ce premier projet de votre parcours est un projet d'entraînement qui ne fera pas l’objet d'une soutenance. Il sera l’opportunité de découvrir ou de renforcer vos compétences dans le langage Python et dans l'utilisation du notebook Jupyter. 

Jupyter est un outil puissant qui combine l'écriture de code, les commentaires et les visualisations, ce qui facilite la tâche pour structurer vos analyses et les communiquer à tout type d’audience.

Vous allez utiliser votre premier algorithme déjà pré-entraîné sur Hugging Face. 

Ce modèle de segmentation d'images proposé dans ce projet et disponible sur Hugging Face, permet d'identifier et d'isoler précisément chaque pièce vestimentaire dans une image. En suivant les étapes, pas à pas, vous découvrirez aussi la gestion des tokens API et la sécurisation des informations sensibles. 

Vous apprendrez à optimiser les images pour les requêtes API et à gérer les erreurs potentielles.

En quoi ces compétences sont-elles importantes pour votre carrière ? 
Ces compétences sont utiles pour le domaine de l'intelligence artificielle. La maîtrise des modèles et l’utilisation des API est recherchée par les recruteurs en data science. Ces pratiques sont essentielles pour analyser et fournir des insights. 

Comment allez-vous procéder ? 
Ce projet est découpé en 2 activités incluant des cours et des exercices : 

Cours : En fonction de votre expérience et de votre niveau en programmation, vous suivrez 2 cours qui vous formeront aux bases de l’analyse de données en Python.
Exercice  : Vous vous exercerez en utilisant un algorithme sur Hugging Face.
 À l’issue de ce projet, vous aurez une session de bilan avec votre mentor pour discuter de votre projet.

Cela vous assurera que vous êtes sur la bonne voie avant de passer à la suite.

Prêt à démarrer votre projet ? 
Lancez-vous dans la première section cours.

Objectifs pédagogiques
Choisir et configurer un modèle d’apprentissage déjà entrainé
Configurer l’environnement de travail pour exploiter des données

------

Exercice - Requêtez des services IA

C'est votre premier jour au sein du laboratoire d'intelligence artificielle de ModeTrends, l'une des agences de conseil en marketing digital les plus influentes dans l'industrie de la mode. Fondée il y a 15 ans, ModeTrends collabore avec des marques de luxe et des distributeurs de fast fashion dans plus de 30 pays.

Face à l'explosion du marketing d'influence, ModeTrends a lancé un projet ambitieux : "Fashion Trend Intelligence". Ce projet vise à développer un système automatisé d'analyse des tendances vestimentaires sur les réseaux sociaux. Son objectif est d'offrir aux marques clientes des informations précises et en temps réel sur les nouvelles modes émergentes, bien avant qu'elles ne deviennent populaires.

Pour démarrer ce projet, votre responsable Sophia a présenté cette mission à l'équipe IA lors d'une réunion d'information. Elle a exposé le contexte général, les objectifs et les spécificités techniques de la fonctionnalité de "segmentation de vêtements" que vous allez développer.

Vous recevez un mail récapitulatif que Sophia vous a écrit suite à la réunion de présentation :

De : Sophia Le Guennec
Envoyé : Hier 16:38
À : Moi
Objet : Récap mission "Fashion Trend Intelligence"
Bonjour,

 

Merci pour ton attention lors de la présentation du projet "Fashion Trend Intelligence". Je voulais, dans ce mail, te faire un récap de ta mission. Comme tu l'as compris, l'objectif global est de concevoir, développer et déployer un système d'analyse qui aura les fonctionnalités suivantes :

Segmentation vestimentaire : être capable d'identifier et d'isoler avec précision chaque pièce vestimentaire dans une image.

Analyse stylistique : être capable de classifier les pièces selon leur nature, couleur, texture et style.

Agrégation de tendances : être capable de compiler ces données sur des milliers de publications pour identifier les tendances émergentes.

Comme tu peux le voir, ce projet est ambitieux et présente plusieurs challenges! Mais ne t'inquiète pas, ton rôle sera de travailler sur la première fonctionnalité, la Segmentation vestimentaire.

En ce qui concerne cette fonctionnalité de segmentation, la première étape est d'utiliser un modèle pré-entraîné capable d'identifier les différentes pièces vestimentaires dans une photo comme sur l'illustration ci-dessous. 
Pour y arriver, peux-tu évaluer si on peut utiliser les endpoints serverless de Hugging Face et particulièrement le modèle SegFormer-clothes, modèle affiné sur la segmentation vestimentaire ?

Ta première tâche sera donc de tester si le modèle Segformer clothes de Hugging Face peut correctement segmenter les vêtements à partir de quelques images que nous avons annotées (cf les données de test en pièce jointe). Ce n’est pas la panacée, mais ça sera suffisant pour un premier test. Vérifie que lorsque tu envoies une image au service Hugging Face, il te renvoie bien la segmentation correcte des différentes pièces vestimentaires.

Voici le notebook créé par le précédent Data Scientist, peux-tu le reprendre et changer ta clé API et faire tourner le modèle ? 

Ta seconde tâche sera de nous faire un chiffrage approximatif du coût d’utilisation de cette solution avec l’API de Hugging Face. Pourrais-tu évaluer le coût pour 500 000 images sur 30 jours par exemple ?

Tu trouveras un template de présentation en pièce jointe pour présenter les résultats de ton projet à l’équipe ainsi que les données de test.

N’oublie pas de mettre en avant ta capacité à être synthétique et à simplifier ton discours technique si nécessaire. 

Bon courage!

Sophia 

Directrice IA - ModeTrends

Dans cet exercice, vous allez développer un système d'analyse des tendances mode en utilisant un modèle d'IA pour segmenter les vêtements portés par des influenceurs sur Instagram. Comme le décrit le brief de Sophia, votre responsable chez ModeTrends, vous êtes chargé de tester si le modèle segformer_clothes de Hugging Face peut identifier les différentes pièces vestimentaires dans des images.

Cet exercice est entièrement guidé. Suivez les étapes ci-dessous.

------

Témoignage d'un expert data sur son usage de l'IA générative

Témoignage d'un expert Data  sur son utilisation de l'intelligence artificielle générative.

Nous avons posé quelques questions à Damien Fabiano, sur son usage de l’Intelligence Artificielle Générative en tant que mentor, référent technique et expert Data.

Dans quelle mesure l’IA est-elle utile dans ton métier d’expert data aujourd’hui ?

Aujourd’hui, j’utilise des outils d’IA Générative pour aller plus vite sur des petits sujets : Analyse exploratoire rapide d’un petit jeu de données, création d’un résumé sur une analyse complète, nettoyage du code et ajout de commentaires structurés. Je me permets d’utiliser l’IA là dessus car ce sont des sujets que je maîtrise déjà. Je délègue à l’IA des tâches où je n’ai plus de valeur ajoutée.

 J’ai mis aussi toute mon équipe sur des méthodes de vibe coding (Code généré par de l’IA Générative) comme BMAD ou OpenSpec. Cela permet de profiter de la puissance de l’IA tout en limitant les risques de  dette technique. Sur la même idée que le point précédent, il faut savoir que sur une équipe qui sait déjà coder, qui maîtrise déjà son métier, l’IA vient les rendre 10x plus rapides.

Par contre, si un junior intègre l’équipe, je vais limiter son utilisation de l’IA a de la conception de documentation et à de l’aide à la compréhension de code et de concepts mais pas à la génération pure de code.

 J’utilise aussi beaucoup l’IA comme coach personnel et professionnel. C’est comme avoir un mentor particulier, disponible 24h sur 24, 7j sur 7, et que j’ai paramétré pour qu’il me challenge sur mes points aveugles. (Prompt system en annexe ci-dessous).

 J’apprends beaucoup plus vite qu’avant, l’IA me permet de comprendre en moins de 15 minutes un concept en lui posant  des questions très précises, là où il m’aurait fallu probablement une bonne heure de recherche internet.

Comment réalises-tu ta veille sur l’IA ? Comment fais-tu pour te tenir au courant des évolutions ?Barre
Je commençais à suivre des comptes LinkedIn pour suivre les évolutions de l’IA, et puis j’ai très rapidement arrêté car 90% de ce qui est publié est trop superficiel. Je travaille principalement avec une série de newsletters journalières nommée TLDR. Cela m’évite de m’éparpiller et de me faire happer par les réseaux sociaux. Je lis le résumé des articles et si certains m’intéressent, je les enregistre et soit je les lis plus tard, soit je les écoute en format audio (avec des outils comme Matter, Instapaper ou Readwise) pendant mes temps de trajets.

 Pour chacune des news, ou des nouveaux concepts que je découvre, je me force toujours à comprendre le fond et à le connecter à quelque chose que je connais déjà. Par exemple, je connaissais déjà les modèles GPT, il m’a donc été plus facile de comprendre les modèles de diffusion en me disant que l’un ajoute du contenu étape par étape tandis que l’autre enlève du bruit étape par étape.

Quelle approche de l’IA préconises-tu pour des étudiants en Data ? Quels réflexes adopter quand on se forme à un métier dans la data ? Barre
Je donne aux étudiants les mêmes consignes qu’aux juniors qui rejoignent l’équipe. Je leur dis de l’utiliser pour apprendre sans modération. Attention, apprendre ne veut pas dire réciter comme un perroquet !

L’IA pour apprendre est plus efficace sur les sujets “falaises” que sur les sujets “montagnes”. 

Les langages de programmation sont des sujets “montagnes”. C'est -à -dire que ce sont des sujets qui se maîtrisent avec du temps et de la pratique. Plus vous faites du Python, plus vous devenez bons et plus les concepts s’ancrent en vous. 

A contrario, les sujets “falaises” sont des changements de paradigmes, des sujets avec des moments “Eureka”, où certains ont l’éclair de génie après 10h de travail et d’autres après 30h. Pour un étudiant en data analyse, c’est souvent le cas en SQL, alors que pour un étudiant en Data engineering, ce sont Docker et les environnements.

Avec les avancées de la tech en général, le monde se transforme de plus en plus vite. Les outils que l’on utilisait il y a 5 ans sont déjà obsolètes, les outils d’aujourd’hui seront sûrement obsolètes dans 2 ans. Donc savoir apprendre très rapidement de nouveaux concepts est devenu crucial. Et pour ça, l’IA est d’une grande aide.

D’après toi, comment faudrait-il utiliser intelligemment l’IA Generative pour une utilisation professionnelle et technique efficace au quotidien ?Barre
Je pense qu’il n’y a qu’un seul grand concept à garder en tête sur l’utilisation de l’IA en milieu professionnel : la création de valeur. 

Est-ce que l’IA aujourd’hui dans mon travail peut m’aider à créer plus de valeur pour moi et mon entreprise ? Utiliser l’IA pour réduire un temps sans valeur ajoutée n’est pas suffisant si le temps gagné n’est pas concentré sur de la valeur ajoutée. Par exemple, un manager qui parvient à gagner une heure de temps “administratif”, doit pouvoir utiliser cette heure pour se mettre au service de son équipe. 

Une des meilleures façons de garder en tête cette création de valeur reste sa mesure.

Exemples : 

Score NPS qui s’améliore sur un produit

Réduction du nombre de projets en retard

Amélioration de la note managériale lors des évaluations de fin d’année.