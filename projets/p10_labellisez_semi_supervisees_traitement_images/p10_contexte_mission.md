(P10) Projet 10

Labellisez et appliquez des approches semi-supervisées en traitement d'images


Qu’allez-vous apprendre dans ce projet ?
 

Si vous suivez ce parcours dans sa totalité, vous avez découvert les concepts de traitement d'images, d’analyse non-supervisée et de deep learning dans des contextes complexes. 

Dans ce nouveau projet, vous allez consolider vos compétences en :

Prétraitement d’images et extraction de features avec des modèles pré-entraînés

Analyse non-supervisée (clustering, visualisation de la structure des données) 

Application d’approches semi-supervisées

Clarté de présentation de vos analyses.

Vous allez pratiquer l’analyse non-supervisée (comme le clustering) pour détecter des structures dans les images. Vous utiliserez des modèles pré-entraînés pour extraire des features visuelles. Vous plongerez dans l’apprentissage semi-supervisé, un compromis entre supervision totale et autonomie du modèle. Cette méthode permet de tirer parti de données non labellisées. 

 

En quoi ces compétences sont-elles importantes pour votre carrière ?
Ces compétences sont utiles pour le traitement de données non-structurées comme les images. Elles sont essentielles dans des domaines comme la santé, l’industrie ou la surveillance visuelle. Les pratiques d’apprentissage semi-supervisé sont recherchées par les recruteurs en deep learning et en computer vision. Savoir prétraiter les données et extraire des représentations pertinentes permet de concevoir des modèles robustes. Être capable d’interpréter ses résultats est une qualité clé pour tout Data Scientist.

 
Comment allez-vous procéder ?
Ce projet est découpé en 2 activités :

Cours : Vous suivrez les cours :

Initiez-vous au deep learning pour comprendre les enjeux du traitement d’image et du deep learning.

Initiez-vous à l'apprentissage semi-supervisé pour apprendre le traitement d’images.

Vous avez deux façons de réaliser ce projet :
Option A – Mission en entreprise : vous appliquez vos compétences sur un cas réel de votre entreprise, en utilisant ses données et son contexte métier. Cette option vous permet de progresser directement sur vos missions professionnelles et de constituer un portefeuille unique et différenciant.

Option B – Mission fictive : vous travaillez sur un cas guidé de scoring de crédit avec un dataset fourni. Cette option vous permet de vous exercer dans un cadre structuré, proche d’un cas réel, avec des données simulées.

Vous terminerez en complétant la fiche d’autoévaluation, qui servira de base de discussion avec votre mentor.

À l’issue de ce projet, vous finaliserez et partagerez les livrables de la mission (démo + revue des choix/limites), afin de consolider les compétences visées par ce projet.

Objectifs pédagogiques
Identifier ou créer un modèle d’apprentissage adapté aux contraintes et aux besoins métier
Préparer et transformer des données afin de les adapter au modèle d’apprentissage.

----------

Mission fictive - Analysez des images médicales avec des méthodes semi-supervisées

Cette mission suit un scénario de projet professionnel.
Vous pouvez suivre les étapes pour vous aider à réaliser vos livrables.

 

Avant de démarrer, nous vous conseillons de :

lire toute la mission et ses documents liés ;

prendre des notes sur ce que vous avez compris ;

consulter les étapes pour vous guider ; 

préparer une liste de questions pour votre première session de mentorat.

Prêt à mener la mission ? 

Vous êtes Data Scientist junior spécialisé en Computer Vision au sein de CurelyticsIA, une startup innovante dans le domaine de la e-santé. L’entreprise développe des solutions basées sur l’intelligence artificielle pour assister les professionnels de santé dans l’analyse d’images médicales, en particulier des IRM.

 

Dans le cadre d’un nouveau projet R&D, CurelyticsIA souhaite explorer la possibilité d’automatiser la détection de tumeurs du cerveau. Un ensemble conséquent de radios a été collecté : la majorité de ces images ne dispose d’aucun étiquetage, tandis qu’un sous-ensemble limité a été annoté par des radiologues experts.

 

Vous êtes chargé de concevoir une première exploration analytique du jeu de données. Plus précisément, votre mission est de :

Explorer les images et extraire des caractéristiques visuelles via un modèle pré-entraîné ;

Appliquer des méthodes de clustering pour identifier des structures ou regroupements dans les données ;

Mettre en œuvre une méthode d’apprentissage semi-supervisé à partir des quelques étiquettes disponibles ;

Synthétiser vos résultats, formuler des recommandations, et les présenter à votre équipe projet.

 

Vous recevez un mail de votre responsable Data Science, Clara Martin, qui vous partage les détails du projet :

 

De : Clara

À : Moi

Objet : Mission d'exploration et de modélisation - Données radios

 

Bonjour,

 

Comme discuté lors de notre dernière réunion, tu es assigné à la première phase du projet BrainScanAI. Tu trouveras en pièce jointe un fichier zip contenant :

Le jeu de données de radiographies (en format PNG + métadonnées anonymisées),

Une documentation technique sur le format des images ;

Une liste restreinte de labels annotés par nos partenaires hospitaliers (normal/cancéreux). 

Pour info, notre budget actuel pour la labellisation par IA est de 300 euros pour ce dataset. 

Tes objectifs :

Extraire des caractéristiques visuelles pertinentes à l’aide d’un modèle pré-entraîné (type ResNet ou équivalent).

Réaliser un clustering exploratoire pour identifier des regroupements naturels.

Mettre en œuvre une méthode semi-supervisée en exploitant les labels partiels pour prédire les étiquettes manquantes.

Proposer des livrables au format Notebook contenant :

l’extraction des features

le preprocessing adapté au(x) modèle(s) utilisés

l’analyse non-supervisée (.ipynb)

l’entraînement de modèles de clustering

l’approche semi-supervisée (.ipynb)

 

Ces livrables doivent être accompagnés d’un support de présentation proposant des recommandations techniques pour un passage à l’échelle (budget de 5 000 euros pour 4 millions d’images à labelliser). Est-ce que ce passage te paraît faisable et si oui, sous quelles conditions ?

Rédiger une synthèse de ton approche et de tes résultats dans un support de présentation.

Les contraintes :

Travailler en Python.

Tester plusieurs algorithmes.

Avoir des métriques pertinentes en fonction de l’erreur la plus importante (F1, Acc, Précision, ou autre ?).

Clairement définir ce que tu considères comme un objectif atteint (“definition of done”).

 

N’hésite pas à me faire un point d’étape avant la fin de semaine. Bon courage pour cette mission !

 

Clara

Responsable Data Science

CurelyticsIA