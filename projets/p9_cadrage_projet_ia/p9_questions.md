c'est quoi COMEX
→ Le comité exécutif de l’entreprise, qui prend les décisions stratégiques.

c'est quoi PoC (Proof of Concept)
→ Une expérimentation courte visant à vérifier qu’une solution est techniquement faisable.

c'est quoi un MVP (Minimum Viable Product)
→ Une première version fonctionnelle du produit, prête à être testée par de vrais utilisateurs.

C'est quoi Embedding
→ Une représentation numérique d’une image sous forme de vecteur qui capture ses caractéristiques visuelles.

C'est quoi Similarité
→ Une mesure mathématique qui permet d’évaluer à quel point deux éléments se ressemblent.

C'est quoi Recommandation = top K produits proches
→ Les K produits les plus proches ou pertinents selon un critère donné.

c'est quoi Vision Transformer (ViT)
→ Un modèle d’intelligence artificielle qui analyse les images en capturant les relations globales entre leurs différentes parties.

c'est quoi CNN (Convolutional Neural Network)
→ Un type de réseau de neurones spécialisé dans l’analyse d’images en détectant des motifs comme les formes ou les textures.

c'est quoi LLM (Large Language Model)
→ Un grand modèle d’intelligence artificielle entraîné sur du texte pour comprendre et générer du langage.

c'est quoi modèles de type CLIP
→ Un modèle capable de relier des images et du texte en les projetant dans un même espace de représentation.

c'est quoi modèles de type LLM multimodaux
→ Un modèle capable de comprendre plusieurs types de données à la fois, comme du texte et des images.

c'est quoi Recall@K et Precision@K comment ça se prononce à l'oral
Recall@K
→ La proportion d’éléments pertinents retrouvés parmi les K recommandations proposées.
(À l’oral : “Recall at K”)
→ La proportion d’éléments réellement pertinents parmi les K recommandations affichées.
(À l’oral : “Precision at K”)

c'est quoi le temps d’inférence
→ Le temps nécessaire au modèle pour produire une prédiction après réception d’une requête.

c'est quoi la scalabilité
→ La capacité d’un système à gérer une augmentation du nombre d’utilisateurs ou de requêtes.

c'est quoi KPI business
→ Un indicateur clé de performance permettant de mesurer l’impact du projet sur le business.

c'est quoi un Copil (Comité de pilotage)
→ Une réunion de gouvernance regroupant les décideurs pour suivre l’avancement du projet.

c'est quoi un Coproj (Comité projet)
→ Une réunion opérationnelle entre les équipes impliquées dans le développement du projet.

c'est quoi ROI (Return on Investment)
→ Le retour sur investissement, c’est-à-dire la rentabilité d’un projet par rapport à son coût.

c'est quoi le taux de conversion
→ Le pourcentage d’utilisateurs qui réalisent une action souhaitée, comme un achat.

c'est quoi le compute
→ Les ressources de calcul nécessaires pour faire fonctionner un modèle ou une application.

c'est quoi l'adoption
→ Le niveau d’utilisation réelle de la solution par les utilisateurs.

c'est quoi DPIA (Data Protection Impact Assessment)
→ Une analyse d’impact sur la protection des données permettant d’évaluer les risques RGPD d’un projet.

c'est quoi le staffing
→ Le staffing indique combien de temps chaque profil est mobilisé sur une phase donnée, exprimé en pourcentage de temps.


///////////////////////////////////////:::

///////////// QUESTIONS / REPONSES

///////////////////////////////////:::::::::

2)

	Pourquoi une application mobile et pas une simple fonctionnalité sur le site web ?

L’application mobile permet de faciliter l’usage de la photo et de créer une expérience plus fluide et immersive. Elle peut également renforcer l’engagement client et générer du trafic vers le site e-commerce.


3)

	Pourquoi commencer par un PoC et pas directement par un MVP ?

Parce que le cœur du projet repose sur la capacité du modèle à produire des recommandations pertinentes.
Avant d’investir dans un développement complet, il est essentiel de sécuriser cette faisabilité technique.



	Qu’est-ce qui ferait échouer ce PoC ?

Si les recommandations ne sont pas jugées suffisamment pertinentes par les équipes métier ou si les performances techniques sont insuffisantes, nous déciderions de ne pas industrialiser la solution.


4) 

	Pourquoi ne pas simplement améliorer nos filtres produits existants ?

Améliorer les filtres peut aider, mais cela reste basé sur des catégories déclaratives.
L’IA permet d’exploiter des signaux visuels complexes qui ne sont pas toujours capturés dans les métadonnées produits, comme l’harmonie d’un look ou le style global.
C’est ce différentiel qui crée la valeur.



	Est-ce que ce n’est pas technologiquement surdimensionné ?

Justement, le PoC est là pour vérifier que la complexité technique apporte une réelle valeur métier, avant toute industrialisation.


5)

	Pourquoi ne pas intégrer directement les préférences utilisateur ?

Parce que cela ajouterait une complexité supplémentaire.
Le risque principal est d’abord technique : est-ce que le modèle comprend correctement le style visuel ?
Une fois cette capacité validée, on pourra enrichir avec la personnalisation.


	Pourquoi top N et pas un seul produit ?

Proposer un top N augmente la probabilité de pertinence perçue et laisse plus de liberté au client, tout en restant simple techniquement.


6) 
	Pourquoi pré-entraîné ?
- Gain de temps
- Pas besoin de dataset gigantesque
- Suffisant pour un PoC


	C’est quoi un embedding ?

Une représentation vectorielle qui encode les caractéristiques visuelles d’une image.


	Pourquoi recherche de similarité ?

Parce qu’on ne classe pas une catégorie, on cherche les produits les plus proches visuellement.

	
	Pourquoi ne pas entraîner un modèle from scratch ?

Cela nécessiterait un volume de données très important et un temps d’entraînement long.
Pour un PoC, utiliser un modèle pré-entraîné permet de réduire le risque et le délai.


	Pourquoi Vision Transformer plutôt qu’un CNN classique ?

Les Vision Transformers ont montré de très bonnes performances sur des tâches de représentation visuelle globale.
Cependant, pour un PoC, les deux approches sont envisageables, et nous pourrions comparer leurs performances.


	Comment mesurez-vous la similarité ?

En calculant une distance entre vecteurs, par exemple la distance cosinus.


7)

	Pourquoi ne pas utiliser directement un LLM multimodal ?

Les LLM multimodaux sont plus coûteux en compute et en latence.
Pour un PoC, il est plus prudent de valider d’abord la performance d’une approche plus simple et maîtrisée.

	
	Est-ce que cette alternative serait plus performante ?

Potentiellement sur la compréhension sémantique, mais pas nécessairement sur la similarité visuelle pure.
L’intérêt serait surtout d’enrichir l’expérience utilisateur dans une phase ultérieure.


	Pourquoi l’avoir mentionnée si vous ne l’utilisez pas ?

Parce qu’il est important de montrer que nous avons exploré plusieurs options techniques avant de choisir l’approche la plus adaptée au périmètre et au délai du PoC


8) 
	
	Les images catalogue suffisent-elles vraiment ?

Pour un PoC, oui.
L’objectif est de tester la capacité du modèle à identifier des similarités visuelles entre produits existants.


	Pourquoi ne pas utiliser une API externe comme OpenAI Vision ?

Pour des raisons de maîtrise des coûts et de protection des données, il est préférable d’héberger le modèle dans l'environnement Azure de l'entreprise.


	Pourquoi mentionner des datasets publics ?

Pour améliorer la robustesse et éviter le surapprentissage sur un catalogue limité.


9) 

	Quel seuil de Recall@K considérez-vous acceptable ?

Pour un PoC, nous viserions un niveau significativement supérieur à un benchmark aléatoire, par exemple supérieur à 60–70 %, mais la validation finale reste métier


	Pourquoi ne pas mesurer directement l’impact sur le chiffre d’affaires ?

Parce que le PoC vise à valider la faisabilité technique.
L’impact business sera mesuré lors du pilote en conditions réelles.


	Comment testez-vous la généralisation ?

En évaluant les performances sur différents styles et catégories produits non vus pendant l’entraînement.


10)

	Combien de temps dure chaque étape ?

L’expérimentation représente la part la plus importante, car elle nécessite plusieurs itérations.
Les phases de cadrage et de synthèse sont plus courtes mais essentielles pour structurer la décision.


	Pourquoi tester plusieurs modèles si vous avez déjà choisi une approche ?

Même en privilégiant une approche basée sur Vision Transformers, il est important de comparer différentes configurations pour identifier la plus performante.


	Comment décidez-vous du Go / No Go ?

Sur la base des critères techniques définis précédemment et de la validation métier.



12 ) Données et profils


	Pourquoi 4 à 6 semaines et pas 2 ?

Parce qu’un PoC nécessite plusieurs itérations techniques et des validations métier.
Réduire davantage le délai augmenterait le risque d’une évaluation insuffisante.


	Pourquoi un seul Data Scientist ?

Le périmètre est volontairement limité à une seule fonctionnalité.
Un Data Scientist dédié est suffisant pour un PoC, à condition d’avoir un support data engineering.



	Et si le modèle ne fonctionne pas ?

Le PoC est justement conçu pour identifier ce risque rapidement et à coût maîtrisé.



13 )

	Pourquoi ne pas faire tout dans un seul service ?

Séparer les briques permet de scaler indépendamment et de faciliter la maintenance.



	Quel est le point critique de cette architecture ?

Le service d’inférence, car il concentre la charge compute et impacte la latence.



	Comment gérez-vous la montée en charge ?

En utilisant des services managés Azure avec autoscaling sur les endpoints ML.


	Pourquoi séparer moteur de similarité et modèle IA ?

Le modèle extrait les caractéristiques visuelles.
Le moteur de similarité effectue la comparaison mathématique.
Cette séparation permet d’optimiser et scaler chaque composant indépendamment.


	Où se situe le catalogue produit ?

Le catalogue est indexé en amont sous forme d’embeddings, ce qui permet une recherche rapide au moment de la requête.


	Quel est le point critique en production ?

L’inférence du modèle, car elle impacte directement la latence et les coûts compute.


14)

	Pourquoi ne pas faire la recherche directement dans Azure ML ?

Azure ML gère l’inférence.
La recherche vectorielle peut être optimisée séparément pour améliorer les performances et la scalabilité.



	Où se situe la scalabilité ?

Au niveau de l’inférence et de la recherche vectorielle, qui peuvent être autoscalées selon le trafic.



	Quel est le point le plus coûteux ?

L’inférence modèle, car elle mobilise du compute GPU ou CPU intensif.


15)

	Que se passe-t-il si le trafic double ?

Les endpoints peuvent autoscaler.
Les coûts augmentent proportionnellement à l’usage, mais restent maîtrisables car variables.


17) 

	Pourquoi 6 mois ?

Parce que nous intégrons une phase d’industrialisation complète et un pilote métier réel.
Réduire le délai augmenterait le risque de déploiement précipité.


	Pourquoi 8 semaines pour le PoC ?

Le développement ML nécessite plusieurs itérations et validations métier.
Huit semaines permettent d’expérimenter sérieusement sans surinvestir


	Pourquoi une phase MVP séparée du PoC ?

Le PoC valide la faisabilité technique.
Le MVP transforme cette faisabilité en solution industrialisée.


	Pourquoi du MLOps uniquement à partir de la phase 3 ?

Parce que la complexité opérationnelle apparaît surtout à la mise en production et au monitoring.


18)

	Pourquoi 100 % et pas 80 % ?

La phase d’expérimentation nécessite une forte disponibilité pour enchaîner rapidement les itérations.

	
	Pourquoi pas de Data Scientist à 100 % en phase MVP ?

L’effort principal devient opérationnel.
Le Data Scientist intervient en support ponctuel.


	Pourquoi ne pas garder le MLOps dès le PoC ?

Le PoC reste un environnement simplifié.
La complexité MLOps devient critique lors de la mise en production.


19) 

	Pourquoi un groupe témoin ?

Pour isoler l’impact réel de la recommandation IA et éviter les biais liés à la saisonnalité ou au trafic.
Le groupe témoin permet de vérifier que l’amélioration observée est bien due à l’IA, et non à un facteur externe comme une promotion ou une période de forte activité.


	Combien d’utilisateurs au pilote ?

Un échantillon représentatif mais limité, par exemple 10 à 20 % du trafic, afin d’obtenir des résultats significatifs tout en limitant le risque.

	
	Que se passe-t-il si les résultats sont mitigés ?

Nous pouvons ajuster le modèle ou les règles métier avant une extension plus large.


20)

	Pourquoi 120 jours Data Scientist ?

Cela correspond à la phase de développement intensif du modèle, avec plusieurs cycles de tests et ajustements.



	Pourquoi seulement 20 000 € par an en run ?

Le modèle est principalement sollicité en inférence, pas en entraînement continu.
Les coûts sont donc proportionnels au trafic.


	Et si le trafic double ?

Les coûts compute augmenteraient proportionnellement, mais restent variables et maîtrisables.


	Pourquoi ne pas externaliser plutôt que mobiliser des ressources internes ?

Les compétences sont déjà présentes en interne, ce qui réduit les coûts et facilite la montée en compétence durable.


21)

	Ces 14 % sont-ils réalistes ?

Ce sont des estimations marketing.
Même si la performance réelle était inférieure, la marge entre coûts et gains reste confortable.


	Et si l’adoption est plus lente ?

Le modèle économique reste favorable car les coûts sont majoritairement one-shot et les coûts run restent limités.


	Pourquoi inclure les magasins si c’est une app mobile ?

L’application peut générer du trafic omnicanal, notamment via le click-and-collect ou la découverte de produits en magasin.


	Que se passe-t-il si les gains sont divisés par deux ?

Même avec une performance divisée par deux, le projet resterait rentable sur un horizon légèrement plus long.


22)

	Utilisez-vous les images pour entraîner un modèle ?

Non. Les images utilisateurs ne sont pas utilisées pour entraîner un modèle tiers.
Elles sont utilisées uniquement pour générer des embeddings nécessaires à la recommandation.


	Combien de temps conservez-vous les images ?

Une durée limitée et définie contractuellement, avec suppression automatique après traitement ou inactivité.


	Pourquoi ne pas utiliser un LLM externe ?

Pour garantir la maîtrise des données et éviter toute réutilisation non souhaitée.


23)

	Comment prouvez-vous que les embeddings ne permettent pas la ré-identification ?

Les embeddings sont des représentations mathématiques compressées, non exploitables directement pour reconstruire une image.
Toutefois, une analyse d’impact (DPIA) pourrait être réalisée avant mise en production complète.


	Avez-vous prévu une DPIA ?

Oui, une analyse d’impact serait prévue en phase MVP avant déploiement à grande échelle.