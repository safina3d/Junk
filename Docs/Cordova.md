# Installation de crodovaCLI
```npm install -g cordova```

# Creation d'un project Cordova
```cordova create projectName com.test.appid AppName```

# Structure du projet
```
testApp
|_ hooks : Permet de modifier le process de CLI build avec des instructions custom
|_ platforms : C'est la ou les builds (pour chaque plateforme) seront créés
|_ plugins: Emplacement des plugins cordova
|_ www: Emplacement de l'application web
```

# Ajouter une plateforme
```cordova platform add android```

# Supprimer une plateforme
```cordova platform remove android```

# Les meta tags
Meta Viewport tag : 
```<meta name="viewport" content="user-scalable=no, initial-scale=1, maximum-scale=1, minimum-scale=1, width=device-width">```


# Build
`cordova build` : Permet de lancer un build pour toutes les plateformes installé, sinon specifier son nom. Cette commande est un raccourcis pour deux autres commandes qui sont :
  - `cordova prepare` : Permet de copier le repertoire www dans la plateforme choisie
  - `cordova compile` : Permet de lancer le process de compilation

`cordova run android` = build + emulate


`android avd` pour creer un emulateur android