
	VOCABULAIRE /////////////////////

-------------- (rapide)


Spécialisation du modèle

La spécialisation du modèle, c’est le fait de partir d’un LLM généraliste et de l’adapter à un usage précis, ici le triage médical. L’objectif était d’obtenir des réponses plus utiles, plus prudentes et plus structurées. Pour ça, j’ai utilisé un SFT avec LoRA, puis un alignement DPO. 


Garde-fous de sécurité

Les garde-fous de sécurité servent à éviter qu’un modèle génératif prenne trop de liberté sur des cas sensibles. Dans mon POC, j’ai mis en place des red flags. Si un symptôme grave est détecté, la priorité est forcée et le système renvoie une réponse prudente sans laisser le modèle improviser.


Traçabilité

La traçabilité, c’est le fait de conserver l’historique des interactions pour pouvoir relire et auditer le comportement du système. Dans le projet, chaque interaction est enregistrée dans une base SQLite et peut être retrouvée via l’API.


Automatisation

L’automatisation permet de sécuriser l’évolution du projet. J’ai mis en place une pipeline GitHub Actions qui vérifie automatiquement la qualité du code, les tests et la conteneurisation.


SFT

Le SFT, c’est l’étape où on apprend au modèle à produire le type de réponses attendu à partir d’exemples supervisés.


DPO
Le DPO sert à aligner le modèle en lui apprenant quelles réponses sont préférables à d’autres.


Splits train / validation / test
Les splits permettent de séparer l’apprentissage, le suivi de l’entraînement et l’évaluation finale, pour éviter de tout mélanger.


LoRA
LoRA permet de fine-tuner un modèle de manière beaucoup plus légère, ce qui était important dans mon environnement contraint.

Loss
C’est l’erreur du modèle pendant l’apprentissage.
Plus elle baisse, plus le modèle apprend à mieux reproduire ce qu’on attend de lui sur les données d’entraînement.


Eval loss
C’est la même idée, mais mesurée sur des données de validation que le modèle n’a pas utilisées pour apprendre directement.
C’est plus intéressant à montrer en soutenance, parce que ça permet de voir si le modèle s’améliore aussi sur des données non vues.


Batch size faible
J’ai utilisé un batch size faible pour rester compatible avec la mémoire limitée du GPU.


Accumulation de gradient
L’accumulation de gradient permet de compenser un batch très petit en cumulant plusieurs petites étapes avant d’actualiser le modèle.


Variantes LoRA low-VRAM
J’ai utilisé des variantes LoRA low-VRAM pour réduire encore la consommation mémoire et rendre l’entraînement réaliste sur ma machine.


Backend
c’est simplement le “moteur” utilisé derrière l’API pour produire la réponse.


stub
Le backend stub simule le comportement du service sans charger le vrai modèle, ce qui est utile pour la démo et les tests.


transformers + PEFT
Le backend transformers + PEFT correspond à l’inférence locale avec le vrai modèle et ses adapters LoRA.


PEFT
PEFT signifie Parameter-Efficient Fine-Tuning. C’est une approche qui permet d’utiliser un modèle spécialisé de manière plus légère, notamment avec LoRA.


vllm-openai
Le backend vLLM correspond à la trajectoire d’industrialisation envisagée pour servir le modèle plus efficacement sur une infrastructure GPU adaptée.


P50
C’est la médiane.
Ça veut dire : 50 % des requêtes sont plus rapides que cette valeur, et 50 % sont plus lentes.

Local réel : P50 ~13,5 s = avec le vrai modèle local, une requête prend environ 13,5 secondes en médiane
Cloud stub : P50 ~0,23 s = avec le backend léger sur le cloud, une requête prend environ 0,23 seconde en médiane


Lint
Le lint, c’est une vérification automatique du code pour détecter des problèmes de qualité ou de cohérence.


Ruff
Ruff est l’outil qui vérifie automatiquement la qualité du code Python dans le projet.


Recueil initial
Le recueil initial, c’est la première collecte d’informations patient, par exemple les symptômes, le contexte et les signes de gravité.


Inférence
C’est le moment où on utilise un modèle déjà entraîné pour produire une réponse sur une nouvelle entrée.
L’inférence, c’est l’utilisation du modèle une fois entraîné, pour générer une réponse sur un nouveau cas.


POC
Un Proof of Concept, c’est un prototype qui sert à démontrer qu’une solution est techniquement faisable.


----------------- (plus détaillé)


Spécialisation du modèle

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


Traçabilité

C’est la capacité à garder une trace de ce que le système a reçu, produit et décidé.
Ça sert à pouvoir auditer le comportement du système, comprendre ses décisions, analyser les cas à risque et justifier ce qui s’est passé.
J'ai mis en place un audit des interactions dans SQLite, avec un endpoint dédié pour relire une interaction par identifiant. Cela permet de retrouver les échanges et les décisions prises par le système.


Automatisation

C’est le fait d’automatiser les contrôles techniques du projet pour éviter les régressions et rendre le système plus maintenable.
Ça sert à fiabiliser le projet dans le temps, en vérifiant automatiquement que le code reste propre, testable et déployable.
J'ai mis en place un pipeline GitHub Actions qui exécute automatiquement :
- le lint avec Ruff
- les tests avec Pytest
- la vérification du build Docker


SFT

Le SFT, ou Supervised Fine-Tuning, c’est une phase d’entraînement supervisé où le modèle apprend à mieux répondre à partir d’exemples question-réponse déjà préparés.
Ça sert à spécialiser un modèle généraliste sur une tâche précise.


DPO

Le DPO, ou Direct Preference Optimization, c’est une méthode d’alignement qui apprend au modèle à préférer une meilleure réponse plutôt qu’une moins bonne, à partir de paires de réponses.
Ça sert à rendre le comportement du modèle plus cohérent, plus prudent et plus conforme aux attentes.


Splits train / validation / test

Ce sont les trois sous-ensembles du dataset.
- train sert à entraîner le modèle
- validation sert à suivre l’apprentissage et ajuster les choix
- test sert à évaluer le comportement final sur des données non vues


LoRA

LoRA, ou Low-Rank Adaptation, est une méthode qui permet d’adapter un grand modèle sans réentraîner tous ses paramètres.
On n’ajoute et n’entraîne qu’une petite partie, ce qui réduit fortement le coût mémoire et GPU.


Batch size faible

Le modèle traite très peu d’exemples à la fois, ici parce que la mémoire GPU est limitée.
À faire rentrer l’entraînement dans la mémoire disponible.


Accumulation de gradient

Comme on ne peut pas traiter beaucoup d’exemples d’un coup, on cumule l’effet de plusieurs petites itérations avant de faire une vraie mise à jour des poids.
À simuler un batch plus grand sans avoir besoin de toute la mémoire correspondante.


Variantes LoRA low-VRAM

LoRA sert déjà à alléger le fine-tuning. Les variantes low-VRAM vont encore plus loin en réduisant certains réglages, par exemple la taille des séquences, le rang LoRA ou les modules ciblés, pour consommer moins de mémoire.
À rendre l’entraînement faisable sur une carte graphique modeste.


stub

C’est un faux backend, très léger. Il ne fait pas tourner le vrai modèle lourd.
Il sert surtout à :
- faire une démo rapide
- faire passer la CI
- garder une API réactive dans un environnement limité


transformers + PEFT

Là, on est sur le vrai backend local.
- transformers = la bibliothèque Hugging Face qui permet de charger et faire tourner un modèle
- PEFT = la bibliothèque qui permet d’utiliser les adapters LoRA sans recharger ou réentraîner un gros modèle complet


vllm-openai

Ça, ce n’est pas ton déploiement actuel principal en cloud gratuit.
C’est plutôt la voie prévue pour aller vers quelque chose de plus performant.

vLLM est un moteur d’inférence optimisé pour servir des LLM plus rapidement
openai ici veut dire que ton API peut parler à un serveur vLLM via une interface compatible OpenAI


Lint

Le lint, c’est le contrôle automatique de la qualité du code.
Ça sert à repérer :
- les erreurs de style
- les incohérences
- certaines erreurs simples avant même l’exécution


Ruff

Ruff est l’outil de lint que tu as utilisé pour ton projet Python.
Il sert à analyser rapidement le code et signaler des problèmes de style ou de qualité.


Recueil initial

- la collecte des premières informations du patient
- les symptômes décrits
- les éléments de contexte utiles au triage