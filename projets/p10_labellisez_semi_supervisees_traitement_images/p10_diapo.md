BrainScanAI Détection tumeur vs normal (IRM)

Approche semi‑supervisée à partir de peu de labels

L’objectif est de classifier des IRM en tumeur (cancer) vs normal avec très peu d’images labellisées. 

On va exploiter un gros volume non labellisé, mais de façon contrôlée, sans fuite de données. 

Ici on ne cherche pas à faire un diagnostic complet, juste une décision binaire pour aider au tri.

------

Labellisé : image avec une vérité terrain connue (ici “normal” ou “tumeur”).
Non labellisé : image sans étiquette ; elle peut servir à “apprendre la structure” des données sans connaître la classe.
Tumeur vs normal : les 2 classes à prédire : tumeur (cancer) ou pas de tumeur (normal).
IRM : “Imagerie par Résonance Magnétique”, un type d’examen médical qui produit des images du cerveau. Nous, on utilise directement l’image pour apprendre un modèle qui reconnaît des motifs visuels associés à une tumeur.”
Supervisé : on apprend avec des images dont on connaît la réponse (label).
Semi‑supervisé : on combine un petit lot d’images labellisées avec beaucoup d’images non labellisées pour améliorer l’apprentissage.
Peu de labels : en médical, annoter coûte cher (temps d’expert), donc on veut tirer parti des images sans label.
Détection : décider si une image contient quelque chose d’anormal ou non (ici, une tumeur).



---------

Contexte et probléme


Objectif : détecter tumeur vs normal sur IRM

Contraintes : labels rares et coûteux

Risques : biais, outliers/doublons, fuite de données

Stratégie : non labelisé de façon contrôlée

On part d’un cas classique en imagerie médicale : classification binaire tumeur/normal. 

La contrainte principale est la rareté du label, parce que annoter en médical coûte cher et prend du temps. 

L’idée du projet est donc d’utiliser les images non labellisées pour améliorer le modèle.

Le problème est simple, mais la difficulté, c’est la rareté des labels et le risque de fuite : donc j’ai mis l’accent sur la qualité des données et une évaluation propre.

-----------------------

J'ai mis l'accent sur la qualité des données et une évaluation propre.
Comment j'ai procéder ? Qu'est-ce que j'ai fait ? 
Avant de modéliser, j’ai fait un contrôle qualité pour éviter d’apprendre sur des données ‘sales’. J’ai calculé des statistiques simples sur les images, j’ai repéré des images atypiques, et surtout des quasi‑doublons via la similarité dans l’espace des embeddings. Ensuite j’ai gardé une évaluation propre : split train/validation/test sur le labellisé, et le test reste réservé jusqu’à la fin.

Qu'est-ce que j'aurai pu faire encore en + ?
Idéalement, j’aurais ajouté une validation externe (autre source/hôpital), une calibration systématique des probabilités, et si possible une annotation/segmentation de la zone tumorale pour aller au‑delà du binaire. Avec plus de temps : active learning pour augmenter les labels là où le modèle hésite.

-----

Données

Total : 1506 images

Labeled : 100 (50 normal / 50 cancer)

Unlabeled : 1406

Split : train/val/test sur labelisé (test strict)

Risque clé : data leakage (évité)

On dispose d’un petit socle labellisé équilibré (100) et d’un gros volume non labellisé. 

Je génère les pseudo‑labels uniquement sur les données d’entraînement. Le test reste totalement séparé et sert à mesurer les performances à la fin.

C’est important pour éviter de “tricher” indirectement via les données non supervisées. Le but est d’avoir une comparaison équitable entre le supervisé et le semi‑supervisé.

Supervisé : on apprend avec des images dont on connaît la réponse (label).
Semi‑supervisé : on combine un petit lot d’images labellisées avec beaucoup d’images non labellisées pour améliorer l’apprentissage.

------------------------------------

Pseudo-label (pseudo-étiquette) : étiquette “devinée” automatiquement (par clustering ou modèle), utilisée ensuite comme si c’était un label — avec un risque de bruit.
Split train / validation / test :
train : sert à apprendre les paramètres du modèle,
validation : sert à régler les choix (hyperparamètres, seuils, etc.),
test : sert à estimer la performance finale (on n’y touche pas avant la fin).
Fuite de données (data leakage) : utiliser (même indirectement) des infos du test pendant l’entraînement ou la conception → ça “gonfle” artificiellement les résultats.
Clustering : regrouper automatiquement des images “qui se ressemblent” sans labels (ex : KMeans), pour créer des groupes exploitables.

--------------------------------------

Pour éviter toute fuite de données, le test n’est jamais utilisé pour prendre une décision : ni pour choisir les paramètres, ni pour calibrer le clustering, ni pour apprendre le mapping cluster→classe. Le test sert uniquement à l’évaluation finale, exactement pareil pour la baseline et le semi‑supervisé.

------------


Pipeline globale (vue d'ensemble)

Exploration & contrôle qualité (QC)

Prétraitement + extraction de caractéristiques (embeddings)
Clustering + pseudo-labels (“weak labels”)

CNN : comparaison supervisé vs semi-supervisé (+ robustesse)


Le projet est structuré en quatre étapes. D’abord on comprend les données et on vérifie la qualité notamment pour repérer des soucis (doublons, images aberrantes, anormales/outliers).

Ensuite on transforme chaque image en vecteur de features via un CNN pré‑entraîné. 

Puis on applique du clustering pour explorer la structure et produire des pseudo‑labels. 
Clustering : regrouper automatiquement des images similaires sans utiliser de labels.

Enfin on entraîne et compare des modèles CNN avec une validation rigoureuse

=> séparation claire train/val/test et interdiction d’utiliser le test pour guider les choix (anti fuite de données).

-----

QC (Quality Control / contrôle qualité) : vérifs pour repérer des soucis (doublons, images aberrantes/outliers, distributions anormales).
Prétraitement : opérations avant le modèle (redimensionnement, normalisation, éventuellement égalisation d’histogramme/CLAHE).
CNN (Convolutional Neural Network) : réseau de neurones adapté aux images, capable d’extraire des motifs visuels.
Pré-entraîné : modèle déjà entraîné sur un gros dataset (ex. ImageNet) qu’on réutilise comme “extracteur”.
Features / caractéristiques : informations numériques qui décrivent l’image.
Embedding : vecteur de nombres (résumé compact) représentant l’image dans un espace numérique.
Weak labels / pseudo-labels : labels générés automatiquement (donc plus bruités qu’un vrai label).
Robustesse : stabilité des résultats quand on change légèrement les conditions (seed, split, etc.).
Validation rigoureuse : séparation claire train/val/test et interdiction d’utiliser le test pour guider les choix (anti fuite de données).

--------

Embedding / CNN :
Je transforme chaque image en un vecteur en passant par un CNN pré‑entraîné (type ResNet). Je récupère la représentation juste avant la couche de classification : ça donne une empreinte numérique plus pertinente que les pixels pour mesurer des similarités et faire du clustering.

Validation rigoureuse :La comparaison est équitable : mêmes splits, même test strict, mêmes métriques. Et je ne touche pas au test pour régler des choix ; tout se décide sur train/validation.


----

Préprocessing (entrée modèle)

Conversion en 3 canaux (RGB) pour compatibilité modèles pré‑entraînés

Redimensionnement 224×224

Normalisation ImageNet (mêmes statistiques que l’entraînement du backbone)

Pipeline stable, reproductible et traçable

Les IRM sont actuellement en niveaux de gris, on les convertit en RGB pour être compatibles avec les modèles pré-entrainé (backbones ImageNet). 

Le resize et la normalisation standard permettent d’exploiter un modèle pré‑entraîné sans ré‑apprendre une représentation from scratch. 

L’objectif ici n’est pas “d’optimiser au maximum” mais de fournir une base solide et reproductible. Tous les traitements sont traçables via les fichiers d’index et les artefacts.

--------------

Préprocessing / prétraitement : étapes appliquées à l’image avant de l’envoyer au modèle.
RGB (3 canaux) : format image à 3 canaux ; ici, on copie la même info de gris sur 3 canaux.
Resize / redimensionnement : mettre toutes les images à la même taille (224×224) pour le réseau.
Normalisation ImageNet : mise à l’échelle des pixels avec les moyennes/écarts‑types utilisés pour entraîner le modèle ImageNet (ça aligne les distributions).
Backbone : partie “extracteur de caractéristiques” d’un CNN.
Pré‑entraîné (pretrained) : déjà entraîné sur un grand dataset, qu’on réutilise pour gagner en performance et en temps.
Reproductible : si on relance, on obtient la même pipeline et des résultats cohérents.
Traçable : on peut relier chaque résultat aux paramètres et fichiers (index, artifacts).

---

Extraction de features (embeddings)

Modèle : ResNet50 pré‑entraîné (extracteur figé)

Sortie : 1 vecteur de 2048 valeurs par image

Donc : matrice de features 1506×2048 (1 ligne = 1 image)

Au lieu de comparer les pixels, on transforme chaque image en embedding (une ‘empreinte’ numérique plus sémantique). Ces embeddings servent ensuite à mesurer la similarité, ils servent de base au clustering et à certaines étapes de contrôle qualité. 
(faire du PCA/t‑SNE et du clustering)

À ce stade, je ne travaille plus directement sur les pixels. J’utilise un réseau pré‑entraîné, ResNet50
(je retire la dernière couche de classification et je récupère le vecteur juste avant.)

Résultat : pour chaque image, j’obtiens un vecteur de 2048 nombres, comme une empreinte qui résume son contenu visuel. En empilant toutes les images, j’ai une matrice 1506×2048. 
C’est beaucoup plus pratique et pertinent pour calculer des similarités, réduire la dimension (PCA/t‑SNE) et faire du clustering.

------

Embedding : “empreinte numérique” d’une image (un résumé en nombres).
Extracteur figé : on n’entraîne pas ResNet50 ici, on s’en sert juste pour produire des vecteurs.
Avant la tête de classification : on prend la représentation interne, pas la prédiction finale ImageNet.
Similarité : deux images proches → deux vecteurs proches.

---------

PCA :“La PCA compresse les vecteurs en gardant l’essentiel : ça réduit le bruit et rend les étapes suivantes plus stables/rapides.”
t‑SNE :“t‑SNE sert surtout à visualiser en 2D : ça aide l’intuition, mais ce n’est pas une preuve quantitative.”
Ton “comment” :“J’applique d’abord une PCA sur les embeddings, puis j’utilise PCA/t‑SNE pour visualiser si des groupes apparaissent. Ensuite je passe aux métriques et au clustering pour décider.”


---

Qualité données : nettoyage & biais

Statistiques par image : taille + intensité (moyenne, dispersion, percentiles)

Détection d’images atypiques (outliers) : score robuste + distance dans l’espace des embeddings

Détection de doublons : similarité cosinus (quasi‑doublons)

Règle de sécurité : ne jamais supprimer automatiquement les images labellisées


Avant de modéliser, je fais un contrôle qualité pour limiter les biais et artefacts donc c’est à dire éviter que le modèle apprenne des ‘mauvaises’ régularités : images trop sombres, aberrantes, ou doublons. Si on laisse ça, le modèle peut sur‑apprendre des artefacts ou être trompé par des répétitions, et les résultats deviennent moins fiables.

Pour les outliers, j’utilise des méthodes robustes qui évitent d’éjecter des images à tort, parce qu’elles sont ‘différentes’ mais pourtant valides. 
Pour les doublons, je regarde la similarité dans l’espace des embeddings : c’est plus fiable que comparer des pixels, surtout s’il y a de petites variations. 

Et point important : je protège le jeu labellisé ce qu’on appelle le « gold set », donc aucune suppression automatique côté labeled.

-----------

Outlier (valeur atypique) : image qui “sort du lot” (intensité, distribution, ou représentation très différente).
Z‑score robuste : mesure “à quel point c’est atypique” en s’appuyant sur des stats résistantes aux extrêmes.
Médiane : valeur centrale (plus robuste que la moyenne).
MAD (Median Absolute Deviation) : mesure de dispersion robuste (alternative robuste à l’écart‑type).
Embedding : empreinte numérique d’une image ; distance faible = images similaires.
Similarité cosinus : compare l’orientation de deux vecteurs (pratique pour dire “ils se ressemblent” même si l’échelle change).
Quasi‑doublon : même contenu mais petite variation (contraste, zoom, léger crop).


-------

Limiter les biais et artefacts
Ça veut dire éviter que le modèle apprenne des ‘mauvaises’ régularités : images trop sombres, aberrantes, ou doublons. Si on laisse ça, le modèle peut sur‑apprendre des artefacts ou être trompé par des répétitions, et les résultats deviennent moins fiables.


---

Analyse non supervisée (objectif)

Objectif : vérifier si l’espace des embeddings porte une structure utile (normal vs tumeur)

PCA : réduire le bruit et simplifier (plus stable / plus rapide)

Visualisation : PCA + t‑SNE (lecture qualitative, pas une preuve)

On cherche d’abord à comprendre si les images se regroupent naturellement dans l’espace des embeddings. 

Ici l’idée est simple : avant de faire du semi‑supervisé, je veux voir si les embeddings contiennent déjà une structure exploitable. 

Je commence par une PCA pour réduire le bruit et rendre les méthodes plus stables. Ensuite je vais pouvoir (j’utilise PCA et t‑SNE) visualiser si des groupes apparaissent. Mais avec prudence, c’est surtout pour l’intuition, donc je complète avec des métriques.

-------------

Non supervisé : on n’utilise pas les labels pour apprendre la structure.
Réduction de dimension : passer de 2048 dimensions à moins (ex. 50, 2) en gardant l’essentiel.
PCA : méthode linéaire qui résume les directions principales de variation (souvent plus stable).
t‑SNE : projection pour visualisation 2D/3D qui préserve surtout les voisinages locaux (à interpréter avec prudence).
Structure utile : des images similaires se retrouvent proches, et potentiellement les classes se séparent un peu.


-------

Clustering : méthodes testées & choix

Méthodes testées : KMeans (principal) + DBSCAN (comparaison)

Choix : KMeans → simple, stable, interprétable, donne des groupes exploitables

Sortie : clusters → pseudo‑labels pour entraîner le modèle semi‑supervisé

Maintenant que j’ai une représentation en embeddings, je peux regrouper les images automatiquement. 

J’ai testé KMeans et, en comparaison, une méthode par densité type DBSCAN pour voir si le résultat est sensible au choix d’algorithme. 

Au final je garde KMeans : c’est simple, stable et ça produit des groupes exploitables. 

Et ces groupes servent ensuite à créer des pseudo‑labels qui alimentent l’entraînement semi‑supervisé.

(TRANSITION) 
Une fois les clusters obtenus, je dois les convertir en pseudo‑labels ‘tumeur/normal’ sans fuite de données, puis entraîner le CNN et comparer au supervisé.

--------

Clustering : regrouper des images similaires sans labels.
KMeans : méthode qui découpe en 𝑘 groupes en minimisant la distance aux centres.
DBSCAN : regroupe selon la densité (trouve des “nuages” + peut laisser des points en bruit).
Robustesse : résultat qui ne change pas trop quand on change un peu les paramètres / l’algo.
Pseudo‑labels / weak labels : labels automatiques, utiles mais potentiellement bruités.

-------

DBSCAN :“DBSCAN est un clustering par densité : il regroupe des ‘nuages’ de points et peut laisser des points isolés en ‘bruit’. Je l’utilise surtout comme comparaison pour voir si le résultat dépend trop de l’algorithme.”

Intuition :L’intuition, c’est ce que me suggèrent les visualisations : est‑ce que ça ressemble à des groupes ?

Métriques :“Les métriques, c’est ce qui tranche objectivement : un score qui mesure si les clusters correspondent un minimum aux classes sur les données labellisées.”


----

Évaluer le clustering sans fuite

Évaluation sur les images labellisées d’entraînement/validation (jamais sur le test)

Métrique : ARI = accord entre clusters et classes 

Si ARI trop faible → pseudo‑labels trop bruités → Rester prudent

Pour éviter toute fuite de données, j’évalue le clustering uniquement sur la partie labellisée… mais que côté train/val (entrainement / validation) et jamais sur le test. 

Sur les images non labellisées, je ne peux pas mesurer directement la qualité du clustering parce que je n’ai pas de labels. Du coup, j’évalue le clustering sur les données labellisées du train avec l’ARI, et je garde le test totalement à part pour éviter toute fuite. L’ARI est pratique car elle ne dépend pas du nom des clusters  (si on échange les clusters 0 et 1, le score ne change pas)

Si l’ARI est faible, ça me dit que les pseudo‑labels risquent d’être bruités, donc je reste prudent avant de les injecter dans l’entraînement.

-----

Vérité terrain (ground truth) : le vrai label connu.
ARI (Adjusted Rand Index) : score d’accord entre deux partitions (clusters vs classes), corrigé du hasard ; utile quand les labels de clusters sont arbitraires (cluster 0/1).
Fuite de données : utiliser le test pour prendre une décision (même indirectement) → biais.
Pseudo‑labels : labels auto générés ; utiles mais bruités.

-----

Vérité terrain (ground truth) :La vérité terrain, c’est le vrai label connu (ici normal/tumeur) sur le sous‑ensemble labellisé.

Classes vs clusters (explication ultra claire) :Une classe, c’est ‘normal’ ou ‘tumeur’ (défini par l’humain). Un cluster, c’est juste un groupe trouvé automatiquement (cluster 0/1) sans signification médicale au départ. Donc je dois apprendre un mapping cluster→classe à partir du train labellisé, puis l’appliquer au non‑labellisé.


---


Pseudo‑labels & filtrage de confiance

Associer chaque cluster à une classe à partir du train labellisé (sans toucher au test)


Générer des pseudo‑labels pour les images non labellisées


Réduire le bruit : ne garder que les pseudo‑labels les plus “confiants” (filtrage)

Un cluster n’a pas de nom médical : il faut le traduire en ‘normal’ ou ‘tumeur’. Pour ça, j’utilise uniquement les images labellisées du train : je regarde quel cluster correspond le mieux à quelle classe, et je fixe ce mapping pour que ça correspond plutôt à ‘tumeur’ ou correspond plutôt à ‘normal’. 

Ensuite j’applique ce mapping aux images non labellisées pour produire des pseudo‑labels. Comme ces pseudo‑labels peuvent être faux, je garde seulement les cas les plus confiants : ça réduit le bruit avant d’entraîner le CNN.

Je n’utilise jamais le test pour décider le mapping ou le filtre, sinon je biaise l’évaluation.


(…) La confiance, c’est à quel point une image est clairement plus proche d’un cluster que de l’autre ; si elle est entre les deux, je la retire.

Si le filtrage devient trop strict et déséquilibre tout, je relâche le seuil pour garder un minimum de diversité. 

-------

Le clustering fait des groupes (cluster A, cluster B), mais il ne sait pas dire “tumeur” ou “normal”.
Sur les images déjà labellisées (train uniquement), on regarde : “dans quel cluster tombent plutôt les tumeurs ? dans quel cluster tombent plutôt les normales ?”
Ça nous donne une règle : cluster A → tumeur, cluster B → normal.
Important : on apprend cette règle uniquement sur le train labellisé (sinon fuite de données).
On applique cette règle aux images non labellisées : elles reçoivent un pseudo‑label (tumeur/normal).
Comme ces pseudo‑labels peuvent être faux, on ajoute un filtre de confiance :
si une image est “clairement” dans un cluster → on la garde ;
si elle est “entre les deux” / ambiguë → on la retire (ou on ne l’utilise pas).
5) Filet de sécurité : si le filtre est trop strict et qu’il ne reste presque plus d’images (ou presque qu’une seule classe), on relâche le filtre pour garder un jeu exploitable.

-------

Mapping cluster→classe : règle qui dit “cluster A = tumeur, cluster B = normal”.
Confiance (pseudo‑label) : indicateur “à quel point l’affectation au cluster est claire”.
Filtrage : retirer les exemples les plus incertains pour réduire les erreurs.
Bruit de label : pseudo‑labels parfois faux (risque principal du semi‑supervisé).

---


Modèles entraînés : baseline vs semi‑supervisé

Baseline supervisée : CNN (ex. ResNet18) sur labeled uniquement

Semi‑supervisé : pré‑entraînement sur pseudo‑labels → fine‑tuning “strong”

Comparaison équitable : même test, mêmes métriques

À ce stade, j’entraîne deux modèles pour comparer proprement. La baseline, c’est ce qu’on obtient en supervisé pur : un CNN entraîné uniquement sur les 100 images labellisées. 

Le semi‑supervisé essaie d’exploiter les 1406 images non labellisées : je leur attribue des pseudo‑labels, j’en fais une phase de pré‑entraînement, puis je termine par un fine‑tuning sur les vraies labels. Et pour que la comparaison soit honnête, j’évalue les deux modèles sur le même test, avec les mêmes métriques.

Le but n’est pas forcément de battre la baseline à tout prix, mais de voir si l’ajout de pseudo‑labels apporte un gain ou au moins de la stabilité, sans tricher sur le test.

------

Baseline : référence “simple” qui sert de point de comparaison.
Supervisé : on apprend avec des exemples dont on connaît la classe.
Semi‑supervisé : on combine peu de labels fiables + beaucoup de données non labellisées (avec pseudo‑labels).
Pré‑entraînement : première phase pour “amorcer” le modèle avec plus de données.
Fine‑tuning : ajustement final sur les données labellisées (plus fiables).
Pseudo‑label : label généré automatiquement, donc potentiellement bruité.
Weak/Strong (si tu le gardes) : augmentations “légères” vs “fortes” sur les images ; l’idée est d’améliorer la robustesse sans trop dégrader le signal.
Généralisation : capacité à bien marcher sur des images jamais vues (test).

---

Résultats & robustesse


Métriques : F1 macro + matrice de confusion (accuracy en complément)

Multi‑seeds : moyenne / écart‑type

Message clé : score similaire, mais variance plus faible en semi‑supervisé

Comme on a peu de données, une seule exécution peut être trompeuse. Donc je répète l’entraînement sur plusieurs seeds et je regarde la moyenne et la variabilité: l’objectif, c’est un modèle fiable, pas un résultat ‘chanceux’

Côté métriques, je regarde l’accuracy, mais surtout le F1 macro et la matrice de confusion, parce que ça dit mieux où le modèle se trompe.

Côté semi‑supervisé, j’ai d’abord pseudo‑labellisé 100% des images non annotées, soit 1406. Ensuite je filtre pour limiter le bruit, et je n’en garde qu’environ un quart — autour de 350 images — pour l’entraînement weak.”  : le semi‑supervisé est comparable en score, et souvent plus stable

Sur le test final, le modèle supervisé atteint 95% d’accuracy et un F1 macro d’environ 0.95. En semi‑supervisé, je suis à 90% d’accuracy et ~0.90 de F1 macro. 
Et surtout, quand je répète sur plusieurs seeds, le semi‑supervisé filtré est plus stable : même moyenne mais la variance est nettement plus faible, donc je vise un modèle fiable plutôt qu’un coup de chance, c’est plus robuste

----

Accuracy : proportion de bonnes prédictions.
F1 macro : moyenne des F1 des deux classes (chaque classe compte autant) → utile quand on ne veut pas qu’une classe “écrase” l’autre.
Matrice de confusion : tableau qui montre où le modèle se trompe (faux positifs / faux négatifs).
Seed : graine aléatoire qui change l’initialisation et certains tirages → permet de mesurer la variabilité.
Variance / écart‑type : mesure à quel point les résultats bougent d’un run à l’autre.

----

Conclusion

Pipeline bout‑en‑bout : embeddings → clustering → semi‑supervisé

Résultats : performances comparables au supervisé + meilleure stabilité (multi‑seeds)

Valeur projet : reproductible, traçable, garde‑fous anti‑fuite + QC données

Suite : normalisation spécifique IRM + annotation progressive (active learning) + calibration/validation

Pour conclure, j’ai construit un pipeline complet qui exploite un grand volume d’images non labellisées sans fuite de données : extraction d’embeddings, clustering, puis entraînement semi‑supervisé. 

En résultat, le semi‑supervisé atteint des performances comparables au supervisé, avec un gain surtout sur la stabilité quand on répète sur plusieurs seeds. 

Les points forts du projet, c’est la reproductibilité, la traçabilité des artefacts, et le contrôle qualité qui limite les risques liés aux données comme les doublons et outliers. 

Et les prochaines étapes logiques seraient d’adapter la normalisation au contexte IRM, puis d’augmenter progressivement le jeu labellisé via de l’active learning, avec calibration et validation plus poussées.

----------

Traçabilité : on peut relier résultats ↔ paramètres ↔ fichiers (artefacts).
Calibration : rendre les probabilités du modèle fiables (ex : 0.8 ≈ 80% de chances).
Active learning : le modèle suggère les exemples les plus utiles à annoter pour améliorer vite avec peu de labels.


---


Recommandations & déploiement (4M images, 5000€) + Q&A (backup)



Traçabilité : index, versions de modèles, métriques (artefacts)

Pipeline batch relançable (coût maîtrisé)

Suivi simple : dérive + contrôle qualité (QC)

Q&A (backup) : anti‑fuite, pourquoi embeddings, pourquoi filtrage

Si je dois projeter ça en déploiement avec 4M d’images et un budget limité, je recommande un pipeline batch relançable : on traite par lots, on garde un index, on versionne les modèles et on trace les métriques. 

L’objectif, c’est de pouvoir lreproduire un run et expliquer un résultat. Et en production, je mets un monitoring simple : détection de dérive + contrôle qualité, pour repérer tôt si les images changent ou si a qualité se dégrade. 

---------

Artefacts : fichiers produits par le pipeline (index, embeddings, modèles, métriques) qui prouvent et reproduisent le résultat.
Traçabilité : capacité à relier un résultat à des données, paramètres et versions.
Pipeline batch relançable : traitement par lots qu’on peut relancer à l’identique si besoin (robuste et économique).
Dérive (drift) : les données changent dans le temps (machines différentes, protocoles, qualité), ce qui peut faire baisser les performances.
Monitoring : surveillance continue d’indicateurs (qualité image, distributions, perf si labels dispo).


