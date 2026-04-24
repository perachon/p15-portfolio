
QUESTIONS

1) 	SLIDE 1 – Contexte

❓ C’est quoi le scoring ?

Le scoring, c’est le fait d’attribuer un score numérique à un individu pour estimer un risque ou une probabilité.
Score = probabilité qu’un client fasse défaut.


❓ Pourquoi industrialiser un modèle déjà performant ?

Un modèle performant en notebook n’a aucune valeur business.
La valeur vient de sa capacité à être utilisé de manière fiable, monitorée et maintenable en production.


❓ En quoi ton travail relève du MLOps ?

J’ai mis en place une chaîne complète :
API, Docker, CI/CD, logging structuré, monitoring drift et optimisation performance.



2) SLIDE 2 – Contexte métier

❓ Pourquoi la traçabilité est-elle critique en credit scoring ?

Pour des raisons réglementaires et de conformité (RGPD, audit interne).
Chaque décision doit être explicable et traçable.


❓ Que se passe-t-il si le modèle dérive ?

Il peut accorder trop de crédits risqués ou refuser trop de bons profils, ce qui a un impact financier direct.



3) SLIDE 3 – Objectifs

❓ Pourquoi prioriser le monitoring ?

Parce que le risque principal en production n’est pas le modèle initial, mais la dérive dans le temps.


❓ Pourquoi séparer logging et monitoring ?

Le logging stocke les événements.
Le monitoring analyse ces événements pour détecter des anomalies.



4) SLIDE 4 – Architecture

❓ Pourquoi Docker ?

Reproductibilité, portabilité, cohérence environnement dev / prod.


❓ Pourquoi FastAPI ?

Performance, validation intégrée via Pydantic, documentation automatique.


❓ Comment ton système détecte-t-il une anomalie ?

Analyse statistique des latences et test KS pour la dérive des distributions.


❓ Et si la latence explose ?

On identifie si c’est un problème :
- de cold start
- de CPU
- d’imports
- de parallélisation



5) SLIDE 5 – API

❓ Pourquoi valider côté API si le modèle a été entraîné proprement ?

Parce que les données production peuvent être corrompues, mal formées ou incomplètes.


❓ Pourquoi distinguer 422 et 500 ?

422 = erreur utilisateur
500 = erreur système
Cela facilite le debugging et le monitoring.


❓ Pourquoi logger les inputs et outputs ?

Pour analyser la dérive des données et pouvoir reconstruire un incident si nécessaire.


❓ Pourquoi un endpoint /health ?

En production, on doit pouvoir vérifier rapidement que le service est opérationnel sans déclencher une prédiction complète.


❓ Pourquoi utiliser FastAPI ?

FastAPI permet une validation automatique des données, une documentation Swagger intégrée et des performances élevées.


6) 

❓ Pourquoi le CI/CD est crucial en MLOps ?

Parce qu’un modèle ML évolue souvent.
Sans automatisation, le risque d’introduire des bugs en production est élevé.


❓ Que testes-tu exactement ?

Le bon fonctionnement de l’API
La validité des réponses
Les cas d’erreurs
Le comportement du modèle


❓ Que se passe-t-il si un test échoue ?

Le déploiement est bloqué.
Cela évite d’envoyer en production une version instable.


7) 

❓ Pourquoi du JSON ?

Format structuré, facilement exploitable par des outils de monitoring.


❓ Pourquoi logger les outputs ?

Pour analyser l’évolution des prédictions dans le temps et détecter un drift.


❓ Est-ce conforme RGPD ?

Oui, car on peut anonymiser les données sensibles et limiter le stockage aux variables nécessaires.



8) Test automatisés


❓ Pourquoi tester une API simple ?

Parce qu’en production, même une petite modification peut introduire une régression.
L’automatisation garantit la stabilité dans le temps.



9) Data drift

❓ Pourquoi tester la probabilité et pas les features ?

La probabilité est un bon indicateur global.
Mais idéalement, il faudrait monitorer aussi les features critiques.


❓ Que ferais-tu si drift détecté ?

- Analyse des features responsables
- Retrain du modèle
- Revalidation
- Redéploiement via CI/CD



10) Analyse des perfs

❓ Pourquoi la médiane est plus faible que la moyenne ?

Parce que la première requête tire la moyenne vers le haut.


❓ Comment as-tu identifié le goulot d’étranglement ?

Via cProfile qui montre que le temps était principalement consommé lors du chargement du modèle.

Streamlit Dashboard


11) Optimisations

❓ Pourquoi ne pas utiliser ONNX ?

ONNX est pertinent pour des modèles lourds ou en environnement haute performance.
Dans ce cas, le goulot n’était pas l’inférence mais le chargement initial.


❓ As-tu mesuré l’impact sur la précision ?

Oui, l’optimisation n’a pas modifié le modèle, uniquement son chargement.



12) Slide Data Drift

❓ Pourquoi utiliser le test KS ?

Parce qu’il permet de comparer deux distributions sans hypothèse forte sur leur forme.


❓ Pourquoi surveiller la probabilité ?

C’est un indicateur global du comportement du modèle.
Mais en production réelle, on surveillerait aussi les features critiques.


❓ Si drift détecté, que fais-tu ?

Analyse des variables responsables,
retrain du modèle,
validation,
redéploiement via CI/CD.



13) Slide Performance

❓ Pourquoi la latence moyenne est élevée ?

À cause du cold start lié au chargement du modèle.


❓ Comment as-tu identifié le goulot ?

Avec cProfile qui montre que le temps était consommé par le téléchargement et les imports.


❓ Pourquoi ne pas utiliser GPU ?

LightGBM est déjà très rapide en CPU.
Le goulot n’était pas l’inférence mais le chargement initial.



14) Slide Optimisation

❓ Quel est le gain concret ?

Suppression de la latence initiale importante.
Temps de réponse stable et prévisible.


❓ As-tu modifié le modèle ?

Non, uniquement le processus de chargement.



15) Slide Limites

❓ Ton système est-il scalable ?

Oui via Docker.
Il pourrait être déployé sur Kubernetes pour scaler horizontalement.


❓ Qu’est-ce qui manquerait pour une vraie prod ?

Monitoring temps réel, alerting automatique, observabilité complète.


.......

❓ Si tu devais améliorer une seule chose, ce serait quoi ?

Réponse intelligente :

Ajouter un monitoring plus fin sur les features critiques et un système d’alertes automatiques.
