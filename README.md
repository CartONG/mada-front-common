# Mada front common

Ce repo regroupe les dépendances communes des projets :
- Onusida : https://github.com/CartONG/mada-onusida-front
- ASM (en cours)

## Lancer un projet en local

Voir https://github.com/CartONG/mada-onusida-front#lancer-le-projet-en-local

## Gestion des dépendances

Chaque projet utilise mada-front-common comme une dépendance NPM (gestion de packet node/io.js).

Structure de dossiers type :
- `mada-onusida-front`
    - (fichiers du projet...)
    - `node_modules` crée et géré par NPM
        - `bootstrap` dépendance externe, installée par `npm install`
        - `mada-front-common` : cette dépendance
        - ...

### Je veux mettre à jour mada-front-common dans un projet

```
rm -rf node_modules/mada-front-common`
npm install
```

Réinstalle mada-front-common comme dépendance NPM

### Je travaille sur un projet et sur mada-front-common en même temps

```
cd mada-front-common
npm link
cd ../mada-onusida-front
npm link mada-front-common
```

Crée un lien symbolique entre les deux repos en local (plutôt que d'utiliser la dépendance sur le repo github)


## Workflow/structure

Gulp est configuré pour :
- compiler le template principal de `src` vers `dist`, en injectant les traductions pour chaque locale (situées dans `src/translations`)
- concaténer les dépendances JS dans un seul fichier
- concaténer les dépendances CSS dans un seul fichier
- copier les assets statiques nécéssaires dans `dist`,

**Le workflow est volontairement simpliste (pas de bundler type Browserify/Webpack, etc), pour laisser le projet accessible techniquement.**

Il est primordial d'utiliser un linter pour modifier le fichier JS (app.js). Eslint est configuré par défaut sur ce repo et les repos des projets.



## Communication avec le back

Le front attend une liste complète des entités au format GeoJSON.

Les champs à afficher/à filtrer sont tous définis dans le fichier https://github.com/CartONG/mada-onusida-front/blob/gh-pages/src/index.template.html

- champs à afficher dans le template (popup et modales) :
  - https://github.com/CartONG/mada-onusida-front/blob/6e1474644dbf9619ab20b06435cad04b40c5aaca/src/index.template.html#L375


- champs utilisés pour le filtrage situés dans des data-attributes :
  - https://github.com/CartONG/mada-onusida-front/blob/6e1474644dbf9619ab20b06435cad04b40c5aaca/src/index.template.html#L140
  - https://github.com/CartONG/mada-onusida-front/blob/6e1474644dbf9619ab20b06435cad04b40c5aaca/src/index.template.html#L143


Lors du filtrage, si un champ n'est pas présent dans l'entité du GeoJSON, le champ filtré est ignoré (l'entité est affichée).

Structure de test utilisée, voir https://raw.githubusercontent.com/CartONG/mada-onusida-front/gh-pages/src/assets/testData.json

```
"properties": {
  "localisation": "Ambavahala",
  "nom_ong": "ONG 1",
  "titre": "Action 1",
  "description": "...",
  "type": [
    "plaidoyer","preservatifs"
  ],
  "population": [
    "vih","perdus_de_vue"
  ],
  "date_debut": "2011-01-01",
  "date_fin": "2015-01-01"
}
```

### Valeurs utilisées pour le filtrage :

`type` et `population` peuvent contenir zéro, une ou plusieurs valeurs, ou être totalement omis.

`date_debut` et `date_fin` peuvent être chacun omis (si date_fin est omis, le filtrage court de date_debut jusqu'à aujourd'hui) 

## Traductions

Le template principal est compilé par gulp dans toutes les locales de `src` vers `dist`.

Les traductions sont situées pour chaque locale dans un fichier JSON dans `src/translations`.

La locale par défaut est fr-FR et compile vers index.html, les autres vers index.[locale].html, index.[mg-MG].html par exemple.

Si une clé n'est pas traduite, la version fr-FR est utilisée par défaut.
