Déploiement d'un modèle ML



Objectif du projet

Mettre en place une solution complète de déploiement de modèle ML incluant CI/CD, API, base de données et tests automatisés.


Missions clés

Automatiser les tests et le déploiement (CI/CD)
Exposer le modèle via une API FastAPI
Assurer la traçabilité des données dans PostgreSQL
Garantir la fiabilité via une suite de tests
Documenter l’ensemble du projet


( Stack technique )

Langage : Python 3.9
Framework API : FastAPI
DB : PostgreSQL + SQLAlchemy
Pipeline CI/CD : GitHub Actions + Hugging Face Spaces (Docker SDK)
Tests : Pytest + Coverage
Documentation : Swagger / Markdown

---------

structure du dépot et CI/CD
Git & versions

Je me suis basé sur un workflow standard qui est le trunk-based development, dont le principe est : Une branche principale : main (ou master). Chaque dev crée de petites branches (ex : feat/api-endpoint)

Commits atomiques = une modification unique, cohérente, clair et complète qui ne fait qu’une seule chose, 

PR obligatoires = Pull Request sert à demander la fusion (merge) d’un ensemble de commit, d’une branche. Avec de la relecture, des tests, des commentaires d’une personne de l’équipe pour une validation.

Les tags et releases qui vont permettrent de marquer les versions stables du projet
( Tags : v0.3.0 → Dummy Model, v0.4.0 → PostgreSQL, v0.5.0 → Tests complets )


---

CI/CD (Continuous Integration / Continuous Deployment) 

Qu’est-ce que c’est ?

Installe les dépendances → récupère tout ce qu’il faut pour exécuter le projet (ex. : pip install -r requirements.txt).
Exécute Pytest → lance tous les tests unitaires pour s’assurer que le code fonctionne.
Calcule la couverture de tests → mesure quelle proportion du code est testée (ex. : 85% de couverture).
But : s’assurer que chaque modification ne crée pas de bug avant d’être fusionnée.

2eme point : Si la CI est réussie (tests verts) Alors le workflow déploie le projet sur Hugging
But : automatiser la mise en ligne du code validé sans intervention manuelle.

3eme point : Objectif : sécuriser les identifiants utilisés dans le déploiement.

( HF_TOKEN est un jeton d’accès personnel à Hugging Face. ) Il est stocké dans les “GitHub Secrets” → il n’apparaît jamais en clair dans le code.
GitHub Actions peut y accéder via ${{ secrets.HF_TOKEN }}.
But : éviter d’exposer des clés sensibles dans le dépôt public.

Et enfin la protection de branche pour garantir la qualité du code avant fusion dans la branche principale
On empêche de fusionner une PR si la CI n’est pas verte. Optionnellement, peut exiger des reviews humaines ou un nombre minimal d’approbations.

----

Ici on peut voir (Swagger UI) avec les différents endpoints travaillés : [.]

L’API est construite autour de FastAPI

/health pour vérifier que l’API et la base de données fonctionnent
/predict pour tester la chaîne CI/CD avec un modèle simple
/p4/predict pour exposer le vrai modèle de machine learning d’attrition, d’une prédiction d’une future démission d’un salarié

Chaque endpoint a une documentation, un schéma de réponse et une gestion d’erreurs claire.

--------------------------------------------------

Développement de l'api FastApi 

Validation Pydantic

Chaque donnée d’entrée est typée avec StrictFloat, ce qui impose uniquement des valeurs numériques réelles acceptées.
( Par exemple « 5.0 » (string) acceptée mais pas « abc » )

Et FastAPI utilise Pydantic pour valider automatiquement le corps de la requête JSON avant même d’exécuter le code du endpoint.
Cela garantit que le modèle ne reçoit que des données cohérentes et bien formatées.


Documentation

/docs qui est le Swagger qui est
Interface interactive générée automatiquement par FastAPI. Elle permet de tester les endpoints en direct et de visualiser les schémas de requête/réponse.

Avec des modèles PredictRequest et Response auto-générés
Ces deux classes définissent les structures de données échangées entre le client et l’API. Et ça va générer automatiquement :
Les schémas JSON dans Swagger
Les types de données attendues
Les exemples d’appels d’API

-------

Intégration du modèle ML & PostgreSQL

Pour l’intégration du modèle ML j’ai utilisé directement des artefacts

Un artefact = un fichier produit par le pipeline d’entraînement qu’on va pouvoir réutiliser pour des déploiements ou de l’analyse.

Ces artefacts proviennent du travail réalisé sur le projet 4. 
Le .joblib contient le modèle entraîné, et le .json décrit la structure d’entrée attendue.
L’API charge ces fichiers automatiquement au démarrage pour garantir la cohérence entre les requêtes reçues et le modèle utilisé.
( C’est ce qu’on appelle la séparation du code et du modèle : l’API ne réentraîne pas, elle sert uniquement à prédire. )

----

Le endpoint /p4/predict suit une logique en quatre étapes :

Input JSON
Validation de la requête par Pydantic,
Envoyé dans le code du pipeline ML,
Ce qui va nous renvoyer une Prédiction et calcul des probabilités,
Et l’étape d’enregistrement de la requête et de la réponse dans la base de données.

[…] 

Alors j’ai mis petit exemple de ce que le modèle attend comme paramètre, ici la request avec les features
Et la réponse nous disant que la prédiction est égale à 1 donc que le modèle prétend à ce que le salarié démissionne

( L’API renvoie la classe prédite (0 ou 1), les probabilités associées, la version du modèle et le temps d’inférence.
Les valeurs sont arrondies à trois décimales pour plus de lisibilité. )

---

Pour la base de données :  ces deux tables couvrent les deux usages principaux :
employees → pour stocker les données réelles issues du dataset RH ;
prediction_logs → pour enregistrer toutes les requêtes API et leurs résultats.

Ensemble, ça permet d’assurer la traçabilité complète de chaque prédiction, 
la reproductibilité (c’est la capacité de refaire exactement le même résultat, modèle, prédiction, performance, à partir du même code, des mêmes données et de la même config.)
et d’avoir un historique de performance du modèle.

Ici un exemple, on voit bien qu’il y a 23 lignes de logs dans la table, une fois qu’on fait une demande au modèle ML, un nouveau log apparait et est enregistrée et tracée

( PostgreSQL en local sinon on utilisait au début SQLite aussi pour Hugging Face )
( PostgreSQL j’ai du installé Docker Desktop sur Windows )
http://localhost:5050
admin@example.com / admin
mluser / mlpass

---

Pourquoi des tests ?

L’objectif, c’est garantir que l’API et le modèle se comportent comme prévu, même quand on change du code. 

Structure des tests

Je vérifie la validation Pydantic : types stricts, champs manquants → réponse 422. Ça protège le modèle d’entrées incohérentes.

Je teste le chargement des artefacts (pipeline + schéma) en incluant des cas d’erreur : fichiers manquants, clé feature_order absente 
Je teste l’endpoint /p4/predict : cas OK 200, erreurs 400/422/503. en utilisant des mocks pour rendre le test indépendant des artefacts réels.

Je vérifie la traçabilité côté DB : un appel /p4/predict → +1 ligne dans prediction_logs. On teste la vraie chaîne API → DB.

---

Résultats & couverture
La couverture est d’environ 85% ce qui est une très bonne moyenne pour un projet d’API ML.
Ça inclut tout ce qui est : validation, chargement des artefacts, endpoint, et insertion DB.

( Les lignes non couvertes correspondent surtout à du code de démarrage FastAPI, des handlers d’erreur exceptionnels et des fonctions de log non critiques. )

Intégration Continue (CI)
Les tests tournent automatiquement à chaque push/PR via GitHub Actions. 
Si un test casse, la PR ne peut pas être mergée. On sécurise la branche main pour éviter de se retrouver avec des bugs en PROD

Les tests fonctionnels sont mockés pour être rapides et stables en CI.

Screen
Ici le rapport de couverture généré avec pytest, il résume les lignes exécutées par les tests pour chaque module du projet.
On peut voir ici que la couverture globale est de 85 %


Bonnes pratiques appliquées
Entrées strictes : StrictFloat + erreurs 422 automatiques via Pydantic.
Déterminisme : jeu d’inputs fixes → résultats reproductibles.
Isolation : tests d’intégration utilisent une DB SQLite temporaire pour ne pas toucher la vraie base.
Mocks ciblés : on remplace pipeline/DB quand nécessaire pour tester la logique métier.

Limites & next steps : Prochaines améliorations : tests de performance (latence max) et tests de non-régression du modèle sur un jeu de référence versionné.

-----

Améliorations possibles

Ajouter un endpoint /logs/last : 
Permet de visualiser les 5 dernières prédictions sans passer par pgAdmin ou ligne de commande

Export des logs vers un dashboard (Grafana / Streamlit) :
Pour du monitoring de performances du modèle en production et pouvoir faire des analyses et de la supervision

Migration vers PostgreSQL managé (Supabase) :
Simuler un vrai environnement de prod comme le postgre est uniquement en local

-----



https://huggingface.co/spaces/perachon/p5-ml-deploy-poc-dev


Erreur 422 (Unprocessable Entity)

Si une clé est manquante ou mal typée ; FastAPI renvoie directement une erreur HTTP 422 avec le détail de la validation échouée.




EN LOCAL

Pour la base de données :

docker exec -it p5-postgres psql -U mluser -d mldb -c "SELECT COUNT(*) FROM prediction_logs;

Invoke-RestMethod -Uri "http://127.0.0.1:8000/p4/predict" -Method POST -ContentType "application/json" -Body '{  "heure_supplementaires":5,  "augementation_salaire_precedente":1,  "nombre_participation_pee":2,  "age":35,  "revenu_mensuel":3000,  "annees_dans_l_entreprise":5,  "annee_experience_totale":10,  "satisfaction_employee_environnement":3,  "distance_domicile_travail":12}’


docker exec -it p5-postgres psql -U mluser -d mldb -c "SELECT * FROM employees LIMIT 5;"



Quels défis avez-vous rencontrés lors du développement de l'API avec FastAPI et comment les avez-vous surmontés ?

Les principaux points ont été la validation stricte des entrées, la gestion propre des erreurs 400/422/503, et quelques soucis d’initialisation (événements de startup, import, et dépendances en CI). J’ai verrouillé la validation avec Pydantic, normalisé les réponses d’erreur, et fiabilisé le démarrage (DB + modèle) via des dépendances injectées et un loader mis en cache. Enfin, j’ai corrigé les problèmes CI (httpx manquant, chemin src) et les différences d’environnement (SQLite vs Postgres, Docker/HF).

Validation entrée : Pydantic (StrictFloat) + modèles Predict*Request → 422 automatique si champ manquant/mauvais type.
Codes d’erreur :
422 (validation Pydantic),
400 (erreur d’inférence contrôlée),
503 (modèle/artefacts absents).
Startup / imports :
Avertissement on_event déprécié → prêt à migrer vers lifespan.
No module named 'src' en CI → ajouté src/__init__.py + PYTHONPATH=. dans pytest.ini.
httpx manquant pour TestClient → ajouté à requirements.txt.
Modèle & artefacts : loader avec cache + feature_order dans un input_schema.json pour garantir l’alignement features API ↔ entraînement.
Environnements :
Local/CI: SQLite (fichier) ou Postgres via docker-compose.
Hugging Face: SQLite en /tmp (chemin écrivable) pour éviter “unable to open database file”.
Déploiement HF : Space SDK Docker, push via GitHub Actions + HF_TOKEN secret.

Stratégie de tests & couverture
Pouvez-vous expliquer votre stratégie de tests et comment avez-vous assuré une couverture complète du code ?

“J’ai structuré les tests en 3 niveaux : unitaires (validation & loader), fonctionnels (endpoints FastAPI avec TestClient), et intégration (API ↔ DB). J’ai mocké les artefacts pour être indépendant du disque, et j’ai mesuré la couverture avec pytest-cov (≈85%). Les tests tournent en CI à chaque push/PR, ce qui bloque tout code instable.

Unitaires :
test_schemas_p4.py (Pydantic : types, champs manquants),
test_loader_p4.py (fichiers manquants, feature_order, cache).
Fonctionnels :
test_p4_predict.py : 200 / 400 / 422 / 503, monkeypatch du loader pour simuler pipeline & proba, dependency_overrides pour bypass DB.
Intégration :
test_integration_db_p4.py : crée une SQLite temporaire, override DATABASE_URL, lance l’API et vérifie prediction_logs (+1 ligne).
Couverture : rapport pytest -q --cov=src --cov-report=term-missing (≈ 85%).
Robustesse : tests déterministes (seed côté entraînement), résultats stables, et CI systématique.

Configuration de la DB & intégration avec le modèle ML
Comment avez-vous géré la configuration de la base de données et son intégration avec le modèle ML ?

“En local, j’utilise PostgreSQL via docker-compose (avec pgAdmin), initialisé par un script Python qui crée les tables et charge le dataset. L’API loggue chaque prédiction dans prediction_logs. En CI/HF, je bascule sur SQLite via la variable DATABASE_URL. L’intégration avec le modèle est encapsulée : l’API charge le pipeline + schéma (artefacts), fait l’inférence, enregistre la requête/réponse et renvoie la prédiction.”
Modèle de données (SQLAlchemy) :
employees (dataset RH),
prediction_logs (version du modèle, latence, request/response JSON, timestamp).
Provisioning :
docker-compose.yml lance postgres:14-alpine + pgadmin.
scripts/create_db.py : crée tables + insert du CSV (non versionné).
Routing FastAPI :
get_session() (dépendance) pour une session DB par requête, commit/rollback try/except dans le routeur.
Archi inference :
loader.py → charge pipeline.joblib + input_schema.json (feature_order) et met en cache,
route /p4/predict → construit un DataFrame dans le bon ordre, predict (+ predict_proba si dispo), arrondi proba, loggue en DB.
Environnements :
Local: postgresql://…
CI/HF: sqlite:///… (chemins écrivable, ex /tmp/app.db en container).
Sécurité des secrets : HF_TOKEN en GitHub Secrets (et pas dans le code).




--- API ---

Pourquoi avoir choisi FastAPI ?

Pour sa rapidité, sa compatibilité native avec Pydantic et Swagger, et la simplicité du typage.C’est idéal pour exposer des modèles ML avec validation stricte des données.

Comment gères-tu les erreurs côté API ?

FastAPI renvoie automatiquement les statuts 422 pour les erreurs de validation.Pour les autres, j’utilise HTTPException et un bloc try/except pour lever les bons statuts — 400 pour erreur de prédiction, 503 si artefacts absents.


--- BDD ---

Pourquoi PostgreSQL plutôt que SQLite ?

PostgreSQL est plus robuste et adapté à la production, notamment pour gérer plusieurs connexions simultanées.SQLite reste utile pour les tests et le déploiement léger sur Hugging Face.

Comment garantis-tu la traçabilité des prédictions ?

Chaque appel /p4/predict crée une ligne dans la table prediction_logs avec le model_version, les features, la réponse, et le timestamp.Cela permet de rejouer ou auditer chaque inférence.


--- TESTS ---

Comment as-tu défini ta stratégie de tests ?

J’ai découpé les tests sous plusieurs niveaux :
Unitaire : valider chaque module isolé,
Fonctionnel : tester l’API dans son ensemble,
Intégration : valider les interactions API ↔ DB.
Les mocks garantissent des tests reproductibles et rapides.

Comment as-tu choisi quoi tester en priorité ?

J’ai ciblé les composants critiques : validation des entrées, chargement du modèle et communication DB. L’objectif n’était pas d’avoir 100 % de couverture, mais de sécuriser les flux principaux et les cas d’erreurs

Pourquoi la couverture n’est pas de 100 % ?

Les parties non couvertes concernent du code de démarrage ou de logging, non critiques pour la logique métier.L’objectif n’est pas la perfection, mais la pertinence.


--- ARTEFACTS ---

Comment garantis-tu la compatibilité entre ton modèle et l’API ?

Je charge le modèle avec son schéma d’entrée (input_schema.json) : ça garantit que les features reçues dans l’API correspondent à celles utilisées à l’entraînement.


--- CI/CD ---

Que se passe-t-il si un test échoue sur le pipeline ?	

Le workflow GitHub Actions bloque automatiquement la fusion vers main. Le but est de garantir qu’on ne déploie jamais un code instable.


--- Sécurité / données ---

Comment éviter les fuites ou les erreurs utilisateur ?

La validation Pydantic bloque toute entrée mal typée, et les logs ne contiennent pas de données sensibles. En production, j’ajouterais une authentification et des quotas d’appels à l’avenir


--- Évolution / maintenance ---	

Comment comptes-tu maintenir ce projet sur le long terme ?	

Le pipeline CI/CD et les tests automatisés permettent déjà une maintenance fluide. Ensuite, le versioning du modèle et des données via MLflow serait une prochaine étape.


--- Démonstration / DB ---

Comment puis-je vérifier qu’une prédiction a bien été loggée ?	

Chaque prédiction est insérée dans la table prediction_logs avec la requête, la réponse, la latence et la version du modèle. 
On peut le vérifier directement via pgAdmin ou une requête SQL en ligne de commande


--- Performance ---	

Quel est le temps moyen d’inférence ?

En local, sur un modèle linéaire, on est autour de 8 à 10 ms. En production Docker, ce serait similaire ou légèrement plus lent selon la charge CPU.


------



Idée														Bénéfice

Ajouter un endpoint /logs/last								Permet de visualiser les 5 dernières prédictions sans passer par pgAdmin.
Ajout de migrations avec Alembic							Gérer les évolutions de la DB proprement dans le temps.
Export des logs vers un dashboard (Grafana / Streamlit)		Monitoring en production.
Authentification API (JWT)									Sécuriser les requêtes.
Déploiement de Postgres managé (RDS / ElephantSQL)			Simuler un vrai environnement de prod.