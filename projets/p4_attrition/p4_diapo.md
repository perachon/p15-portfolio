Attrition ESN
TechNova Partners


Présentation du projet
Objectif : Réduire le nombre de démissions dans l’entreprise

----

Comment ?
Modèle de classification supervisée 
Prédire la probabilité de départ 
Identifier les facteurs clés influençant l’attrition

----

Les étapes suivies
Sources de données (3 fichiers)
Système SI RH
Evaluations annuelles de performance
Sondage

Regrouper les données 
Même nombre de ligne
Recouvrement

(Tableau)
Fichier
Colonne ID
Contenu principal

extrait_eval
eval_number (ex: E_1)
Données de satisfaction + évaluation

extrait_sirh
id_employee (ex: 1)
Infos RH : âge, genre, revenu, poste…

extrait_sondage
code_sondage (ex: 1)
Participation, enfants, promotions, etc.

Pour cela on a 3 fichiers sources, on est allé chercher les données dans notre système RH

Demander à nos employés de remplir un sondage qui nous aide à mettre en place des actions pour le bien-être des employés. 

Même nombre de ligne pour faciliter le groupement et la jointure des données

Le recouvrement c’est quand plusieurs fichiers contiennent des informations sur les mêmes entités (ici : les employés), et qu’on peut les faire correspondre entre eux.

---

Les étapes suivies

Préparation des données
Nettoyage
Suppression des colonnes non pertinentes (Ids, constantes)
Encodage des variables catégorielles (OneHot)
Gestion des corrélations fortes

1470 employés
1233 restants 
237 démissionnés (16 %)

Séparation train/test :
Stratification = représentatif

Stratification pour conserver la même proportion des classes. On s’assure que la proportion de 0 et de 1 est la même dans train et test
C’est représentatif, donc le modèle apprend et est testé dans des conditions comparables.


Jeu final
  X (features) 
  y (cible = départ 0/1) 

----

Dummy Classifier (baseline) → accuracy ≈ 84 %, mais incapacité totale à prédire les départs (classe minoritaire).
Régression logistique → résultats corrects, recall ~0.68 sur la classe "départ", bon compromis mais pas optimal.
Random Forest brut → fort surapprentissage (overfitting).

C’est le sur-apprentissage en français, un des concepts les plus importants en Machine Learning.
Overfit = quand notre modèle apprend trop bien les données d’entraînement, au point de mémoriser les détails et il “récite par cœur” au lieu de “comprendre”.


Le fine-tuning = ajuster les hyperparamètres d’un modèle pour améliorer ses performances.
Pourquoi c’est utile ?
Les valeurs par défaut des hyperparamètres ne sont pas optimales pour un jeu de données.
Si on ne les ajuste pas → on risque soit :
sous-apprentissage (le modèle est trop simple),
sur-apprentissage (overfit) (trop complexe, il mémorise au lieu de généraliser).
Le fine-tuning permet de trouver le meilleur compromis (bon rappel sans overfit).

------------------------------------------------------------------------------------------------------------

MODELE LINEAIRE

Idée : C’est un modèle qui suppose une relation linéaire entre les variables explicatives (X) et la logit de la probabilité de la classe cible (y).
Donc : chaque variable pousse la proba vers “reste” ou “part” en fonction de son poids.
C’est un classique en classification binaire.
Facile à interpréter : on peut lire les coefficients pour savoir quelles features influencent le départ.
Rapide à entraîner, robuste, baseline solide.
Sert souvent de point de comparaison avant des modèles plus complexes.

🔹 Modèle linéaire
C’est un modèle qui suppose une relation droite, proportionnelle entre les variables d’entrée (features) et la sortie (prédiction).
Exemple : la régression logistique → chaque variable a un poids qui s’ajoute, et le modèle trace une frontière droite entre les classes.👉 Avantage : simple, interprétable, rapide.👉 Limite : pas adapté si la relation est complexe ou non linéaire.

🔹 Modèle non linéaire
Il peut apprendre des relations plus complexes, avec des interactions, des formes courbes, des seuils.
Exemple : les arbres de décision et les forêts aléatoires (Random Forest) → ils découpent l’espace des données en zones irrégulières.👉 Avantage : plus puissant, capture des patterns compliqués.👉 Limite : plus difficile à interpréter, risque d’overfitting.


----

Validation croisée

Éviter de juger notre modèle sur un seul découpage train/test 

Mesurer la performance moyenne et la stabilité

Détecter l’overfit

Comparer proprement plusieurs modèles

Régression logistique 
Moins de réussite sur l’accuracy et la precision
Mais gros avantage recall (70% vs 43%) et pas d’overfit
 C’est le bon choix métier : mieux vaut détecter plus de départs quitte à avoir quelques fausses alertes.

Random Forest régularisée
Haute précision mais rappel trop faible → elle ratera beaucoup trop de départs.
Fort overfit → instable pour une vraie utilisation.

 Modèle gagnant = Régression logistique.



------------------------------------------------------------------------------------------------------------------------------------------------

Accuracy : Proportion de prédictions correctes (global)  La note générale sur 20 à un examen
Precision : Quand le modèle prédit positif, quelle proportion est correcte ?  Parmi les employés que j’annonce partant, cb partent vraiment ?
Recall : Parmi les vrais positifs cb le modèle en retrouve ?  Parmi tous les employés qui partent, cb le modèle en a su détecter ?


----

Régression logistic

Seuil de décision

Si on baisse le seuil

→ plus de départs détectés 
(recall ↑) mais plus de fausses alertes (precision ↓).

Si on monte le seuil 

→ moins de fausses alertes (precision ↑) mais on rate plus de vrais départs (recall ↓).





Le seuil de décision est ajustable pour n’importe quel modèle de classification qui sort des probabilités.
C’est exactement le compromis faux positifs vs faux négatifs.
C’est très important selon les besoins métiers ex:
Médecine (détecter une maladie rare) → rappel max → seuil bas.
Anti-fraude bancaire (éviter fausses alertes coûteuses) → précision max → seuil haut.
RH (départs d’employés) → dépend du coût de rater un départ vs fausse alerte → plutôt recall élevé.


Tracer la courbe Precision / Recall en fonctn du seuil 

Ces courbes nous permettent de visualiser le compromis et de choisir un seuil adapté au contexte métier.
 Le bon choix métier : mieux vaut détecter plus de départs quitte à quelques fausses alertes.



--------------------------------------------------------------------------------------------


Accuracy : Proportion de prédictions correctes (global)  La note générale sur 20 à un examen
Precision : Quand le modèle prédit positif, quelle proportion est correcte ?  Parmi les employés que j’annonce partant, cb partent vraiment ?
Recall : Parmi les vrais positifs cb le modèle en retrouve ?  Parmi tous les employés qui partent, cb le modèle en a su détecter ?

Courbe jaune Recall = (→ éviter les faux négatifs).
👉 Le recall mesure la capacité à ne pas rater les vrais cas positifs importants (ici les départs).
Courbe bleu Precision =  (→ éviter les faux positifs).
👉 La précision mesure la capacité à ne pas se tromper quand on dit positif, c’est sûr.


----

Seuil très bas (0,2)	 Cas typique si on ne veut absolument ne pas rater de départs, mais ça sature les RH 		de fausses alertes.

Seuil par défaut (0,5)	 Cas équilibré pas parfait, mais bon compromis entre rappel et précision.

Seuil très haut (0,9)	 Cas typique alerte ultra fiable mais trop rare → inutile en RH car on rate la majorité des 		départs.




Cas : “mieux vaut alerter trop que rater des départs”.   Favoriser légèrement le recall plutôt que la precision

Seuil 0,2 (très bas)
Recall(1) = 0.91 → le modèle détecte presque tous les partants (43/47 trouvés).
Precision(1) = 0.22 → mais seulement 22% de nos alertes sont correctes (énormément de faux positifs : 145 !!!). 
Seuil 0.90 (très haut)
Precision(1) = 1.00 → quand on dit “départ”, on as TOUJOURS raison (8/8).
Recall(1) = 0.17 → mais on rate 39 partants sur 47 (seulement 8 détectés).

Seuil 0.50 (par défaut)
Recall(1) = 0.68 (32/47 partants détectés).
Precision(1) = 0.38 (4 alertes sur 10 correctes).

----


Et par rapport au revenu mensuel alors ?

Le coef est proche de 0, ça veut dire qu’il n’apporte pas beaucoup d’info au modèle après prise en compte des autres variables.
Il est probablement redondant avec d’autres (corrélations fortes). 
Et c’est cohérent en pratique : le revenu seul n’explique pas le départ, mais combiné avec la charge de travail, le poste, ou les déplacements, ça devient significatif.


---------------------------------------------------------------------------------------------------------------------------------

Importance des variables (coefficients de la logistique)

Facteurs aggravants du turnover : surcharge de travail (heures sup, déplacements fréquents), postes exposés (consultant, commercial), célibat → logique métier.

Facteurs protecteurs : satisfaction dans le travail, implication dans des dispositifs financiers (PEE), postes à responsabilité (manager, direction).


La régression logistique apprend des coefficients pour chaque variable.
Coefficient positif → augmente la probabilité de départ.
Coefficient négatif → réduit la probabilité de départ.
Valeur absolue élevée → influence plus forte.


----

analyse des erreurs 

FN (rater des départs) = profils plus âgés, expérimentés, bien payés, installés.

 probablement car les raisons de leur départ ne sont pas captées dans les données actuelles.


FP (fausses alertes) = profils plus jeunes, en mobilité (déplacements, heures sup), mais qui finalement restent.

L’analyse des erreurs montre que le modèle rate souvent des salariés expérimentés et stables sur le papier (FN), probablement car les raisons de leur départ ne sont pas captées dans les données actuelles (départ à la retraite ou opportunité externe)

À l’inverse, il produit des fausses alertes sur des salariés plus jeunes et mobiles (FP), dont la charge de travail et les déplacements laissent penser à un risque de départ, alors qu’ils restent dans l’entreprise.

Conclusion pour ce modèle : Il repère surtout les signes visibles de surcharge (heures sup, déplacements), mais il a plus de mal à détecter les causes plus discrètes liées à la carrière (opportunités ailleurs, évolution managériale).

----

Random Forest finetuné
Seuil de décision

Le F1-score (équilibre précision + rappel) culmine autour de 0.50.

Après 0.55, le F1 s’effondre car le rappel chute. Cela confirme que le seuil optimal de la RF fine-tunée est probablement ≈ 0.50 (pas 0.60 comme la logit).


---

Importance native Gini

revenu_mensuel (~9.6%) → la plus utilisée dans les splits → très discriminante pour prédire les départs.

age (~8%) → poids important, peut refléter un effet générationnel.

annee_experience_totale + annees_dans_l_entreprise (~7.5% chacun) → ancienneté/expérience fortes contributrices.

heures_supplementaires (~7%) → logique RH, trop d’heures sup → risque de départ.

distance_domicile_travail (~4.3%) → contrainte logistique.


Plusieurs méthodes de traitement de données pour ce modèle

Méthode Importance native Gini : Rapide, mais peut surévaluer les variables très corrélées ensemble.

Ici on peut voir que le revenu mensuel


---

Permutance Importance

heures_supplementaires (0.16) → de loin la variable la plus critique. Quand on la brouille, le F1 s’effondre.

nombre_participation_pee (0.036) → étonnamment fort impact → peut refléter l’engagement dans l’entreprise.

satisfaction_employee_environnement (0.026) → cohérent, perception de l’environnement.

revenu_mensuel (0.022) → toujours important, mais bien moins qu’en Gini.

annes_sous_responsable_actuel (0.018) → stabilité/ancienneté sous un manager spécifique.

satisfaction_employee_equipe (0.016) → dynamique d’équipe importante.

annees_dans_l_entreprise (0.016) → confirme le rôle de l’ancienneté.

Rappel du principe
On prend le modèle entraîné, on brouille aléatoirement les valeurs d’une feature dans le jeu de test,
Puis on mesure la chute de performance (ex. F1, Recall, Accuracy).
Plus la performance chute, plus la variable était utile. 


---------------------------------------------------------------------------------------------------------------------------------- 

 Ici, ce ne sont pas seulement les variables continues (revenu, âge) qui ressortent, mais aussi des variables plus “RH” (satisfaction, heures sup, engagement PEE).

Ce que représente le graphique
Axe horizontal = baisse moyenne de la métrique (ici le Δ F1) quand la variable est brouillée.→ Plus c’est grand, plus la variable est utile au modèle.
Barres bleues = moyenne de cette baisse de performance sur toutes les répétitions (ici n_repeats=20).→ Donc “l’importance moyenne” mesurée pour chaque feature.
Traits noirs = écart-type (variation) entre les répétitions.→ Ça montre la stabilité de l’estimation.



---

Beeswarm plot SHAP

Heures supplémentaires
 + d’heures sup = + risque de départ.

Âge
Les jeunes employés sont plus exposés à partir.

Revenu mensuel
Bas revenus (bleu) → SHAP positifs (risque ↑).
Haut revenus (rouge) → SHAP négatifs (risque ↓).
 Salaire protège contre le départ.

Satisfaction environnement / équipe
 Plus la satisfaction est forte, plus ça retient les employés.

Statut marital Célibataire
Être célibataire (1 = rouge) → à droite → augmente le risque.
Marié/divorcé (0 = bleu) → plutôt neutre/négatif. Célibataires plus enclins à partir.


Pour le troisième outil d’interprétation, c’est le plus puissant pour comprendre notre modèle, car il donne à la fois :
l’importance globale (comme les deux précédentes),
le sens de l’impact (augmentation ↗️ ou diminution ↘️ du risque de départ),
et même des explications locales (par salarié).

Pour expliquer comment ça fonctionne : 
Si le point est bleu ça signifie quel a valeur est petite => rouge valeur grande
Si le point est situé à droite du graph ça signifie qu’il est considéré comme risque de départ

 

-----------------------------------------------------------------------------------------------------------------------------------------------------------

C’est une vue globale : on observe les tendances générales entre variables et probabilité de départ.
Il montre aussi la variabilité intra-variable : par ex. même pour le revenu, certains hauts salaires peuvent quand même être prédits “à risque” selon d’autres facteurs (d’où la dispersion).
Ce n’est pas une simple importance “moyenne” comme Random Forest : on a aussi le sens (↑ ou ↓ risque).
Pour un modèle fine-tuné Random Forest il apprend une logique très “métier” :
Heures sup + bas salaires + jeunes employés + faible satisfaction = départ probable.
Haut salaires + anciens + satisfaits = restent.

----

Waterfall Plot SHAP Local (FP)


Le modèle “voit” cet employé comme à haut risque de départ (74%)

Facteurs majeurs à risque : jeune, bas salaire, faible ancienneté, pas encore stabilisé avec un manager.

Facteurs protecteurs : pas d’heures sup, satisfait de son environnement, impliqué via PEE.

Mais la balance penche clairement côté risque → les facteurs négatifs pèsent plus fort.

Faux positif : salarié fidèle prédit comme départ → souvent lié à un cumul de signaux "à risque" (ex. bas revenu, jeune âge, heures sup.).


---------------------------------------------------------------------------------------------------------

Structure du graphique
Base value = 0.50 → le modèle, sans info, prédit en moyenne 50% de risque de départ.
Ensuite, chaque variable ajoute (rouge) ou retire (bleu) du risque.
f(x) = 0.743 → au final, pour cet employé, le modèle prédit 74.3% de risque de départ.
Prédiction = parti / Réel = resté


Les facteurs qui augmentent le risque (rouge, à droite)
Année expérience totale (1) : +0.07→ Son profil d’expérience globale contribue à augmenter son risque.
Années dans l’entreprise (1) : +0.07→ Peu d’ancienneté → plus de probabilité de départ.
Revenu mensuel (2033) : +0.06→ Salaire bas → risque ↑.
Âge (24 ans) : +0.05→ Jeune → risque ↑.
Années sous responsable actuel (0) : +0.06→ Peu ou pas de stabilité avec un manager → risque ↑.
 
Les facteurs qui diminuent le risque (bleu, à gauche)
Pas d’heures supplémentaires (0) : -0.03→ Absence de surcharge protège un peu.
Nombre participation PEE (1) : -0.03→ Participation dans l’épargne salariale → engagement.
Fréquence déplacement Aucun (True) : -0.02→ Pas de déplacements → moins de contraintes.
Satisfaction environnement (4/5) : -0.01→ Satisfaction élevée → retient un peu.
Statut marital non célibataire (False) : -0.01→ Être marié/divorcé réduit légèrement le risque.


---

Waterfall Plot SHAP local (FN)


Plusieurs signaux rouges forts (heures sup, jeune, salaire bas, poste à risque).

Mais aussi pas mal de signaux bleus (engagement PEE, distance correcte, stabilité).

Le modèle a tranché en faveur du maintien (0.482 < 0.5 base value) mais en réalité il est parti → Erreur critique 

Ajuster le seuil (par exemple à 0.45) aurait pu sauver cette prédiction.

Faux négatif : salarié parti mais non détecté → souvent lié à un profil atypique (revenu correct, mais variables cachées non captées par les données par ex des opportunités externes).

------------------------------------------------------------------------------------------------------------------------------------------------

la base value est indépendante → c’est la moyenne des probabilités de départ que le modèle a vues.

Structure du graphique
predict_proba = [0.518, 0.482] → 48% de chance de départ seulement → le modèle pense qu’il reste.
predict = 0 → prédiction "reste".
y_test = 1 → en réalité, il est parti → erreur critique (le modèle a raté un départ). ⚠️

Facteurs qui poussaient vers le départ (rouges) :
Heures supplémentaires (oui) : +0.06 → gros facteur de risque de départ.
Âge jeune (29 ans) : +0.03 → population plus mobile.
Expériences précédentes (6) : +0.02 → profil instable, plus de mobilité.
Poste = Représentant Commercial : +0.02 → métier avec turnover fréquent.
Salaire bas (2800) : +0.01 → facteur de risque supplémentaire.

Facteurs qui poussaient vers le maintien (bleus) :
Participation PEE : -0.04 → signe d’engagement.
Distance domicile-travail raisonnable : -0.03.
Statut marital = pas célibataire : -0.02 → souvent associé à stabilité.
Expérience sous responsable actuel : -0.01.
Pas trop de déplacements fréquents : -0.01.



----

Waterfall Plot SHAP local (VN et VP)


Conclusion 
VN = modèle détecte bien la stabilité (revenu haut, ancienneté).
VP = modèle capte bien les signaux de départ (salaire bas, heures sup).
FP = fausses alertes → modèle exagère certains signaux (ex. âge jeune).
FN = ratés → profils ambigus proches du seuil, rappel à améliorer.


----------------------------------------------------------------------------------------------------------

VN
Les facteurs clés ici :
Revenu élevé (11557) : -0.05 → réduit fortement le risque de départ.
participation PEE : -0.04 → signe d’engagement, réduit le risque.
Pas d’heures supplémentaires : -0.04 → bon équilibre pro/perso → réduit le risque.
Expérience longue (5 ans dans l’entreprise, 10 ans totale) : stabilise l’employé → -0.03 et -0.01.
Âge 31 ans : contribue un peu à rester (dans la tranche médiane stable).
Quelques signaux rouges (expériences précédentes = 9, département Commercial) poussent vers le départ, mais trop faiblement pour renverser la tendance.
 Résultat final : beaucoup plus de facteurs bleus que rouges → modèle confiant que l’employé reste (et il avait raison).

Interprétation globale
Ce contraste illustre bien que SHAP permet non seulement de dire “le modèle s’est trompé” ou “il a eu raison”, mais surtout pourquoi.

VP
heures supplémentaires = 1 → +0.08→ Très fort facteur qui pousse vers la prédiction "départ".
revenu_mensuel bas (2042) → +0.06→ Salaire bas → augmente proba départ.
âge jeune (26) → +0.04→ Jeune profil → plus mobile.
participation PEE = 1 → -0.04→ Signe d’engagement → réduit proba départ.



----


Conclusion et recommandations


Pour conclure :
Les modèles identifient des signaux métier pertinents. Et ça constitue un outil d’aide à la décision qui va permettre d’anticiper certains départs, mais doit être utilisé avec précaution, en complément de l’expertise RH.

Variables clés à surveiller 

Il faudrait cibler les jeunes salariés à bas revenus avec de nombreuses heures sup., améliorer l’équilibre pro/perso et renforcer la satisfaction au travail


----------------------------------------------------------------------------------------------------------------------------------

Comparaison méthodes :
Importance native (Gini) met en avant revenu, âge, ancienneté.
Permutation importance confirme surtout l’effet "heures supplémentaires".
SHAP (beeswarm) montre que certaines variables n’ont pas un effet linéaire, ex. un revenu bas pousse au départ, mais un revenu élevé réduit fortement ce risque.


Variables clés à surveiller :

Heures supplémentaires : principal facteur augmentant la probabilité de départ

Revenu mensuel + âge + ancienneté : corrélés fortement avec la rétention (plus faibles = plus de risques)

Satisfaction au travail (équipe, environnement, équilibre pro/perso) : protecteurs contre le départ

Données incomplètes : certaines variables métier manquent (qualité du management, opportunités externes…)

Cibler en priorité les salariés : 

Jeunes, bas revenus, souvent en heures supplémentaires, faible satisfaction (travail, équipe, environnement)


Actions possibles :

Politiques RH pour équilibrer la charge de travail

Augmentations ciblées pour salariés à risque

Programmes de fidélisation (formation, participations engagements interne)



