# claude-memory-template

Modèle de dépôt pour la mémoire partagée de Claude, à utiliser avec le plugin `memoire` de la marketplace [claude-plugins](https://github.com/BadBben/claude-plugins). Un dépôt créé à partir de ce modèle devient la sauvegarde et le point de synchronisation de votre mémoire Claude entre vos appareils (Claude Code sur plusieurs machines, et le chat claude.ai).

Ce dépôt ne contient que des données : des fichiers mémoire au format natif de Claude Code. Aucun script, aucune configuration. Le plugin fait tout le reste.

## Créer votre dépôt

1. Bouton **Use this template** en haut de cette page, puis **Create a new repository**. Choisissez un nom (par exemple `claude-memory`) et la visibilité **Private** : votre mémoire est personnelle.
2. Installez le plugin `memoire` depuis la marketplace, sur claude.ai (Customize > Plugins > Add > Add marketplace, `BadBben/claude-plugins`) ou dans Claude Code (`claude plugin marketplace add BadBben/claude-plugins` puis `claude plugin install memoire@badbben`).
3. Dans Claude Code, dans un dossier de travail, demandez « charge la mémoire du projet X » ou « sauvegarde la mémoire ». Le plugin demande une fois l'URL de votre dépôt (`votre-compte/claude-memory`) et l'enregistre sur l'appareil. Rien d'autre à faire.

Le README du plugin détaille les prérequis (git, identité git, authentification GitHub) et le fonctionnement : [plugins/memoire/README.md](https://github.com/BadBben/claude-plugins/blob/main/plugins/memoire/README.md).

## Structure

```
claude-memory/
  README.md                  ce fichier
  general/                   mémoire générale, hors projet
    MEMORY.md                index : une ligne par entrée
    profil/                  qui vous êtes, comment vous travaillez
    projets/                 une fiche par projet : quoi, état, où vit sa mémoire
    references/              machines, comptes, outils, URLs (jamais de secret)
    apprentissages/          ce qui marche ou pas, décisions d'outillage
  projets/
    <nom>/memory/            copie partagée de la mémoire auto d'un projet
```

- `projets/<nom>/memory/` est créé par le plugin au premier push d'un projet. Vous n'avez rien à y mettre à la main.
- `general/` est votre mémoire hors projet, consultée et enrichie par le skill `generale`. Les quatre dossiers sont là pour guider, vous pouvez en ajouter.
- Les fichiers `.gitkeep` ne servent qu'à garder les dossiers vides dans git ; le plugin les ignore.

## Format des fichiers mémoire

Même format que la mémoire auto de Claude Code, pour que Claude n'ait rien à apprendre : un sujet par fichier, nom en kebab-case, et un en-tête YAML.

```
---
name: slug-du-fichier
description: "une ligne, utilisée pour décider si le fichier est pertinent"
metadata:
  type: user | feedback | project | reference
---

Le contenu.
```

`MEMORY.md` est l'index d'un dossier : une ligne par fichier, `- [Titre](dossier/fichier.md) : accroche`. Claude lit l'index d'abord, puis seulement le fichier utile.

## Ce que le plugin écrit ici

- Des commits titrés `memoire(<projet>): synchro depuis <machine> le <date>`, avec en corps un récap rédigé par la session et la liste des fichiers ajoutés, modifiés ou supprimés. Vous validez chaque commit avant qu'il parte.
- Un `journal.md` par mémoire consolidée : ce que les consolidations ont retiré des fiches, par date, sans rien perdre.
- Jamais le marqueur `.memoire-sync` (il reste local, et il est dans `.gitignore`).

## Règles

- Jamais de secret dans ce dépôt : pas de mot de passe, de jeton, de clé. Le plugin refuse un fichier qui en contient un motif.
- Un push après un pull : si un autre appareil a poussé entre-temps, le push refuse et le pull fusionne.
- Un fichier supprimé, ou beaucoup plus de lignes retirées qu'ajoutées, est signalé avant le commit : une mémoire grossit ou se réorganise, elle ne rétrécit pas sans qu'on le sache.
- Gardez le dépôt privé.
