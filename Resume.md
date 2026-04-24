# Résumé de projet — Gabarit + prompt à envoyer dans chaque chat

Objectif : récupérer, pour **chaque projet**, les infos nécessaires pour (1) choisir le meilleur candidat pour le P15, (2) rédiger le rapport, (3) alimenter la carte mentale et le portfolio.

## Mode d’emploi (très simple)

Pour chaque projet :

1) Ouvre le chat du projet (ou un nouveau chat dédié).
2) Copie-colle le **Prompt exact** ci-dessous.
3) Remplace seulement les éléments entre `<<< >>>`.
4) Demande une réponse en **Markdown**.
5) Colle ensuite la réponse dans un fichier : `projets-resumes/<nom_du_projet>.md` (tu peux le créer plus tard, l’important est d’avoir le contenu).

Conseils :
- Si une info est inconnue, le chat doit écrire `TBD` (pas inventer).
- Si plusieurs variantes existent (plusieurs modèles, plusieurs datasets), demander un mini-comparatif.

---

## Prompt exact à copier-coller (à envoyer à chaque chat)

Copie-colle tout ce bloc dans une doc genre Resume.md avec tout ce que je te demande en dessous et remplace uniquement les champs `<<< >>>` :

"""
Tu es mon assistant. Je veux que tu remplisses un **gabarit de résumé** pour un projet, afin de sélectionner un projet pour un portfolio (AI Engineer) et rédiger un rapport de conduite de projet.

Règles :
- Réponds en **Markdown**.
- **Ne fabrique rien** : si tu n’es pas sûr d’une info, n'inventes rien, mets `TBD`.
- Sois **factuel** et concis, mais donne les **chiffres/métriques** quand ils existent.
- Si tu mentionnes un livrable (repo, notebook, démo), ajoute le lien exact si tu l’as.
- Si plusieurs variantes existent (plusieurs modèles, plusieurs datasets), demander un mini-comparatif.

Infos disponibles (contexte du projet) :
<<<COLLE ICI le texte du contexte, exigences, consignes, ou l’historique du chat du projet>>>

Maintenant, remplis le gabarit suivant (sans changer les titres) :

# Fiche projet — <<<TITRE DU PROJET>>>

## 0) TL;DR (5 lignes max)
- Objectif :
- Données :
- Approche :
- Résultat :
- Statut (POC / production / démo) :

## 1) Contexte & organisation
- Type d’organisation / “client” (réel ou fictif) :
- Parties prenantes :
- Enjeux business/usage :
- Contraintes (sécurité, infra, RGPD, scalabilité, latence, budget) :

## 2) Besoin métier (problem framing)
- Problème précis à résoudre :
- Utilisateurs finaux :
- Décisions que le modèle/système doit aider à prendre :
- KPI métier (comment on mesure la valeur) :
- KPI techniques (métriques ML/IA, latence, coût, robustesse) :

## 3) Données
- Sources (où, comment, fréquence) :
- Volume (lignes, taille) :
- Variables/colonnes clés :
- Qualité (valeurs manquantes, bruit, labels) :
- Split (train/val/test), fuites possibles :
- Risques (biais, RGPD, droits) :

## 4) Approche IA/ML (ou LLM)
- Type de tâche (classification, régression, NLP, RAG, etc.) :
- Baseline :
- Modèles testés :
- Features / preprocessing :
- Stratégie d’évaluation (CV, holdout) :
- Métriques + résultats chiffrés :
- Erreurs fréquentes / limites :

## 5) Solution & architecture
- Composants (data prep, entraînement, inférence, UI/API) :
- Flux (entrée → traitement → sortie) :
- Déploiement (API, batch, app) :
- Sécurité (auth, secrets, PII) :

## 6) MLOps / industrialisation (même partiel)
- Versioning data/modèles (DVC/MLflow/registry) :
- Reproductibilité (env, lock, seeds) :
- CI/CD (tests, lint, build) :
- Monitoring (perf, data drift, concept drift, logs) :
- Retraining (déclencheur, fréquence, validation) :
- Orchestration (Airflow, Prefect, etc.) :
- Coût infra (estimation ou drivers) :

## 7) Pilotage projet
- Planning / jalons :
- Livrables produits :
- Risques identifiés + mitigation :
- Arbitrages (ce qui a été coupé et pourquoi) :

## 8) Mon rôle & collaboration
- Ce que j’ai fait moi-même :
- Ce que j’ai appris :
- Ce que je ferais différemment :

## 9) Liens & preuves
- Repo :
- README :
- Notebook(s) :
- Démo (URL / vidéo) :
- Slides/rapport :

## 10) Alignement P15 (très important)
Dis si ce projet permet de couvrir ces points, avec 1 phrase chacun :
- Collecte besoins & contexte orga :
- Audit solution data :
- Identification solution technique :
- Aide à la décision (risques/KPI/coûts) :
- Pilotage (délais/coûts/livrables/perf) :

## 11) Recommandation
- Est-ce un bon candidat “projet personnel technique” pour le portfolio ? (oui/non)
- Pourquoi :
- Les 3 améliorations qui le rendraient excellent pour un portfolio :
"""

---

## Variante de prompt (si tu veux une sortie encore plus exploitable)

À la fin du prompt exact, ajoute :

"""
En plus, propose :
1) Une phrase de pitch “recruteur” (1 ligne)
2) Un plan de README (titres seulement)
3) Une mini-roadmap d’industrialisation en 5 étapes
"""