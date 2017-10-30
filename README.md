# Blog

## Hugo

```bash
# Créer un nouveau post
hugo new post/my-first-post.md
```

## Firebase

```bash
# Installation de firebase cli
npm install -g firebase-tools

# Identification firebase
firebase login

# Création de l'alias hosting
firebase use revue-de-code

# Déploiement sur firebase
hugo && firebase deploy
```

## Thème

```bash
# Installation du thème hugo-academic en submodule
git submodule add -b master git@github.com:fleporcq/hugo-academic.git themes/academic
```