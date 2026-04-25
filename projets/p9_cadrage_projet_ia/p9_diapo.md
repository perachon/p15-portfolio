Réalisez le cadrage d'un projet IA
Application mobile de recommandation vestimentaire par photo


Présentation de cadrage – PoC IA

Fashion-Insta : réseau de magasins 
		+ e-commerce

Forte concurrence sur l’expérience client digitale

Difficulté pour les clients à trouver des articles similaires à leur style

Opportunité : utiliser la photo comme point d’entrée


L’entreprise évolue dans un marché très concurrentiel, où l’expérience client digitale est devenue un levier majeur de différenciation.
Aujourd’hui, un client peut aimer un style ou une tenue, mais il ne sait pas toujours comment la retrouver ou l’exprimer via des mots-clés sur notre site.L’opportunité ici est d’exploiter un usage simple et naturel : la photo, afin de proposer des recommandations pertinentes et directement exploitables commercialement.

------------------------

	Pourquoi une application mobile et pas une simple fonctionnalité sur le site web ?

L’application mobile permet de faciliter l’usage de la photo et de créer une expérience plus fluide et immersive. Elle peut également renforcer l’engagement client et générer du trafic vers le site e-commerce.


-----------

Objectif du PoC IA

Démontrer la faisabilité technique de la recommandation par image

Valider la valeur métier de l’IA pour Fashion-Insta

Périmètre volontairement limité

Pas un produit final



L’objectif de ce Proof of Concept n’est pas de livrer une application complète mais de répondre à une question simple : est-ce que l’IA est capable de comprendre un style vestimentaire à partir d’une photo et de proposer des recommandations pertinentes ?
Ce PoC vise donc à valider la faisabilité technique, mais aussi la valeur métier de cette approche.Le périmètre est volontairement limité : il ne s’agit pas de développer l’application complète, mais de tester la brique IA centrale, qui conditionne tout le reste.


-----------------

	Pourquoi commencer par un PoC et pas directement par un MVP ?

Parce que le cœur du projet repose sur la capacité du modèle à produire des recommandations pertinentes.
Avant d’investir dans un développement complet, il est essentiel de sécuriser cette faisabilité technique.


	Qu’est-ce qui ferait échouer ce PoC ?

Si les recommandations ne sont pas jugées suffisamment pertinentes par les équipes métier ou si les performances techniques sont insuffisantes, nous déciderions de ne pas industrialiser la solution.


----

Valeur ajoutée de l’IA

Compréhension visuelle du style (coupe, couleur, texture)

Dépasse les filtres classiques (catégories, mots-clés)

Expérience client plus intuitive

Différenciation concurrentielle forte
L’intérêt de l’IA ici est de dépasser les limites des filtres classiques basés sur des catégories ou des mots-clés.
Un moteur traditionnel fonctionne si l’utilisateur sait exactement ce qu’il cherche.
Avec la vision par ordinateur, on analyse directement les caractéristiques visuelles d’une tenue : couleurs, textures, coupes, associations.
Cela permet de proposer des recommandations plus naturelles, plus intuitives, et donc plus engageantes pour le client.


--------------

	Pourquoi ne pas simplement améliorer nos filtres produits existants ?

Améliorer les filtres peut aider, mais cela reste basé sur des catégories déclaratives.
L’IA permet d’exploiter des signaux visuels complexes qui ne sont pas toujours capturés dans les métadonnées produits, comme l’harmonie d’un look ou le style global.
C’est ce différentiel qui crée la valeur.


	Est-ce que ce n’est pas technologiquement surdimensionné ?

Justement, le PoC est là pour vérifier que la complexité technique apporte une réelle valeur métier, avant toute industrialisation.

-------

Fonctionnalité centrale du PoC

Entrée : photo prise par l’utilisateur

Sortie : top N d’articles visuellement similaires

Analyse purement visuelle

Pas de personnalisation utilisateur dans le PoC


Pour ce PoC, on va se concentrer uniquement sur la brique centrale : la recommandation d’articles à partir d’une photo
Concrètement, l’utilisateur envoie une image, et le système retourne un top N d’articles visuellement similaires.
On fait volontairement une analyse purement visuelle, sans intégrer pour l’instant les préférences utilisateurs ou l’historique d’achat.
L’objectif est de valider la capacité du modèle à comprendre le style, avant d’ajouter d’autres dimensions. Si cette brique ne fonctionne pas, le reste de l’application n’a pas de sens. C’est donc le bon point de départ

-------------------------

	Pourquoi ne pas intégrer directement les préférences utilisateur ?

Parce que cela ajouterait une complexité supplémentaire.
Le risque principal est d’abord technique : est-ce que le modèle comprend correctement le style visuel ?
Une fois cette capacité validée, on pourra enrichir avec la personnalisation.


	Pourquoi top N et pas un seul produit ?

Proposer un top N augmente la probabilité de pertinence perçue et laisse plus de liberté au client, tout en restant simple techniquement.


---

Approche technique IA

- Modèle de vision par ordinateur pré-entraîné (Vision Transformers / CNN)
- Extraction d’empreintes visuelles (embeddings)
- Recherche de similarité dans le catalogue
- Approche rapide et robuste

Techniquement, on va utiliser un modèle de vision par ordinateur pré-entraîné, de type Vision Transformer ou CNN sur des images de vêtements.
Ce modèle permet de transformer chaque image en une représentation numérique, appelée embedding.
On compare ensuite cet embedding à ceux du catalogue produit afin d’identifier les articles les plus similaires visuellement.
Cette approche est rapide à mettre en œuvre, robuste et particulièrement adaptée à un PoC.


---------------------

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


-----

Approche alternative

Modèle multimodal image + texte
Plus riche sémantiquement
Plus coûteux et complexe
Option à explorer selon résultats

En alternative pour une seconde approche, il serait possible d’utiliser un modèle multimodal capable de comprendre à la fois l’image et une description textuelle du style (comme des modèles de type CLIP ou des LLM multimodaux)
Cette approche permettrait de capter des dimensions plus sémantiques du style, par exemple un “look casual chic” ou “tenue professionnelle”.
Cette solution offre un potentiel plus riche, Par contre, elle est plus coûteuse, plus complexe à mettre en œuvre et moins nécessaire pour un PoC.
C’est pourquoi on va privilégier une approche plus simple dans un premier temps.

-----------------------------------

	Pourquoi ne pas utiliser directement un LLM multimodal ?

Les LLM multimodaux sont plus coûteux en compute et en latence.
Pour un PoC, il est plus prudent de valider d’abord la performance d’une approche plus simple et maîtrisée.

	
	Est-ce que cette alternative serait plus performante ?

Potentiellement sur la compréhension sémantique, mais pas nécessairement sur la similarité visuelle pure.
L’intérêt serait surtout d’enrichir l’expérience utilisateur dans une phase ultérieure.


	Pourquoi l’avoir mentionnée si vous ne l’utilisez pas ?

Parce qu’il est important de montrer que nous avons exploré plusieurs options techniques avant de choisir l’approche la plus adaptée au périmètre et au délai du PoC


---------


Données & outils

Images produits du catalogue
Datasets publics de mode
Cloud Microsoft Azure
Pas de données personnelles clients


Pour entraîner et tester le modèle, on va utiliser principalement les images produits déjà présentes dans le catalogue de l’entreprise.
Si besoin, on pourra compléter avec des datasets publics spécialisés dans la mode afin d’améliorer la robustesse du modèle.
L’ensemble de la solution est déployé sur Microsoft Azure, partenaire cloud de l’entreprise.
Et, dans le cadre du PoC, aucune donnée personnelle utilisateur n’est utilisée à ce stade, ce qui réduit fortement les risques réglementaires.

-------------------------------

	Les images catalogue suffisent-elles vraiment ?

Pour un PoC, oui.
L’objectif est de tester la capacité du modèle à identifier des similarités visuelles entre produits existants.


	Pourquoi ne pas utiliser une API externe comme OpenAI Vision ?

Pour des raisons de maîtrise des coûts et de protection des données, il est préférable d’héberger le modèle dans l'environnement Azure de l'entreprise.


	Pourquoi mentionner des datasets publics ?

Pour améliorer la robustesse et éviter le surapprentissage sur un catalogue limité.




-----

Critères de succès du PoC

Recommandations jugées pertinentes par les métiers
Temps de réponse acceptable
Capacité à généraliser à différents styles
Décision Go / No Go
Critères techniques
	Recall@K sur le top 5 produits
	Precision@K
	Temps d’inférence < 500 ms

Le succès du PoC sera évalué à la fois sur des critères techniques et métier.
D’un point de vue technique, on va mesurer la performance du modèle avec des métriques comme Recall@K et Precision@K, ainsi que le temps d’inférence.
D’un point de vue métier, les équipes évalueront la pertinence perçue des recommandations sur différents styles vestimentaires.
L’objectif est de croiser ces deux dimensions pour validation du projet avant toute décision d’industrialisation.

--------------------------------

	Quel seuil de Recall@K considérez-vous acceptable ?

Pour un PoC, nous viserions un niveau significativement supérieur à un benchmark aléatoire, par exemple supérieur à 60–70 %, mais la validation finale reste métier


	Pourquoi ne pas mesurer directement l’impact sur le chiffre d’affaires ?

Parce que le PoC vise à valider la faisabilité technique.
L’impact business sera mesuré lors du pilote en conditions réelles.


	Comment testez-vous la généralisation ?

En évaluant les performances sur différents styles et catégories produits non vus pendant l’entraînement.


-------

Déroulé du PoC

- Cadrage et préparation des données
- Tests de plusieurs modèles
- Évaluation métier
- Synthèse et recommandations

Le PoC se déroule en quatre étapes principales :
D’abord, un cadrage technique et une préparation des données, afin de garantir la qualité des entrées du modèle.
Ensuite, une phase d’expérimentation où on va tester plusieurs approches de modélisation et de similarité.
Puis, une évaluation croisée technique et métier des résultats obtenus.
Pour terminer sur une synthèse et recommandations.

---------------------------

	Combien de temps dure chaque étape ?

L’expérimentation représente la part la plus importante, car elle nécessite plusieurs itérations.
Les phases de cadrage et de synthèse sont plus courtes mais essentielles pour structurer la décision.


	Pourquoi tester plusieurs modèles si vous avez déjà choisi une approche ?

Même en privilégiant une approche basée sur Vision Transformers, il est important de comparer différentes configurations pour identifier la plus performante.


	Comment décidez-vous du Go / No Go ?

Sur la base des critères techniques définis précédemment et de la validation métier.

-----------

Positionnement du PoC dans le cycle produit

PoC : validation faisabilité technique (ce projet)
MVP : première version utilisable par des clients
Run : exploitation industrielle et amélioration continue

Le projet que je présente aujourd’hui correspond uniquement à la phase PoC, dont l’objectif est de valider la faisabilité technique.
Si les résultats sont concluants, on passera ensuite à une phase MVP, puis à une phase Run.

----------

Durée et profils nécessaires

Durée estimée : 4 à 8 semaines (PoC)
1 Data Scientist
1 AI Engineer / Data Engineer
1 référent métier mode


Le PoC est conçu pour être court et maîtrisé, avec une durée estimée entre 4 et 8 semaines.
Il mobilise principalement un Data Scientist pour la modélisation, un Data Engineer pour les pipelines et la gestion des données, ainsi qu’un référent métier qui est indispensable pour valider la pertinence des recommandations.

Cette organisation permet de limiter les risques et donc limiter les coûts tout en maximisant la qualité des résultats.

--------------

	Pourquoi 4 à 8 semaines et pas 2 ?

Parce qu’un PoC nécessite plusieurs itérations techniques et des validations métier.
Réduire davantage le délai augmenterait le risque d’une évaluation insuffisante.


	Pourquoi un seul Data Scientist ?

Le périmètre est volontairement limité à une seule fonctionnalité.
Un Data Scientist dédié est suffisant pour un PoC, à condition d’avoir un support data engineering.


	Et si le modèle ne fonctionne pas ?

Le PoC est justement conçu pour identifier ce risque rapidement et à coût maîtrisé.


-------

System Design – Vue d’ensemble

Schéma simplifié end-to-end

Utilisateur → IA → Recommandations

Cette slide donne une vue d’ensemble du fonctionnement de la solution, sans entrer dans le détail technique.

*clic*L’utilisateur envoie une photo via l’application mobile => Cette image est ingérée via une API sécurisée, puis stockée temporairement.

*clic*Un modèle de vision par ordinateur extrait un embedding représentant le style => Cet embedding est comparé au catalogue via un moteur de similarité.

*clic*Les recommandations sont ensuite filtrées selon des règles métier avant d’être renvoyées à l’utilisateur.


---------------------


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



----------

Détail des briques Data & IA

Liste structurée des briques techniques
Mapping Azure



Pour le détail des briques techniques

*click*L’ingestion API permet de recevoir les images de manière sécurisée.Les images sont stockées temporairement (dans Azure Blob Storage).

*click*Un pré-traitement des images avant l’inférence.Le modèle est déployé via Azure ML pour générer les embeddings.

*click*La recherche vectorielle permet d’identifier les produits les plus similaires.Et on termine sur un système de monitoring assure le suivi des performances et des coûts.

---------------------------

	Pourquoi ne pas faire la recherche directement dans Azure ML ?

Azure ML gère l’inférence.
La recherche vectorielle peut être optimisée séparément pour améliorer les performances et la scalabilité.


	Où se situe la scalabilité ?

Au niveau de l’inférence et de la recherche vectorielle, qui peuvent être autoscalées selon le trafic.


	Quel est le point le plus coûteux ?

L’inférence modèle, car elle mobilise du compute GPU ou CPU intensif.


-----

Scalabilité & maîtrise des coûts

Scalabilité selon trafic marketing
Services managés Azure
Coûts maîtrisables
Approche progressive



La solution est scalable dès sa conception.
L’inférence est déployée sur Azure ML avec autoscaling selon la charge.
Les principaux coûts concernent : le compute et le stockage.
Les services managés permettent d’ajuster les ressources et d’éviter tout surdimensionnement.

--------------

	Que se passe-t-il si le trafic double ?

Les endpoints peuvent autoscaler.
Les coûts augmentent proportionnellement à l’usage, mais restent maîtrisables car variables.


---------------

Rôles & responsabilités Data & IA

Ici je détaille la répartition des responsabilités entre les différents profils Data.
Le Data Scientist est responsable du développement et de l’évaluation du modèle.
Le Data Engineer gère les pipelines et la structuration des données.
Le MLOps Engineer prend en charge le déploiement et le monitoring en production.
Le Tech Lead assure la cohérence globale de l’architecture et les choix technologiques.

----
Timeline globale du projet

Cette timeline présente une trajectoire progressive sur environ 6 mois.

On commence par 3 semaines de cadrage pour sécuriser le périmètre et les données.

La phase PoC, sur 8 semaines, est dédiée au développement et aux tests du modèle.

Si les résultats sont validés, la phase MVP permet l’industrialisation sur Azure (6 semaines)

Enfin, la phase Run correspond au pilote métier et à la mesure du ROI.

Pour la gouvernance projet, des Copil et Coproj réguliers assurent l’alignement entre Data et métier.

-------------------------------------------------------------------------------------------------------------

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


	Pourquoi 6 semaines pour le MVP ?

Parce qu’il s’agit de transformer un prototype fonctionnel en solution robuste et déployée en environnement cloud.


	Pourquoi 4 semaines pour le Run ?

Parce qu’il faut un temps d’observation suffisant pour mesurer les indicateurs business sans allonger inutilement le délai.


----
Justification des taux de staffing & livrables

Les taux de staffing sont adaptés aux besoins de chaque phase.

En phase de développement, le Data Scientist est mobilisé à plein temps car le modèle doit être testé et ajusté plusieurs fois.

En phase d’industrialisation, le rôle principal revient au MLOps, responsable du déploiement et du monitoring.

Enfin, la phase pilote mobilise moins les équipes Data, car l’objectif devient principalement business.

-------------------------------------------------------------------------------------------------------------



	Pourquoi 100 % et pas 80 % ?

La phase d’expérimentation nécessite une forte disponibilité pour enchaîner rapidement les itérations.

	
	Pourquoi pas de Data Scientist à 100 % en phase MVP ?

L’effort principal devient opérationnel.
Le Data Scientist intervient en support ponctuel.


	Pourquoi ne pas garder le MLOps dès le PoC ?

Le PoC reste un environnement simplifié.
La complexité MLOps devient critique lors de la mise en production.



----

Industrialisation / Run

Stratégie de déploiement progressif (MVP → Run)


Déploiement pilote sur un sous-ensemble d’utilisateurs

Comparaison des performances avec un groupe témoin

Suivi des métriques techniques et métier

Extension progressive à l’ensemble des utilisateurs


Après la phase MVP, il faudra adopter une stratégie de déploiement progressive.

On commence par un pilote sur un sous-ensemble d’utilisateurs afin de limiter les risques.
Les performances sont comparées à un groupe témoin pour mesurer l’impact réel.

On suivra à la fois des métriques techniques, comme la latence, et des KPI métier, etc. (comme le taux de conversion).

l’extension est progressive à l’ensemble des utilisateurs => si les résultats sont concluants.

--------------------------

	Pourquoi un groupe témoin ?

Pour isoler l’impact réel de la recommandation IA et éviter les biais liés à la saisonnalité ou au trafic.


	Combien d’utilisateurs au pilote ?

Un échantillon représentatif mais limité, par exemple 10 à 20 % du trafic, afin d’obtenir des résultats significatifs tout en limitant le risque.

	
	Que se passe-t-il si les résultats sont mitigés ?

Nous pouvons ajuster le modèle ou les règles métier avant une extension plus large.



--------
Le coût du projet repose en majorité sur les ressources humaines internes, pour un montant d’environ 116 500 €

À cela on ajoute 10 000 € de coûts techniques one-shot pour l’infrastructure.

En phase Run, les coûts récurrents liés au compute et au stockage sont estimés à environ 20 000 € par an

*click*
Le coût total la première année est donc d’environ 146 500 €.
Sachant qu’on a retenu des hypothèses prudentes pour ne pas sous-estimer le budget et éviter d’avoir des mauvaises surprises à la fin.

---------------

	Pourquoi 120 jours Data Scientist ?

Cela correspond à la phase de développement intensif du modèle, avec plusieurs cycles de tests et ajustements.


	Pourquoi seulement 20 000 € par an en run ?

Le modèle est principalement sollicité en inférence, pas en entraînement continu.
Les coûts sont donc proportionnels au trafic.


	Et si le trafic double ?

Les coûts compute augmenteraient proportionnellement, mais restent variables et maîtrisables.


	Pourquoi ne pas externaliser plutôt que mobiliser des ressources internes ?

Les compétences sont déjà présentes en interne, ce qui réduit les coûts et facilite la montée en compétence durable.



----
L’estimation du ROI repose sur les hypothèses du marketing.

Avec une hausse attendue de 14 % sur le web et 4 % en magasin, le gain annuel est estimé à environ 468 000 €.

En comparant gains et coûts cumulés, la rentabilité est atteinte dès la première année.
Même avec des hypothèses prudentes, le projet génère rapidement une valeur nette positive.

(click)
Ici le graphique Matplotlib illustre le croisement des gains et des coûts cumulés : le point de rentabilité est atteint avant la fin de première l’année.

--------------------------------------------

	Ces 14 % sont-ils réalistes ?

Ce sont des estimations marketing.
Même si la performance réelle était inférieure, la marge entre coûts et gains reste confortable.


	Et si l’adoption est plus lente ?

Le modèle économique reste favorable car les coûts sont majoritairement one-shot et les coûts run restent limités.


	Pourquoi inclure les magasins si c’est une app mobile ?

L’application peut générer du trafic omnicanal, notamment via le click-and-collect ou la découverte de produits en magasin.


	Que se passe-t-il si les gains sont divisés par deux ?

Même avec une performance divisée par deux, le projet resterait rentable sur un horizon légèrement plus long.


---

Principes RGPD & Risques pour le projet

Principes RGPD clés
- Finalité explicite du traitement
- Minimisation des données
- Consentement utilisateur
- Durée de conservation limitée
- Sécurité & confidentialité
- Droits des personnes (accès, suppression)


Risques associés au projet IA
- Traitement d’images contenant des personnes
- Ré-identification indirecte via les photos
- Réutilisation non maîtrisée des images
- Entraînement de modèles sur données personnelles
- Exposition via des APIs externes
- Conservation excessive des données utilisateurs
- Données personnelles indirectes : nom, prénom, email, identifiant de compte


Le projet traite des données sensibles, notamment des images pouvant contenir des personnes.

On a donc analysé les principes du RGPD, comme la minimisation des données, la limitation de la durée de conservation et le consentement utilisateur, etc

Les principaux risques identifiés concernent la ré-identification indirecte via les photos, la réutilisation non maîtrisée des images, l’exposition des données en cas d’utilisation d’APIs externes.

Il y a aussi les données personnelles indirectes liées aux comptes utilisateurs, comme le nom ou l’email.

L’objectif n’est pas d’éliminer tout risque, mais de les identifier clairement afin de mettre en place des mesures adaptées.

-----------------------------------------

	Utilisez-vous les images pour entraîner un modèle ?

Non. Les images utilisateurs ne sont pas utilisées pour entraîner un modèle tiers.
Elles sont utilisées uniquement pour générer des embeddings nécessaires à la recommandation.


	Combien de temps conservez-vous les images ?

Une durée limitée et définie contractuellement, avec suppression automatique après traitement ou inactivité.


	Pourquoi ne pas utiliser un LLM externe ?

Pour garantir la maîtrise des données et éviter toute réutilisation non souhaitée.

-------

Solutions de prévetion & mesure RGPD

1) Gouvernance & usage des données

Consentement explicite à l’inscription
Finalité clairement définie (recommandation uniquement)
Aucune réutilisation des images à des fins d’entraînement de LLMs
Politique de rétention limitée

2)  Mesures techniques (System Design)

Stockage séparé images utilisateurs / catalogue
Suppression automatique après inactivité
Chiffrement des données au repos et en transit
Logs anonymisés

3) Anonymisation & minimisation

Pas de stockage des visages (si possible)
Pré-traitement image : extraction d’embeddings uniquement
Suppression de l’image brute après traitement
Conservation des vecteurs non ré-identifiants

4) APIs & fournisseurs externes

Pas d’envoi d’images utilisateurs à des APIs publiques
Modèles hébergés sur Azure (environnement maîtrisé)
Données non utilisées pour entraîner des modèles tiers




Pour prévenir les risques identifiés, on a intégré des mesures au niveau de l’organisation et techniques directement dans l’architecture.

D’un point de vue gouvernance, le consentement est explicite et les images ne sont utilisées que pour la recommandation, jamais pour entraîner des modèles tiers.

D’un point de vue technique, les images sont stockées séparément, supprimées automatiquement après traitement, et les données sont chiffrées.

On privilégie un hébergement Azure maîtrisé, sans envoi d’images à des APIs publiques.

Enfin, seules des représentations vectorielles non ré-identifiantes peuvent être conservées.

La RGPD n’est pas vue comme une contrainte, mais comme un levier de confiance utilisateur.

L’objectif est de garantir que les données personnelles restent sous le contrôle exclusif de l’entreprise.

--------------------------------------------------

	Comment prouvez-vous que les embeddings ne permettent pas la ré-identification ?

Les embeddings sont des représentations mathématiques compressées, non exploitables directement pour reconstruire une image.
Toutefois, une analyse d’impact (DPIA) pourrait être réalisée avant mise en production complète.


	Avez-vous prévu une DPIA ?

Oui, une analyse d’impact serait prévue en phase MVP avant déploiement à grande échelle.






















