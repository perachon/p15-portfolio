
///////// VOCABULAIRE	

	- Détection: décider si une image contient quelque chose d’anormal ou non (ici, une tumeur).
	- Tumeur vs normal: les 2 classes à prédire :tumeur (cancer)ou pas de tumeur (normal).
	- IRM: “Imagerie par Résonance Magnétique”, un type d’examen médical qui produit des images du cerveau. Nous, on utilise directement l’image pour apprendre un modèle qui reconnaît des motifs visuels associés à une tumeur.”
	- Supervisé: on apprend avec des images dont on connaît la réponse (label).
	- Semi‑supervisé: on combineun petit lot d’images labellisées avec beaucoup d’images non labelliséespour améliorer l’apprentissage.
	- Peu de labels: en médical, annoter coûte cher (temps d’expert), donc on veut tirer parti des images sans label.


	- Labellisé: image avec une vérité terrain connue (ici “normal” ou “tumeur”).
	- Non labellisé: image sans étiquette ; elle peut servir à “apprendre la structure” des données sans connaître la classe.
	- Split train / validation / test train: sert à apprendre les paramètres du modèle,
	- validation: sert à régler les choix (hyperparamètres, seuils, etc.),
	- test: sert à estimer la performance finale (on n’y touche pas avant la fin).
	- Fuite de données (data leakage): utiliser (même indirectement) des infos du test pendant l’entraînement ou la conception → ça “gonfle” artificiellement les résultats.
	- Pseudo-label (pseudo-étiquette): étiquette “devinée” automatiquement (par clustering ou modèle), utilisée ensuite comme si c’était un label — avec un risque de bruit.
	- Clustering: regrouper automatiquement des images “qui se ressemblent” sans labels (ex : KMeans), pour créer des groupes exploitables.


	- QC (Quality Control / contrôle qualité) : vérifs pour repérer des soucis (doublons, images aberrantes/outliers, distributions anormales).
	- Prétraitement : opérations avant le modèle (redimensionnement, normalisation, éventuellement égalisation d’histogramme/CLAHE).
	- CNN (Convolutional Neural Network) : réseau de neurones adapté aux images, capable d’extraire des motifs visuels.
	- Pré-entraîné : modèle déjà entraîné sur un gros dataset (ex. ImageNet) qu’on réutilise comme “extracteur”.
	- Features / caractéristiques : informations numériques qui décrivent l’image.
	- Embedding : vecteur de nombres (résumé compact) représentant l’image dans un espace numérique.
	- Clustering : regrouper automatiquement des images similaires sans utiliser de labels.
	- Weak labels / pseudo-labels : labels générés automatiquement (donc plus bruités qu’un vrai label).
	- Robustesse : stabilité des résultats quand on change légèrement les conditions (seed, split, etc.).
	- Validation rigoureuse : séparation claire train/val/test et interdiction d’utiliser le test pour guider les choix (anti fuite de données).


	- Préprocessing / prétraitement : étapes appliquées à l’image avant de l’envoyer au modèle.
	- RGB (3 canaux) : format image à 3 canaux ; ici, on copie la même info de gris sur 3 canaux.
	- Resize / redimensionnement : mettre toutes les images à la même taille (224×224) pour le réseau.
	- Normalisation ImageNet : mise à l’échelle des pixels avec les moyennes/écarts‑types utilisés pour entraîner le modèle ImageNet (ça aligne les distributions).
	- Backbone : partie “extracteur de caractéristiques” d’un CNN.
	- Pré‑entraîné (pretrained) : déjà entraîné sur un grand dataset, qu’on réutilise pour gagner en performance et en temps.
	- Reproductible : si on relance, on obtient la même pipeline et des résultats cohérents.
	- Traçable : on peut relier chaque résultat aux paramètres et fichiers (index, artifacts).


	- Embedding : “empreinte numérique” d’une image (un résumé en nombres).
	- Extracteur figé : on n’entraîne pas ResNet50 ici, on s’en sert juste pour produire des vecteurs.
	- Avant la tête de classification : on prend la représentation interne, pas la prédiction finale ImageNet.
	- Similarité : deux images proches → deux vecteurs proches.


	- Outlier (valeur atypique) : image qui “sort du lot” (intensité, distribution, ou représentation très différente).
	- Z‑score robuste : mesure “à quel point c’est atypique” en s’appuyant sur des stats résistantes aux extrêmes.
	- Médiane : valeur centrale (plus robuste que la moyenne).
	- MAD (Median Absolute Deviation) : mesure de dispersion robuste (alternative robuste à l’écart‑type).
	- Embedding : empreinte numérique d’une image ; distance faible = images similaires.
	- Similarité cosinus : compare l’orientation de deux vecteurs (pratique pour dire “ils se ressemblent” même si l’échelle change).
	- Quasi‑doublon : même contenu mais petite variation (contraste, zoom, léger crop).


	- Non supervisé : on n’utilise pas les labels pour apprendre la structure.
	- Réduction de dimension : passer de 2048 dimensions à moins (ex. 50, 2) en gardant l’essentiel.
	- PCA : méthode linéaire qui résume les directions principales de variation (souvent plus stable).
	- t‑SNE : projection pour visualisation 2D/3D qui préserve surtout les voisinages locaux (à interpréter avec prudence).
	- Structure utile : des images similaires se retrouvent proches, et potentiellement les classes se séparent un peu.


	- Clustering : regrouper des images similaires sans labels.
	- KMeans : méthode qui découpe en k groupes en minimisant la distance aux centres.
	- DBSCAN : regroupe selon la densité (trouve des “nuages” + peut laisser des points en bruit).
	- Robustesse : résultat qui ne change pas trop quand on change un peu les paramètres / l’algo.
	- Pseudo‑labels / weak labels : labels automatiques, utiles mais potentiellement bruités.


	- Vérité terrain (ground truth): le vrai label connu.
	- ARI (Adjusted Rand Index): score d’accord entre deux partitions (clusters vs classes), corrigé du hasard ; utile quand les labels de clusters sont arbitraires (cluster 0/1).
	- Fuite de données: utiliser le test pour prendre une décision (même indirectement) → biais.
	- Pseudo‑labels: labels auto générés ; utiles mais bruités.


	- Mapping cluster→classe : règle qui dit “cluster A = tumeur, cluster B = normal”.
	- Confiance (pseudo‑label) : indicateur “à quel point l’affectation au cluster est claire”.
	- Filtrage : retirer les exemples les plus incertains pour réduire les erreurs.
	- Bruit de label : pseudo‑labels parfois faux (risque principal du semi‑supervisé).


	- Baseline : référence “simple” qui sert de point de comparaison.
	- Supervisé : on apprend avec des exemples dont on connaît la classe.
	- Semi‑supervisé : on combine peu de labels fiables + beaucoup de données non labellisées (avec pseudo‑labels).
	- Pré‑entraînement : première phase pour “amorcer” le modèle avec plus de données.
	- Fine‑tuning : ajustement final sur les données labellisées (plus fiables).
	- Pseudo‑label : label généré automatiquement, donc potentiellement bruité.
	- Weak/Strong (si tu le gardes) : augmentations “légères” vs “fortes” sur les images ; l’idée est d’améliorer la robustesse sans trop dégrader le signal.
	- Généralisation : capacité à bien marcher sur des images jamais vues (test).


	- Accuracy : proportion de bonnes prédictions.
	- F1 macro : moyenne des F1 des deux classes (chaque classe compte autant) → utile quand on ne veut pas qu’une classe “écrase” l’autre.
	- Matrice de confusion : tableau qui montre où le modèle se trompe (faux positifs / faux négatifs).
	- Seed : graine aléatoire qui change l’initialisation et certains tirages → permet de mesurer la variabilité.
	- Variance / écart‑type : mesure à quel point les résultats bougent d’un run à l’autre.


	- Traçabilité : on peut relier résultats ↔ paramètres ↔ fichiers (artefacts).
	- Calibration : rendre les probabilités du modèle fiables (ex : 0.8 ≈ 80% de chances).
	- Active learning : le modèle suggère les exemples les plus utiles à annoter pour améliorer vite avec peu de labels.


	- Artefacts : fichiers produits par le pipeline (index, embeddings, modèles, métriques) qui prouvent et reproduisent le résultat.
	- Traçabilité : capacité à relier un résultat à des données, paramètres et versions.
	- Pipeline batch relançable : traitement par lots qu’on peut relancer à l’identique si besoin (robuste et économique).
	- Dérive (drift) : les données changent dans le temps (machines différentes, protocoles, qualité), ce qui peut faire baisser les performances.
	- Monitoring : surveillance continue d’indicateurs (qualité image, distributions, perf si labels dispo).


\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\

///////// QUESTIONS


	1) Slide présentation

Q: “Pourquoi semi‑supervisé ici ?”
R: “Parce que labelliser en médical coûte cher. Le semi‑supervisé permet de tirer quelque chose de l’unlabeled tout en gardant un test strict.”


Q: “Quel est le livrable principal ?”
R: “Un pipeline complet + des notebooks reproductibles + recommandations de déploiement.”



	2) Contexte / problème

Q: “C’est du diagnostic médical ?”
R: “Non, c’est une démo technique : classification d’images. En vrai contexte médical il faut validation clinique et données plus robustes.”


Q: “Le plus gros risque ?”
R: “La fuite de données : utiliser indirectement le test pour guider le clustering ou les pseudo‑labels.”



	3) Données

Q: “100 labels, c’est suffisant ?”
R: “C’est suffisant pour un baseline et pour mesurer un gain, mais ça reste petit : d’où l’intérêt du semi‑supervisé et de la robustesse multi‑seed.”


Q: “Pourquoi un test ‘strict’ ?”
R: “Pour que le score final reflète une vraie généralisation, pas une optimisation indirecte.”



	4) Pipeline globale (vue d'ensemble)

Q: “Pourquoi passer par des embeddings ?”
R: “Parce qu’un CNN pré‑entraîné donne une représentation plus robuste que les pixels, surtout avec peu de données.”


Q: “Pourquoi garder le pipeline en étapes ?”
R: “Pour être reproductible et réutiliser les artefacts (features, index, métriques).”


Q: “Pourquoi autant d’artefacts ?”
R: “Pour éviter de recalculer les embeddings et garder de la traçabilité : quelles images, quelles features, quelles métriques.”


Q: “Qu’est-ce qui est le plus utile pour le mentor ?”
R: “Les métriques, les fichiers QC, et la comparaison supervisé vs semi‑supervisé.”



	5) Preprocessing (entrée modèle)

Q: “Pourquoi ImageNet sur IRM, ce n’est pas le même domaine ?”
R: “Ce n’est pas parfait, mais comme extracteur de features ça marche souvent très bien. Et ici on vise une base solide et rapide.”


Q: “Tu as testé d’autres tailles ?”
R: “Je reste sur 224×224 car c’est standard ResNet, plus simple et stable.”



	6) Extraction des features

Q: “Pourquoi ResNet50 et pas un autre modèle ?”
R: “C’est un bon compromis robustesse/standard, facile à expliquer et à reproduire.”


Q: “Tu entraînes ResNet50 ?”
R: “Non, ici il sert d’extracteur fixe. L’entraînement est fait sur un modèle plus léger pour la classification.”



	7) Qualité données : nettoyage & biais

Q: “Tu supprimes vraiment des images ?”
R: “Je prépare un index filtré optionnel. L’idée est de pouvoir comparer avec/sans filtrage.”


Q: “Pourquoi cosinus pour doublons ?”
R: “Dans l’espace embedding, deux images quasi identiques seront très proches : c’est un bon signal de doublon.”



	8) Analyse non supervisée (objectif)

Q: “t‑SNE c’est fiable ?”
R: “C’est surtout visuel. Je m’en sers pour explorer, pas pour conclure seul.”

Q: “Pourquoi PCA avant KMeans ?”
R: “Ça stabilise et accélère, surtout en grande dimension.”



	9) Clustering : méthodes testées & choix

Q: “Pourquoi pas un clustering ‘plus avancé’ ?”
R: “Je privilégie un baseline solide et explicable. Si ça marche déjà, c’est un bon signal.”

Q: “KMeans suppose des clusters ‘ronds’, c’est ok ?”
R: “Oui, c’est une approximation. Je le valide ensuite via l’ARI sur labeled.”



	10) Évaluer le clustering sans fuite

Q: “ARI ‘bon’, c’est combien ?”
R: “Ça dépend. Ici je cherche surtout un signal > 0, pas la perfection. Les pseudo‑labels seront bruités de toute façon.”

Q: “Pourquoi ne pas optimiser sur le test ?”
R: “Parce que ça fausserait la comparaison finale.”

Q : “Pourquoi évaluer sur train/val et pas test ?”
R : “Parce que le test doit rester un juge final. Si je m’en sers pour choisir le clustering, je biaise la comparaison supervisé vs semi‑supervisé.”



	11) Pseudo-labels & filtrage

Q: “Si les pseudo‑labels sont faux, ça casse tout ?”
R: “Ça peut dégrader, oui. D’où le filtrage/pondération et le pré‑train court.”

Q: “Tu utilises unlabeled test ?”
R: “Non. Je garde un test strict labellisé, et l’unlabeled sert uniquement au weak training.”

Q : “Pourquoi apprendre le mapping seulement sur train labellisé ?”
R : “Parce que sinon j’utiliserais indirectement val/test pour décider, et je biaiserais l’évaluation du modèle.”



	12) Modèles : baseline vs semi‑supervisé

Q: “Pourquoi ResNet18 ici ?”
R: “Plus léger, rapide à entraîner, suffisant pour comparer des stratégies.”

Q: “Tu ne fais pas juste de l’augmentation de données ?”
R: “J’en fais aussi, mais l’intérêt ici est d’ajouter de l’information via le pool unlabeled.”



	13) Résultats & robustesse

Q: “Tu peux donner 1 chiffre simple ?”
R: “En moyenne sur 3 seeds, l’accuracy est équivalente, et l’écart‑type est plus faible en semi, donc moins de variance.”

Q: “Pourquoi la stabilité est importante ?”
R: “Parce qu’avec peu de data, un seul run peut être trompeur. La stabilité indique un comportement plus fiable.”

Q: “Ppourquoi pas juste accuracy ?” :
R: “Parce que l’accuracy seule ne dit pas quel type d’erreur on fait ; en médical, confondre une tumeur avec normal (FN) et l’inverse (FP) n’a pas le même impact. La matrice de confusion et le F1 donnent une vue plus fiable.”



	14) Conclusion

Q: “Si tu avais 1 semaine de plus, tu fais quoi ?”
R: “Normalisation IRM plus spécifique + tests d’augmentations + calibration seuil de confiance des pseudo‑labels.”

Q: “Le point fort principal ?”
R: “La comparaison propre et reproductible, sans fuite.”



	15) Recommandations & déploiement

Q: “Pourquoi batch plutôt que temps réel ?”
R: “Budget/complexité : batch coûte moins cher et suffit souvent pour des analyses à grande échelle.”

Q: “Tu monitors quoi concrètement ?”
R: “Distribution des embeddings, taux d’outliers, taux d’échec lecture, et drift des scores/qualité.”

Q: “Pourquoi anti‑fuite partout ?”
R: “Parce que sinon on se ment sur la performance : le test doit rester un juge final, sinon la comparaison supervisé vs semi‑supervisé est biaisée.”

Q: “Pourquoi passer par des embeddings plutôt que les pixels ?”
R: “Les embeddings résument l’image de façon plus ‘sémantique’ : c’est plus pertinent et beaucoup plus efficace pour mesurer des similarités et faire du clustering.”

Q: “Pourquoi filtrer par confiance ?”
R: “Parce que les pseudo‑labels sont bruités : filtrer enlève les cas ambiguës et évite d’entraîner le modèle sur de mauvaises étiquettes.”



	16) Autres

Q: “Quel est le risque majeur si on déploie ?”
R: “La dérive des données (protocoles IRM différents) et une performance qui change. D’où le monitoring.”

Q: “Tu as une phrase de synthèse ?”
R: “On exploite l’unlabeled sans tricher : pipeline propre, résultats comparables, stabilité meilleure.” 

