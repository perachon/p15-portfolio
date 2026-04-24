
QUESTIONS

“C’est quoi FAISS ?”

FAISS est une base de données spécialisée pour la recherche de similarité entre vecteurs.
Elle permet de retrouver rapidement les documents les plus proches d’une requête, non pas par mots-clés, mais par sens.

Concrètement, FAISS stocke des représentations numériques des documents, appelées embeddings, et permet de retrouver très efficacement ceux qui sont les plus similaires à une question donnée.

----

"C’est quoi un embedding ?"

Un embedding est une représentation numérique d’un texte, sous forme de vecteur, qui capture son sens.
Deux textes qui parlent du même sujet auront des embeddings proches, même s’ils n’utilisent pas les mêmes mots.

Exemple concret

“Par exemple, ‘conférence scientifique’ et ‘séminaire de recherche’ seront proches sémantiquement, même si les mots sont différents.”

----

“Pourquoi utiliser des embeddings plutôt que des mots-clés ?”

Parce que les embeddings permettent une recherche sémantique.
On ne cherche pas uniquement des mots identiques, mais des contenus qui ont le même sens.

----

C’est quoi le chunking ?

Le chunking consiste à découper les documents en petits segments de texte avant de les indexer.
c’est nécessaire car les descriptions longues sont difficiles à exploiter telles quelles.
En les découpant, on améliore la précision de la recherche et on évite de fournir trop de contexte inutile au modèle.

----

Comment as-tu découpé les documents ?

Les documents sont découpés en segments de taille fixe, avec un léger chevauchement entre eux.

J’ai utilisé des segments d’environ 800 caractères avec un chevauchement d’environ 120 caractères.
ce n’est pas arbitraire, mais un compromis :
- assez long pour garder du contexte
- assez court pour rester précis

----

Pourquoi un chevauchement entre les segments ?

Le chevauchement permet d’éviter de couper une information importante entre deux segments.
Cela améliore la continuité du contexte lors de la recherche et de la génération.

----

Est-ce que ce choix est optimal ?

C’est un compromis raisonnable pour ce projet.
Les tailles de chunks et le chevauchement peuvent être ajustés selon la nature des données et les performances observées.

----

Pourquoi ne pas interroger directement le JSON ?

Parce que la recherche sémantique nécessite des embeddings et une structure optimisée pour la similarité vectorielle.
Un fichier JSON n’est pas adapté à ce type de recherche performante.

Le JSON est la source des données, FAISS est l’index, et l’API interroge uniquement FAISS au moment des requêtes.