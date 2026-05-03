Démonstration soutenance
Ordre conseillé :
1. GET /health
2. POST /triage sur un cas standard
3. POST /triage sur un cas avec red flag
4. GET /audit/{interaction_id} pour montrer la traçabilité
Cette démonstration permet de montrer en quelques minutes la disponibilité du service, la structure de la réponse, la logique de sécurité et l’audit.



Questions ? 


---

C’est quoi un adapters LoRA ? ça sert à quoi ? 

Un adapter LoRA est un petit jeu de poids additionnels appris pendant le fine-tuning, qu’on vient brancher sur le modèle de base au moment de l’inférence. L’idée est de ne pas réentraîner ni republier tous les poids du LLM, mais seulement la partie qui capture l’adaptation au domaine métier, ici le triage médical. 
Dans ce projet, ça sert à rendre Qwen3-1.7B spécialisé en SFT puis aligné en DPO, tout en gardant une solution plus légère, plus rapide à entraîner, plus simple à publier et plus réaliste avec ta contrainte GPU 6 Go.

Concrètement, ce qu'on fournis sur Hugging Face n’est donc pas le “gros modèle complet” réécrit de zéro, mais les poids d’adaptation LoRA. Pour reconstruire le modèle final, on combine le modèle de base Qwen/Qwen3-1.7B-Base avec l’adapter LoRA publié. C’est une approche standard, propre, et cohérente avec un POC d’AI Engineering.


Un adapter LoRA correspond à un ensemble de poids d’adaptation légers qui se branchent sur le modèle de base ; cela permet de spécialiser Qwen3-1.7B pour le triage médical sans republier l’intégralité des poids du modèle.

=> format plus simple à dire à l'oral ?


------------

	VOCABULAIRES


Spécialisation du modèle
La spécialisation du modèle, c’est le fait de partir d’un LLM généraliste et de l’adapter à un usage précis, ici le triage médical. L’objectif était d’obtenir des réponses plus utiles, plus prudentes et plus structurées. Pour ça, j’ai utilisé un SFT avec LoRA, puis un alignement DPO. 

C’est le fait d’adapter un modèle généraliste à une tâche précise, ici le triage médical. Ça sert à obtenir des réponses plus pertinentes, plus structurées et plus adaptées au contexte métier que celles d’un modèle de base.
Je suis parti de Qwen/Qwen3-1.7B-Base, puis je l’ai entraîné en deux étapes :
- SFT pour lui apprendre le format et le type de réponses attendues
- DPO pour mieux aligner les réponses avec des préférences plus prudentes et plus cohérentes cliniquement
Et j’ai aussi utilisé LoRA pour rendre cet entraînement faisable avec des ressources GPU limitées.


Garde-fous de sécurité
Ce sont des mécanismes qui encadrent le comportement du système pour éviter des réponses risquées. Ça sert à sécuriser l’usage du modèle, surtout dans un domaine sensible comme la santé, en empêchant le système de réagir librement sur des cas potentiellement graves.

J'ai mis en place une logique de red flags. Si certains symptômes critiques sont détectés, comme douleur thoracique, malaise ou saignement important :
- la priorité passe directement à urgence_maximale
- l’API renvoie une recommandation prudente
- le modèle génératif est court-circuité

Les garde-fous de sécurité servent à éviter qu’un modèle génératif prenne trop de liberté sur des cas sensibles. Dans mon POC, j’ai mis en place des red flags. Si un symptôme grave est détecté, la priorité est forcée et le système renvoie une réponse prudente sans laisser le modèle improviser.

Traçabilité
La traçabilité, c’est le fait de conserver l’historique des interactions pour pouvoir relire et auditer le comportement du système. Dans le projet, chaque interaction est enregistrée dans une base SQLite et peut être retrouvée via l’API.

C’est la capacité à garder une trace de ce que le système a reçu, produit et décidé.
Ça sert à pouvoir auditer le comportement du système, comprendre ses décisions, analyser les cas à risque et justifier ce qui s’est passé.
J'ai mis en place un audit des interactions dans SQLite, avec un endpoint dédié pour relire une interaction par identifiant. Cela permet de retrouver les échanges et les décisions prises par le système.



Automatisation
Qu’est-ce que c’est ?
C’est le fait d’automatiser les contrôles techniques du projet pour éviter les régressions et rendre le système plus maintenable.

À quoi ça sert ?
Ça sert à fiabiliser le projet dans le temps, en vérifiant automatiquement que le code reste propre, testable et déployable.

Comment tu l’as fait ?
Tu as mis en place un pipeline GitHub Actions qui exécute automatiquement :

le lint avec Ruff
les tests avec Pytest
la vérification du build Docker
Version orale simple :
« L’automatisation permet de sécuriser l’évolution du projet. J’ai mis en place une pipeline GitHub Actions qui vérifie automatiquement la qualité du code, les tests et la conteneurisation. »


------------

	PPT

-----------


1) Contexte & problème

	Pourquoi un POC et pas directement un produit ?

Réponse courte : Parce qu’en santé il faut d’abord valider la faisabilité technique, la sécurité et la traçabilité avant toute ambition clinique ou réglementaire.


	Quelle est la valeur ajoutée clinique ?

Réponse courte : Aider à structurer le recueil des symptômes, prioriser plus vite et ne pas manquer certains cas critiques grâce aux garde-fous.




2) Données, préparation et gouvernance

	Pourquoi ne pas avoir fusionné plusieurs datasets médicaux ?

Réponse courte : Pour garder un corpus cohérent, limiter la dette de nettoyage et sécuriser la reproductibilité dans le cadre du POC.


	Comment traitez-vous l’anonymisation ?

Réponse courte : Le POC repose sur une source publique documentée, et une stratégie type Presidio est prévue si des données réelles sont intégrées plus tard.




3) Modèle et stratégie de spécialisation

	Pourquoi Qwen3-1.7B et pas un modèle plus gros ?

Réponse courte : Parce qu’il fallait un compromis réaliste entre performance et contraintes matérielles pour un POC exécutable.


	Pourquoi faire du DPO après le SFT ?

Réponse courte : Parce que le SFT apprend la structure de réponse, alors que le DPO affine les préférences et le comportement.




4) Optimisation et suivi des expérimentations

	Qu’avez-vous vraiment optimisé ?

Réponse courte : Le compromis entre faisabilité, stabilité d’entraînement et coût mémoire.


	Pourquoi MLflow était important ?

Réponse courte : Parce que cela apporte une vraie traçabilité expérimentale, attendue dans un projet sérieux d’IA.


	Pourquoi avoir utilisé un batch size faible ?

Parce que j’étais limité par la mémoire GPU. Le batch faible m’a permis de faire tenir l’entraînement sur ma machine.


	Pourquoi ajouter de l’accumulation de gradient ?

Parce qu’avec un batch size très petit, on peut manquer de stabilité. L’accumulation permet de simuler un batch effectif plus grand, sans consommer autant de mémoire d’un coup.


	Pourquoi LoRA ?

Parce que LoRA permet de fine-tuner un grand modèle de manière beaucoup plus légère, en n’entraînant qu’une petite partie des paramètres. C’était le choix le plus réaliste dans mon environnement.


	Qu’est-ce que vous entendez par variantes LoRA low-VRAM ?

Ce sont des réglages encore plus économes en mémoire, par exemple des séquences plus courtes, un rang LoRA plus faible ou moins de modules ciblés, pour rester compatible avec un GPU limité.


	Pourquoi ne pas avoir fine-tuné le modèle complet ?

Parce que ce n’était pas réaliste avec mes ressources matérielles. Le but du POC était de démontrer une approche faisable, pas de viser un entraînement massif inaccessible localement.


	À quoi sert MLflow dans votre projet ?

MLflow sert à suivre les runs, les paramètres et les résultats d’entraînement. Ça permet de comparer les expériences et de garder une vraie traçabilité.

	Pourquoi montrer une courbe d’eval loss ?

Parce qu’elle montre que l’apprentissage est suivi sur un jeu de validation séparé du train. C’est plus utile qu’une simple loss d’entraînement pour voir si le modèle progresse proprement.


	Qu’est-ce que la courbe prouve exactement ?

Elle montre surtout que l’entraînement progresse dans la bonne direction sur le jeu de validation. Elle ne prouve pas à elle seule la qualité clinique du système.


	Quelle différence entre loss et eval loss ?

La loss est mesurée sur le jeu d’entraînement. L’eval loss est mesurée sur un jeu de validation séparé, pour suivre la qualité du modèle sur des données non utilisées directement pour apprendre.


	Pourquoi ne pas parler du test ici ?

Parce que le test sert plutôt à l’évaluation finale. Pendant l’entraînement, on regarde surtout la validation pour piloter et suivre les runs.


	Est-ce que la baisse de l’eval loss suffit pour valider le modèle ?

Non. C’est un bon signal technique, mais il faut aussi des tests fonctionnels, des cas métier et, à terme, une vraie validation clinique.


	Comment savez-vous que l’entraînement n’est pas fait au hasard ?

Parce que les runs sont tracés, les hyperparamètres sont documentés, et les courbes permettent de suivre l’évolution de l’apprentissage.




5) API de triage

	Pourquoi une API plutôt qu’un notebook de démo ?

Réponse courte : Parce qu’une API est testable, intégrable, industrialisable et beaucoup plus crédible pour un SIH.


	Comment se fait l’adaptativité ?

Réponse courte : Par détection de mots-clés et contexte déjà fourni, pour poser les questions utiles sans répéter.




6) Sécurité et traçabilité

	Pourquoi ne pas laisser le modèle décider dans tous les cas ?

Réponse courte : Parce qu’en contexte sensible, il faut encadrer le génératif par des règles métier explicites.


	L’audit sert à quoi concrètement ?

Réponse courte : À tracer les échanges, analyser les comportements et préparer une gouvernance plus robuste.




7) Déploiement et performances

	Pourquoi la démo cloud n’utilise pas directement vLLM ?

Réponse courte : À cause des contraintes de coût et d’infrastructure du cadre gratuit du POC.


	Est-ce que la latence est acceptable ?

Réponse courte : Pour un POC de validation, oui ; pour un usage réel, non, d’où la recommandation d’une infra dédiée.


	Pourquoi avoir choisi Hugging Face Spaces pour la démonstration ?

Réponse courte : parce que c’était une solution simple, publique et rapide à mettre en place pour exposer l’API dans le cadre du POC.


	Pourquoi y a-t-il un écart aussi important entre les latences cloud et local ?

Réponse courte : parce que je ne compare pas deux déploiements identiques. Le cloud repose sur une démo légère, alors que le local exécute le vrai modèle spécialisé.


	Est-ce que ces mesures suffisent pour juger la performance finale du système ?

Réponse courte : non. Elles montrent surtout la faisabilité du POC. Une évaluation de performance plus rigoureuse demanderait une infrastructure comparable et plus adaptée.




8) Qualité logicielle et CI/CD

	Le CD est-il complet ?

Réponse courte : Non, pas au sens d’un redéploiement cloud complet automatique, mais la base CI/CD demandée est bien en place.


	Pourquoi inclure Docker dans le pipeline ?

Réponse courte : Pour vérifier que l’application reste packagée et exécutable dans un environnement propre.


	Qu’est-ce que votre pipeline CI/CD vérifie exactement ?

Réponse courte : la qualité du code avec Ruff, les tests avec Pytest, et le build Docker pour vérifier que l’application reste exécutable.


	Pourquoi avoir mis en place une CI/CD dans un POC ?

Réponse courte : pour limiter les régressions, garder un projet maintenable, et montrer une vraie démarche d’ingénierie logicielle.


	Est-ce que vous avez un vrai déploiement continu automatique ?

Réponse courte : non, pas un déploiement complet en production. En revanche, la base d’intégration continue est bien en place.



9) Conclusion et suite proposée

	Si vous aviez 3 mois de plus, que feriez-vous ?

Réponse courte : Un jeu d’évaluation clinique expert, un déploiement vLLM GPU, plus de monitoring et une intégration supervisée avec un portail ou un SIH.


	Peut-on le mettre en production maintenant ?

Réponse courte : Non, pas de manière autonome ; uniquement dans un cadre pilote, supervisé et très encadré.


	Pourquoi ne pas recommander une mise en production directe ?

Réponse courte : parce qu’il manque encore une validation clinique complète, une intégration réelle au SIH, et une infrastructure plus robuste.


	Quelle serait la prochaine étape la plus pertinente ?

Réponse courte : une phase pilote supervisée, avec validation humaine, enrichissement de l’évaluation et montée en charge progressive.


	Quelle est la principale valeur du projet aujourd’hui ?

Réponse courte : démontrer la faisabilité technique d’un agent IA de triage structuré, traçable et intégrable.


	Comment j’ai procédé pour préparer le vLLM ?

- Côté architecture
J’ai séparé l’API et le moteur d’inférence pour ne pas dépendre d’une seule manière d’exécuter le modèle.

- Côté compatibilité
J’ai prévu un backend compatible avec une logique de serveur vLLM de type OpenAI-compatible.

- Côté suite projet
L’étape suivante aurait été de déployer ce backend sur une machine Linux + GPU pour comparer les performances dans de meilleures conditions.

La préparation vLLM a surtout consisté à anticiper l’industrialisation côté architecture. Je n’ai pas figé l’API sur un seul moteur local, j’ai prévu plusieurs backends d’inférence pour pouvoir garder la même interface tout en changeant l’implémentation derrière. 

L’idée était de pouvoir passer d’une démo légère ou d’une exécution locale à un serveur vLLM plus performant sur Linux + GPU, sans refaire toute l’application. Donc ce que j’ai préparé, ce n’est pas un déploiement public complet vLLM, mais une compatibilité d’architecture pour cette suite.
