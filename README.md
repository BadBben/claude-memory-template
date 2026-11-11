# claude-memory-template

Modèle de dépôt pour la mémoire partagée de Claude, à utiliser avec le plugin `memoire` de la marketplace [claude-plugins](https://github.com/BadBben/claude-plugins). Un dépôt créé à partir de ce modèle devient votre mémoire Claude, partagée entre vos appareils (Claude Code sur plusieurs machines, et le chat claude.ai) : chaque appareil en garde un clone, où Claude Code lit et écrit directement, et le plugin synchronise ce clone à la demande.

Ce dépôt ne contient que des données : des fichiers mémoire au format natif de Claude Code. Aucun script. Le plugin fait tout le reste.

## Créer votre dépôt

1. Bouton **Use this template** en haut de cette page, puis **Create a new repository**. Choisissez un nom (par exemple `claude-memory`) et la visibilité **Private** : votre mémoire est personnelle.
2. Installez le plugin `memoire` depuis la marketplace, sur claude.ai (Customize > Plugins > Add > Add marketplace, `BadBben/claude-plugins`) ou dans Claude Code (`claude plugin marketplace add BadBben/claude-plugins` puis `claude plugin install memoire@badbben`).
3. Dans Claude Code, demandez « configure ma mémoire partagée » et donnez l'URL de votre dépôt (`votre-compte/claude-memory`). Le plugin le clone sur l'appareil, dans `~/claude-memory` par défaut, et retient l'URL.
4. Dans le dossier de travail d'un projet, demandez « branche ce projet sur ma mémoire ». La mémoire auto du projet vit alors dans le clone, et le dossier de travail ne bouge pas.
5. Au quotidien : « récupère la mémoire » en arrivant sur un appareil, « sauvegarde la mémoire » avant d'en changer.

Le README du plugin détaille les prérequis (git, identité git, authentification GitHub) et le fonctionnement : [plugins/memoire/README.md](https://github.com/BadBben/claude-plugins/blob/main/plugins/memoire/README.md).

## Structure

```
claude-memory/
  README.md                  ce fichier
  .gitattributes             fins de ligne identiques sur tous les appareils
  general/                   mémoire générale, hors projet
    MEMORY.md                index : une ligne par entrée
    profil/                  qui vous êtes, comment vous travaillez
    projets/                 une fiche par projet : quoi, état, où vit sa mémoire
    references/              machines, comptes, outils, URLs (jamais de secret)
    apprentissages/          ce qui marche ou pas, décisions d'outillage
  projets/
    <nom>/memory/            la mémoire auto d'un projet
```

- `projets/<nom>/memory/` est créé par le plugin quand vous branchez un projet. Vous n'avez rien à y mettre à la main.
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

- Des commits titrés `memoire(<ce qui change>): synchro depuis <machine> le <date>`, par exemple `memoire(mon-projet, general)`, avec en corps un récap rédigé par la session et la liste des fichiers ajoutés, modifiés ou supprimés. Vous validez chaque commit avant qu'il parte.
- Un commit ne contient que le projet de la session et la partie transverse (la mémoire générale et les fichiers de la racine). Le travail en cours des autres projets reste sur l'appareil jusqu'à ce que vous le poussiez depuis leur session.
- Un `journal.md` par mémoire consolidée : ce que les consolidations ont retiré des fiches, par date, sans rien perdre.

## Règles

- Jamais de secret dans ce dépôt : pas de mot de passe, de jeton, de clé. Le plugin refuse un fichier qui en contient un motif.
- Un push après un pull : si un autre appareil a poussé entre-temps, le push refuse et le pull fusionne.
- Un fichier supprimé, ou beaucoup plus de lignes retirées qu'ajoutées, est signalé avant le commit : une mémoire grossit ou se réorganise, elle ne rétrécit pas sans qu'on le sache.
- Gardez le clone hors d'un dossier synchronisé par un service de fichiers (OneDrive, Dropbox, iCloud) : deux synchronisations dans le même dossier finissent par abîmer le dépôt.
- Gardez le dépôt privé.
