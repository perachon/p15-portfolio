Fashion Trend Intelligence

Présentation di projet

Créer un système automatisé d'analyse des tendances vestimentaires sur les réseaux sociaux dans le but d'offrir des informations précises et en temps réel sur les nouvelles modes émergentes, bien avant qu'elles ne deviennent populaires.

L’objectif global est de concevoir, développer et déployer un système d'analyse qui aura les fonctionnalités suivantes :

Segmentation vestimentaire : être capable d'identifier et d'isoler avec précision chaque pièce vestimentaire dans une image.

Analyse stylistique : être capable de classifier les pièces selon leur nature, couleur, texture et style.

Agrégation de tendances : être capable de compiler ces données sur des milliers de publications pour identifier les tendances émergentes.


----

Fonctionnement de Segformer

Modèle de segmentation sémantique à base de Transformer.
Encoder : transforme l’image en patchs via un backbone Transformer (Mix Vision Transformer - MiT).
Decoder léger : fusionne efficacement les différentes résolutions pour prédire les classes de chaque pixel.
Très bon compromis entre : vitesse d’inférence, précision et consommation mémoire
Fonctionne bien même sur des images complexes (habits, textures, poses variées)
Capable de détecter différentes pièces vestimentaires dans une image.


----
Proposition d’une méthode de validation du modèle

IoU (Intersection over Union) a été choisie comme métrique principale.

Elle est parfaitement adaptée aux tâches de segmentation sémantique, pour les calculs du IoU par classe, ainsi que des moyennes globales à partir des masques prédits et des masques réels (ground truth) du jeu de données.




Le modèle atteint un IoU global de 74%, ce qui traduit une bonne qualité de segmentation globale, notamment sur les classes dominantes. 
Certaines classes fines restent plus difficiles à segmenter mais le modèle est largement exploitable pour des cas d'usage industriels.

----

✅ Traitement local (projet actuel)

Utilisation locale du modèle SegFormer via transformers – aucune requête vers l’API Hugging Face.
0 € (hors coût machine locale) et besoin d’espace disque


☁️ API Hugging Face – Inference API
Traitement à distance (~1s/image). Plan Basic à $0.10/minute au-delà des 30 premières minutes.
~800 $ USD (140h de calcul)


----
Bilan du proejt réalisé

Les défis techniques rencontrés 
Alignement des masques prédits et réels : attention au nommage (image_0.png ↔ mask_0.png) pour assurer la cohérence. 
Prétraitement des images : éviter la déformation via redimensionnement avec padding.
Chargement des bons formats : gestion dynamique des extensions .png, .jpg, etc. via .env.
Évaluation rigoureuse : nécessité de comparer les masques générés aux masques « ground truth » pour des scores justes.
Vos idées pour améliorer le système
Filtrage par classe : ignorer les classes à faible confiance ou trop rares (ex : ceinture).
Amélioration de la lisibilité visuelle : ajout de légendes dynamiques et overlay avec contours.
Les applications potentielles pour les clients de ModeTrend
Essayage virtuel intelligent : détection et segmentation automatique des vêtements portés.
Recherche visuelle : retrouver un vêtement en photo dans la base catalogue.
Personnalisation marketing : recommandations en fonction des habits portés par les utilisateurs.
Analyse de tendances : identification automatisée de types de vêtements populaires.






