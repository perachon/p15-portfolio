# Rapport — P9 Cadrage d’un projet IA (Fashion-Insta)

## 1. Contexte et besoin

- Entreprise (scénario) : **Fashion-Insta** (magasins + e-commerce), marché concurrentiel, enjeu d’expérience client digitale.
- Problème : difficulté pour les clients à retrouver des articles similaires à un style via des mots-clés/filtres.
- Opportunité : utiliser la **photo** comme point d’entrée pour recommander des articles similaires.

## 2. Objectif du PoC IA

- But : démontrer la **faisabilité technique** de la recommandation par image.
- But : valider la **valeur métier** de l’IA pour Fashion‑Insta.
- Périmètre : volontairement limité (PoC) — pas un produit final.
- Fonctionnalité centrale : entrée = photo utilisateur, sortie = **top N** articles visuellement similaires ; analyse purement visuelle (pas de personnalisation dans le PoC).

## 3. Approche technique (PoC)

Approche principale (extraits) :

- Modèle de vision par ordinateur **pré-entraîné** (Vision Transformer / CNN).
- Extraction d’empreintes visuelles (**embeddings**).
- Recherche de similarité dans le catalogue (ex. distance cosinus) pour retourner les produits les plus proches.

Approche alternative (mentionnée) :

- Modèle multimodal image + texte (type CLIP / LLM multimodal) : plus riche sémantiquement mais plus coûteux/complexe, option selon résultats.

## 4. Données et outils

- Données : images produits du catalogue ; datasets publics de mode en complément si besoin.
- Cloud : Microsoft Azure (partenaire cloud).
- Données personnelles : dans le PoC, **pas de données personnelles clients** (extrait).

## 5. Critères de succès (Go / No Go)

Critères métier (extraits) :

- Recommandations jugées pertinentes par les équipes métier.

Critères techniques (extraits) :

- Recall@K (top 5) et Precision@K.
- Temps d’inférence < 500 ms.

Décision : Go/No Go selon validation croisée des critères techniques et métier.

## 6. Déroulé, staffing et timeline

Déroulé PoC (extraits) :

- Cadrage & préparation des données.
- Tests de plusieurs modèles.
- Évaluation métier.
- Synthèse & recommandations.

Durée et profils (extraits) :

- Durée PoC : 4 à 8 semaines.
- Profils : 1 Data Scientist ; 1 AI Engineer / Data Engineer ; 1 référent métier mode.

Timeline globale (extraits) : trajectoire progressive ~6 mois (cadrage → PoC → MVP → Run/pilote) avec gouvernance (Copil/Coproj).

## 7. System design (vue d’ensemble)

- User → ingestion image (API sécurisée) → stockage temporaire.
- Inférence : modèle vision extrait un embedding.
- Moteur de similarité : comparaison embedding vs catalogue indexé (embeddings pré-calculés).
- Filtrage règles métier → retour top N recommandations.
- Scalabilité : services managés Azure ; point critique = service d’inférence (latence & coût compute).

## 8. Coûts, ROI (extraits)

- Coût principalement RH : ~116 500 €.
- Coûts techniques one-shot : ~10 000 €.
- Coûts récurrents Run : ~20 000 € / an.
- Coût total première année (extrait) : ~146 500 €.

ROI (hypothèses marketing, extraits) :

- Gains annuels estimés : ~468 000 € (hausse attendue 14% web et 4% magasin).
- Rentabilité atteinte dès la première année (extrait).

## 9. RGPD : risques et mesures (extraits)

Principes RGPD clés (extraits) :

- Finalité explicite ; minimisation ; consentement ; durée de conservation limitée ; sécurité/confidentialité ; droits des personnes.

Risques identifiés (extraits) :

- Images pouvant contenir des personnes ; ré-identification indirecte.
- Réutilisation non maîtrisée ; conservation excessive.
- Exposition via APIs externes.
- Données personnelles indirectes (nom/email/identifiant).

Mesures (extraits) :

- Gouvernance : consentement explicite ; finalité recommandation uniquement ; pas d’entraînement de modèles tiers/LLMs sur images utilisateurs.
- Techniques : stockage séparé images utilisateurs / catalogue ; suppression automatique ; chiffrement au repos/en transit ; logs anonymisés.
- Minimisation : privilégier embeddings ; suppression de l’image brute après traitement.
- Fournisseurs : pas d’envoi d’images à des APIs publiques ; modèles hébergés sur Azure.
- DPIA : envisagée avant déploiement à grande échelle (phase MVP).

---

Cas d’étude OpenClassrooms — synthèse du projet.
