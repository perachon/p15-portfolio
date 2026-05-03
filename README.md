# P15 — Portfolio AI Engineer (OpenClassrooms)

Ce repo sert à construire un **portfolio** (P15) à partir :
- d’un **projet personnel à dominante technique** (livrable rapport + lien repo/projet)
- d’une **carte mentale** (compétences + réflexivité + soft skills)
- d’un **site portfolio** (ou PDF) pour présenter l’ensemble.

Référence cahier des charges : [CONSIGNES_P15.md](CONSIGNES_P15.md) (copie versionnée de `tmp_screens/p15.md`).

## Hébergement gratuit (recommandé) : GitHub Pages

Le site est dans [docs/](docs/) (page d’accueil : [docs/index.html](docs/index.html)).

### Activer GitHub Pages

1) Pousser ce repo sur GitHub (si ce n’est pas déjà fait)
2) Ouvrir le repo sur GitHub → **Settings** → **Pages**
3) **Build and deployment** → Source: **Deploy from a branch**
4) Branch: `main` (ou `master`) ; Folder: `/docs`
5) Sauvegarder → GitHub fournit une URL du type `https://<user>.github.io/<repo>/`

### Vérifier en local

- Ouvrir directement [docs/index.html](docs/index.html) dans un navigateur
- Ou lancer un petit serveur (optionnel) :
  - `cd docs`
  - `python -m http.server 8000`

## Où mettre le contenu P15

- Site : [docs/index.html](docs/index.html)
  - Remplacer les `TBD` par des infos **vérifiées**
  - Ajouter les liens (repo, démo, rapport)
- Rapports (brouillons) :
  - P7 : [mes_projets/rapport_P7_puls_events_rag.md](mes_projets/rapport_P7_puls_events_rag.md)
  - P8 : [mes_projets/rapport_P8_credit_scoring.md](mes_projets/rapport_P8_credit_scoring.md)

## Note importante

Le cahier des charges P15 **n’impose pas** une techno (il cite seulement des supports possibles : page GitHub / CMS / support PDF). GitHub Pages + site statique est généralement le plus simple à maintenir.

Note : le dossier `tmp_screens/` est ignoré par Git (voir `.gitignore`), donc les fichiers “screens” ne sont pas destinés à être poussés.
