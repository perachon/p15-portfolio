Finetunez votre propre LLM
Qu’allez-vous apprendre dans ce projet ? 
 

Au cours de vos projets précédents, vous avez acquis des bases solides en Deep Learning et en traitement du langage naturel (NLP). Vous avez appris à manipuler des modèles sous forme de système RAG et de système Agentique. Vous avez également compris les mécanismes d'attention et à utiliser l'écosystème Transformers pour charger des modèles pré-entraînés et réaliser des inférences. 

 

Vous allez consolider vos compétences en réalisant un projet d'ingénierie IA de bout en bout en livrant un modèle spécialisé, une API de déploiement et un rapport technique complet.


Dans ce projet, vous irez au-delà de la simple utilisation de LLMs. Vous allez apprendre à les spécialiser grâce au fine-tuning supervisé (SFT), une technique permettant d'adapter un modèle générique à une tâche spécifique. Vous pratiquerez des méthodes d'entraînement économes en ressources comme le Low-Rank Adaptation (LoRA). Vous plongerez aussi dans l'alignement de modèles avec des techniques avancées comme le Direct Preference Optimization (DPO), qui visent à rendre le modèle plus fiable, plus sûr et plus conforme aux attentes humaines.

 

En quoi ces compétences sont-elles importantes pour votre carrière ? 
 

La capacité à fine-tuner et à aligner des modèles de langage est l'une des compétences les plus recherchées aujourd'hui dans le domaine de l'intelligence artificielle. Alors que de nombreuses entreprises adoptent l'IA générative, elles ont besoin de spécialistes capables de personnaliser ces modèles pour des domaines métiers précis (santé, droit, finance) et de garantir leur fiabilité. La maîtrise de ces pratiques vous positionnera comme un candidat de premier plan pour des postes d'AI Engineer, de Machine Learning Engineer ou de Spécialiste NLP, où la création de valeur passe par l'adaptation fine des technologies de pointe.


Comment allez-vous procéder ?

Ce projet est découpé en 4 activités principales pour vous mener progressivement vers la réussite.

Cours - Découvrez les enjeux de l’IA générative (dans le secteur de la santé).

Cours - Ce cours “Post-training 101” décrit comment on part d’un modèle pré-entraîné (axé sur la prédiction du token suivant) pour le transformer en modèle “instruction-tuned” via des étapes de fine-tuning supervisé (SFT) puis de renforcement (RL techniques comme RLHF, RLAIF, RLVR). Il traite également de sujets tels que la conception des données d’entraînement, la définition de la fonction de perte ou des mécanismes de récompense, ainsi que l’évaluation des modèles à travers des métriques automatiques, humaines ou fondées sur les préférences.

Cours - Fine Tuning supervisé. Vous apprendrez à votre modèle à mieux répondre aux besoins spécifiques de la tâche.

Mission : Vous réaliserez la mission principale du projet : le développement d'un Proof of Concept (POC) d'un agent IA de triage médical pour le compte d'un centre hospitalier. Vous terminerez en complétant la fiche d’autoévaluation qui servira de base de discussion avec votre mentor avant la soutenance.

À l’issue de ce projet, vous présenterez les livrables de la mission à un mentor évaluateur lors d’une soutenance. Cela vous permettra de valider les compétences visées par ce projet.

 

Prêt à démarrer votre projet ?

Objectifs pédagogiques
Ajuster les paramètres des procédures d'entraînement afin d'optimiser la performance
Évaluer les performances de l’infrastructure sous-jacente au modèle d'apprentissage
Automatiser le déploiement en intégrant les évolutions du modèle

----

Mission - Développez le POC d'un agent de triage médical

Cette mission suit un scénario de projet professionnel.
Vous pouvez suivre les étapes pour vous aider à réaliser vos livrables.

 

Avant de démarrer, nous vous conseillons de :

lire toute la mission et ses documents liés ;

prendre des notes sur ce que vous avez compris ;

consulter les étapes pour vous guider ; 

préparer une liste de questions pour votre session de mentorat.

Prêt à mener la mission ?
Barre

 

Vous êtes missionné en tant qu'IA Engineer junior pour le compte du Centre Hospitalier Saint-Aurélien (CHSA).

 

Le CHSA est un grand hôpital public français dont le service des urgences connaît une surcharge constante, particulièrement aux heures de pointe. Le personnel infirmier et de triage manque parfois d'effectifs, entraînant des temps d'attente prolongés et un risque de négligence des cas critiques non identifiés rapidement.

 

Vous êtes chargé de développer un POC (Proof of Concept) d'un agent IA de triage médical en 4 semaines.

 

De : Dr. Marie Dubois - Directrice Innovation Médicale

À : moi

Objet : Mission POC – Agent IA Triage Médical (CHSA)

Bonjour,

 

Comme convenu lors de notre entretien, je reviens vers vous pour préciser le contexte et les attendus de la mission confiée par la Direction du Centre Hospitalier Saint-Aurélien.

Le CHSA souhaite, face à la surcharge constante de son service d'urgences, disposer d'un agent intelligent permettant d'assister le personnel soignant dans le triage initial des patients.

 

L'agent IA devra accompagner les patients en :

Collectant leurs symptômes via un questionnaire intelligent adaptatif,

Évaluant le niveau de priorité (urgence maximale / modérée / différée) selon les protocoles médicaux,

Fournissant des explications claires sur l'évaluation et les recommandations,

S'intégrant au système d'information hospitalier existant,

Garantissant la traçabilité de chaque interaction pour les audits médicaux.

 

Notre rôle est de réaliser un Proof of Concept (POC) qui démontre la faisabilité technique et la valeur ajoutée clinique d'un tel système.

 
Approche Technique et Stratégie Expérimentale
Les avancées récentes en intelligence artificielle médicale démontrent que les modèles de langage spécialisés peuvent atteindre des performances diagnostiques comparables à celles de médecins en formation. N’hésitez pas à consulter cet article. Toutefois, leur déploiement en environnement clinique exige une méthodologie rigoureuse et une validation approfondie.

Notre stratégie s'articule autour de trois phases progressives :

 

Phase 1 - Validation Conceptuelle : Nous déploierons Qwen3-1.7B-Base, un modèle compact mais performant, permettant de valider rapidement nos hypothèses techniques tout en évaluant l'acceptabilité clinique du système par les équipes soignantes.
Phase 2 - Optimisation Ciblée : Le modèle sera affiné par fine-tuning supervisé (SFT) avec la technique LoRA, puis optimisé via l'alignement par préférences via DPO pour garantir sa conformité aux protocoles médicaux établis.
Phase 3 - Projection Industrielle : En cas de validation concluante du POC, nous envisagerons le passage à des modèles de plus grande envergure (32B+ paramètres) avec des jeux de données étendus pour la mise en production. L'architecture des données médicales (symptomatologie, antécédents, constantes vitales, protocoles de triage) sera particulièrement soignée pour optimiser l'apprentissage du modèle.
 
Feuille de route et mission détaillée
Pour respecter notre échéance de 4 semaines, vos missions s'organisent selon le planning suivant :

 
Semaine 1 - Préparation et structuration des données
Agrégation des corpus médicaux francophones et anglophones 

MediQA, 

FrenchMedMCQA, 

MedQuAD, 

UltraMedical-Preference).

Constitution d'un dataset d'entraînement SFT de 5 000 paires instruction-réponse, optimisé pour le fine-tuning.

Création du dataset de post-entraînement DPO avec paires de réponses validées/non validées.

Anonymisation et validation de la conformité RGPD des données.

 
Semaine 2 - Entraînement Initial par Fine-Tuning Supervisé (SFT)
Au cours de cette semaine, l'objectif principal est de spécialiser le modèle de base sur notre corpus médical.

Implémentation du SFT : Nous lancerons le fine-tuning supervisé du modèle Qwen3-1.7B-Base.

Optimisation par LoRA : La méthode LoRA (Low-Rank Adaptation) sera utilisée pour adapter le modèle de manière efficace, en optimisant l'usage des ressources GPU disponibles.

Validation intermédiaire : Des premières évaluations seront menées sur un jeu de données de test pour mesurer les progrès du modèle et s'assurer que l'entraînement se déroule correctement.

 
Semaine 3 - Alignement du Modèle par Préférences (DPO)
Cette semaine est dédiée à l'affinage du comportement du modèle pour qu'il corresponde aux attentes et aux pratiques cliniques.

Entraînement DPO : Le modèle préalablement fine-tuné sera entraîné avec la méthode DPO (Direct Preference Optimization). Cet entraînement se basera sur les paires de réponses préférentielles du dataset UltraMedical-Preference.

Alignement clinique : L'objectif de cette phase est d'aligner plus finement les réponses du modèle sur les pratiques cliniques validées, en lui apprenant à distinguer les réponses de meilleure qualité des réponses moins pertinentes ou incorrectes.

 
Semaine 4 - Déploiement et validation
Mise en production pilote

Déploiement d'un endpoint prototype via vLLM.

Simulation d'inférence en conditions quasi-réelles.

Tests de latence, pertinence et traçabilité des interactions.

Évaluation finale

Analyse des métriques de performance.

Rédaction du rapport de synthèse.

Formulation des recommandations stratégiques pour le passage à l'échelle.

 
Livrables attendus
À l'issue de cette mission de 4 semaines, vous devrez fournir :

 

Dataset médical bilingue prêt à l'emploi : Un corpus de données médicales (francophone et anglophone) entièrement nettoyé, structuré et anonymisé en conformité avec le RGPD. Ce dataset sera optimisé pour les phases de fine-tuning (SFT) et d'alignement (DPO).

Modèle d'IA spécialisé et optimisé : Le modèle de langage Qwen3-1.7B, fine-tuné avec les techniques SFT et LoRA, puis aligné par préférences (DPO) pour garantir la pertinence clinique de ses réponses. Les poids finaux du modèle seront fournis.

Endpoint de démonstration déployé sur le cloud : Une interface de démonstration fonctionnelle et accessible via une API. L'endpoint sera déployé sur le fournisseur de cloud de votre choix et optimisé pour une inférence rapide grâce à la technologie vLLM.

Pipeline d'intégration et de déploiement continu (CI/CD) : Un pipeline CI/CD mis en place avec GitHub Actions. Ce pipeline automatisera les tests et le déploiement de nouvelles versions du modèle, assurant ainsi la maintenabilité et l'évolutivité de la solution.

Rapport technique complet et recommandations stratégiques : Un document de 20 pages maximum détaillant :

La méthodologie employée pour la préparation des données et l'entraînement du modèle.

Les métriques de performance (latence, pertinence, etc.).

Une analyse des résultats obtenus.

Une roadmap claire pour le passage à l'échelle et le déploiement à plus grande échelle au sein du CHSA.

 

Cette mission représente un enjeu stratégique majeur pour l'amélioration de la prise en charge des patients au CHSA. Nous comptons sur votre expertise technique et votre rigueur méthodologique pour concrétiser cette innovation au service de la santé publique.

 

Merci pour votre investissement.

Bien cordialement

 

Dr. Marie Dubois

----

Etape 0 : Consultez ces bases théoriques pour bien démarrer

Définition de SFT  - Supervised Fine-Tuning

Le Supervised Fine-Tuning est une méthode d’entraînement où l’on prend un modèle déjà pré-entraîné et on l’affine avec des exemples bien annotés (questions/réponses, consignes/réalisations, etc.). 

Le modèle apprend ainsi à mieux suivre les attentes humaines en copiant les bons comportements montrés dans ces données supervisées.

 
Définition de DPO - Direct Preference Optimization

Le Direct Preference Optimization (DPO) est une méthode d’alignement des modèles de langage basée sur des préférences humaines.
L’idée est d’entraîner le modèle à générer des réponses qui correspondent davantage aux attentes humaines sans passer par un modèle de récompense intermédiaire.
En pratique, le DPO permet d’apprendre directement à partir de paires de réponses annotées par des humains en indiquant laquelle est préférée.




Etape 1 - Collectez et structurez les données

Dans cette étape vous allez :

Collecter, nettoyer et structurer un corpus médical bilingue (français / anglais) destiné au fine-tuning et à l’alignement par préférences. 

Produire environ 5 000 paires instruction-réponse pour SFT et constituer un jeu de paires préférentielles (DPO) validées cliniquement. 

Anonymiser toutes les données et documenter le processus RGPD. 

Définir le schéma des métadonnées (symptômes, antécédents, constantes, source, niveau de confiance). 

Préparer jeux train / val / test et jeux d’évaluations cliniques séparés.

 
Prérequis 

Avoir réalisé un inventaire des sources de données disponibles (MediQA, FrenchMedMCQA, MedQuAD, UltraMedical-Preference, etc.).

Avoir accès aux environnements de stockage et compute (espace disque, notebooks).


Résultats attendus 

Dataset médical bilingue anonymisé et versionné, prêt pour SFT (≈5 000 paires) et pour la constitution du jeu DPO.

Schéma des métadonnées.

Justification du processus RGPD suivi.

 

Recommandations 

 

Vous pouvez utiliser Presidio pour anonymiser vos données. C’est un outil open source conçu pour détecter et masquer automatiquement les données sensibles.

 

Effectuez l’installation avec la commande :

 

pip install presidio-analyzer presidio-anonymizer

Puis créez : 

un moteur d’analyse (AnalyzerEngine) pour identifier les entités sensibles, à défaut nom, prénom des patients.

un moteur d’anonymisation (AnonymizerEngine) pour les masquer.

 

Pensez à utiliser un modèle linguistique adapté (ex : fr_core_news_md) et à tester différentes stratégies de masquage (replace, mask, redact) selon vos besoins. 

 

Contrôlez la qualité du masquage pour vous assurer qu’aucune donnée personnelle identifiable ne subsiste.

Prioriser la qualité (annotations validées) sur la quantité.

Standardiser les formats (JSONL / HF datasets) et enregistrer les métadonnées.

Documenter votre repository sur l’origine et la licence de chaque source ou déposer votre jeu de données sur https://huggingface.co/datasets.

 
 

Points de vigilance

Ne pas mélanger données d’entraînement et données d’évaluation.

Conserver une trace de chaque transformation de données (auditabilité).

 

Outils 

Hugging Face Datasets

Presidio



Etape 2 - Affinez et alignez le modèle

Effectuer le fine-tuning supervisé (SFT) du modèle Qwen3-1.7B-Base en utilisant LoRA afin de limiter l’empreinte GPU, puis entraîner l’alignement par préférences (DPO / GRPO) sur les paires préférentielles. 

Valider les performances intermédiaires sur les jeux de test cliniques et réaliser les contrôles de sécurité (hallucinations, recommandations dangereuses). 

Itérer sur les hyperparamètres et les checkpoints en gardant une traçabilité (fichiers de logs, métriques et checkpoints pour la reprise de l'entraînement si nécessaire).

 

Prérequis 

Avoir accès à GPUs et à un environnement ML (PyTorch / HF transformers / Unsloth ).

Avoir défini les métriques d’évaluation cliniques et les seuils d’acceptation.

Avoir mis en place la sauvegarde/monitoring des modèles et des logs d’entraînement.

 

Résultat attendu  

Modèle Qwen3-1.7B  adapté (SFT LoRA + DPO) avec métriques d’évaluation documentées et checkpoints reproductibles.

 

Recommandations

Commencer par des petits runs LoRA pour valider la pipeline avant la montée en charge.

Mettre en place des checkpoints.

Enregistrer et documenter chaque version (hyperparamètres + seed).

 

Points de vigilance 

Éviter le sur-apprentissage sur les exemples annotés.

 

Outils

PyTorch / Hugging Face Transformers / PEFT (LoRA). 

MLflow ou Weights & Biases pour tracking.

 

Ressources

Tutoriels LoRA + PEFT [nom & lien].

Exemple de notebook LoRA + SFT



Etape 3 - Déployez et validez le POC

Automatiser le déploiement du prototype via le pipeline CI/CD mis en place sur GitHub Actions.

Conteneuriser l'application avec Docker et l'exposer via une API (FastAPI) en utilisant vLLM pour une inférence optimisée.

Réaliser des tests de latence, de robustesse, ainsi que des audits de traçabilité des interactions.

 

Prérequis 

Le modèle fine-tuné et validé est disponible.

Le pipeline CI/CD sur GitHub est fonctionnel.

 

Résultats attendus 

Un endpoint de démonstration déployé et accessible en environnement pilote.

Un processus de déploiement automatisé et reproductible.

Le rapport final incluant les métriques de performance et la roadmap de déploiement.

 

Recommandations

Mesurer la latence et le temps de réponse en conditions réalistes.

Préparer un plan de mise en production conditionnel (checklist « go / no-go »).

 
Points de vigilance 

Protéger les clés / secrets et l’accès aux endpoints.

Prévoir procédures de surveillance après déploiement.

Documenter clairement les limites d’usage pour les utilisateurs.

 

Outils

vLLM / Docker / FastAPI.

Outils de CI/CD et tests d’intégration (GitHub Actions, GitLab CI).

Ressource

 

Documentation vLLM & exemples de déploiement