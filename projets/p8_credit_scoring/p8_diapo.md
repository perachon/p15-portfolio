Crédit Scoring API

Projet de déploiement, monitoring et optimisation d’un modèle de scoring crédit réalisé dans un contexte MLOps.

----

Le modèle ici aide à décider si un crédit peut être accordé ou refusé, en estimant la probabilité de défaut d’un client.
L’enjeu métier c’est de réduire le risque de défaut tout en maintenant un bon taux d’acceptation.

Mais au-delà de la performance du modèle, le vrai enjeu est de rendre la décision d’accorder ou refuser un crédit soit fiable, traçable, monitorée et maintenable dans le temps.

L’objectif de cette mission était de passer d’un modèle de scoring développé en environnement Data Science à une solution prête pour la production.
On a souvent l’habitude de travailler en notebook, avec des tests ponctuels. Mais en production, les enjeux sont différents : stabilité, surveillance, gestion des erreurs, performance.

Le but était donc d’industrialiser le modèle et de mettre en place un cadre MLOps complet.

En production, une mauvaise décision peut avoir un impact financier direct, ce qui rend crucial le contrôle des données, des performances et des erreurs.

---

Les objectifs étaient multiples : rendre le modèle accessible via une API, garantir sa robustesse, surveiller son comportement dans le temps et améliorer ses performances. L’idée n’était pas seulement d’avoir un modèle précis, mais un système exploitable en conditions réelles.

J’ai structuré le projet autour de 4 axes :
- Exposer le modèle via une API stable
- Automatiser les tests et le déploiement
- Mettre en place du monitoring
- Analyser et optimiser les performances
L’idée était de couvrir tout le cycle de vie d’un modèle en production.

---

L’architecture repose sur une API FastAPI exposant le modèle.
L’ensemble est conteneurisé avec Docker pour garantir la reproductibilité.
Un pipeline CI/CD automatise les tests, le build et le déploiement.
Enfin, un système de logging structuré permet d’analyser les performances, les erreurs etc avec une partie Monitoring qui permet d’identifier des latences anormales, de détecter une dérive des données, et d’analyser les erreurs applicatives.

L’objectif est de pouvoir détecter automatiquement un comportement anormal du modèle en production et pouvoir intervenir rapidement.

---

Le modèle est stocké sur Hugging Face Hub, ce qui permet de centraliser la gestion des versions.

L’API est ensuite déployée sur Hugging Face Spaces, ce qui permet d’avoir un endpoint accessible publiquement 
pour pouvoir l’utiliser à n’importe quel moment sur n’importe quel appareil

Chaque modification déclenche un rebuild automatique du service.
Cela démontre que la solution est portable et déployable sans environnement local spécifique.

Ce choix simplifie le déploiement tout en restant compatible avec une infrastructure cloud classique.

---

L’API expose deux endpoints 

Le premier est /health, qui permet de vérifier que le service est actif. C’est un endpoint simple mais essentiel en production, notamment pour les systèmes de monitoring qu’on soit pas obliger de déclencher une prédiction complète pour vérifier que le service fonctionne.

Le second endpoint est /predict, qui reçoit les données d’un client et retourne une probabilité de défaut ainsi qu’une décision métier basée sur un seuil configurable qui transforme la probabilité en décision ACCEPTED ou REFUSED.

Les données sont validées automatiquement grâce à Pydantic ce qui garantit que le modèle ne reçoit que des entrées cohérentes.

---

Le pipeline CI/CD automatise le cycle de vie du projet.
C-à-d que À chaque commit, les tests sont exécutés automatiquement. Si les tests passent, l’image Docker est reconstruite puis déployée.

Cela permet de garantir que chaque modification respecte la stabilité du système et évite les régressions en production en réduisant les erreurs humaines et d’assurer une livraison continue du service.
C’est un élément clé en MLOps car les modèles évoluent régulièrement.

---

Il était essentiel de mettre en place des tests automatisés.
J’ai testé :
- le bon fonctionnement de l’endpoint /predict,
- la validation des données via Pydantic,
- les erreurs 422 et 500,
- et la cohérence des réponses.
Ces tests garantissent que chaque modification du code ne casse pas le comportement attendu et que ça reste fonctionnelle et robuste même après des évolutions du code ou du modèle
En production la stabilité est très importante.

---

Chaque requête est loggée au format JSON.

Elle enregistre :
- L’identifiant de la requête,
- le temps de réponse,
- le code HTTP,
- les inputs,
- les outputs,
- et les erreurs éventuelles.
- Prédiction et la décision finale

Ces logs sont ensuite exploités pour analyser la latence et détecter des comportements anormaux.
Cela permet à la fois de surveiller les performances de l’API et de tracer les décisions prises par le modèle, ce qui est essentiel en contexte bancaire.

Sans monitoring, un modèle peut se dégrader sans que personne ne s’en rende compte.

------

C’EST QUOI ? Le data drift, c’est quand les données en production ne ressemblent plus aux données utilisées pour entraîner le modèle.
Le modèle qui se dégrade ou qui a mal évolué.

Pour détecter une dérive, j’ai comparé les distributions des probabilités entre les données d’entraînement et les données simulées en production.
J’ai utilisé un test statistique de Kolmogorov-Smirnov pour comparer les deux.

Le résultat (p-value) ne montre pas de dérive significative.
Cela signifie que le comportement global du modèle reste stable.

En cas de dérive, il faudrait analyser les variables responsables, vérifier si la relation entre variables et cible a changé, puis éventuellement reentrainer le modèle et redéployer.
Cette étape est essentielle pour garantir la fiabilité du modèle dans le temps.

----

J’ai analysé les temps de réponse moyen en production.
La première requête était plus lente à cause du chargement initial du modèle depuis Hugging Face.
Les requêtes suivantes sont nettement plus rapides et stables
Cela m’a permis d’identifier un phénomène de cold start.

C’est un peu comme un Diesel, ça a du mal a démarré et après c’est parti

---

Pour corriger le problème de cold start, j’ai préchargé le modèle au démarrage de l’application pour optimiser le processus de chargement afin d’éviter un téléchargement répété.
Cela supprime le coût du téléchargement lors de la première requête.

Résultat : réduction significative du temps de latence initial. (L’inférence elle-même était déjà rapide grâce à LightGBM)
Les performances deviennent stables et prévisibles.

Les optimisations n’ont introduit aucune régression fonctionnelle ni métier, tout en améliorant fortement l’expérience utilisateur.
La latence est passée de plusieurs secondes à quelques millisecondes.

---

Là on est sur une solution qui fonctionne bien pour un prototype.
Mais évidemment, il y a des choses qu’on pourrait améliorer.
Par exemple, aujourd’hui je surveille surtout la probabilité prédite pour détecter le drift. Idéalement, il faudrait aussi surveiller encore plus de features qui peuvent être importantes.
Si on passait en vraie production, on mettrait en place des alertes automatiques et une infrastructure plus scalable (= capable de gérer plus de trafic)

Une infrastructure scalable permet d’ajouter automatiquement des instances du service lorsque le trafic augmente, afin de maintenir des temps de réponse stables.

---

Pour conclure, l’objectif était de transformer un modèle de scoring développé en environnement Data Science en un service prêt pour la production.

L’API est aujourd’hui déployée, conteneurisé avec Docker, testée automatiquement, monitorée et optimisée.

On peut surveiller sa performance dans le temps, détecter une dérive éventuelle et faire évoluer le modèle via un pipeline structuré.

Donc on est vraiment passé d’un modèle Data Science à une solution MLOps complète, avec une architecture claire et extensible

Ce projet montre que la valeur d’un modèle ne repose pas uniquement sur sa performance statistique, mais aussi sur sa robustesse et sa capacité à être maintenu dans le temps.


