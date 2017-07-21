# Installation de crodovaCli
```npm install -g cordova```

# Creation d'un project Cordova
```cordova create projectDirectoryName com.test.appid AppName```

# Structure du projet
```
testApp
|_ hooks : Permet de modifier le processus de CLI build avec des instructions personnalisées
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
```
<meta name="viewport" content="user-scalable=no, initial-scale=1, maximum-scale=1, minimum-scale=1, width=device-width">
```


# Build
`cordova build` : Permet de lancer un build pour toutes les plateformes installé, sinon specifier son nom. Cette commande est un raccourcis pour deux autres commandes qui sont:
  - `cordova prepare` : Permet de copier le repertoire www dans la plateforme choisie
  - `cordova compile` : Permet de lancer le process de compilation

`cordova run android` = build + emulate

`cordova build --realease android` : Permet de generer une release de notre app prete à etre envoyée sur le store. Mais avant cela, il faut signer notre apk avec la commande

```
Pour generer notre clé privée
keytool -genkey -v -keystore my-release-key.keystore -alias alias_name -keyalg RSA -keysize 2048 -validity 10000

Pour signer notre apk
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore my-release-key.keystore myApp-unsigned.apk alias_name

Optimiser l'APK
zipalign -v 4 myApp-unsigned.apk  myApp.apk
```


`android avd` pour creer un emulateur android

# Ajouter un plugin
http://cordova.apache.org/plugins/

`cordova plugin add nom-du-plugin`

# Configuration de l'application via Config.xml

Le fichier de configuration de cordova est basé sur les specs w3c widget

```
id="com.test.myApp" // identifiant unique de l'application

// Preferences globales
<preference name="DisallowOverscroll" value="true" /> #Activer/Desactiver le feedback visuel lors d'un scroll en fin de page
<preference name="Fullscreen" value="false" /> #Activer/Desactiver l'affichage en plein ecran
<preference name="Orientation" value="portrait" /> #Definir l'orientation de l'ecran

// Preferences Android
<preference name="android-minSdkVersion" value="18" /> #Version minimale android
<preference name="android-installLocation" value="auto" /> #Installation sur le tel ou un external storage

// Definition des icones
# Valeurs possibles : ldpi/mdpi/hdpi/xhdpi/xxhdpi/xxxhdpi
<platfrom name="android">
  <icon src="path/drawable-ldpi-icon.png" qualifier="ldpi" />
  <icon src="path/drawable-mdpi-icon.png" qualifier="mdpi" />
  <icon src="path/drawable-hdpi-icon.png" qualifier="hdpi" />
</platform>

// Definition des splashscreens
# idem que les icones mais en utilisant les tags splash au lieu de icon
# Remarque: le splash tag peut definir aussi l'orientation de l'ecran
<splash src="path/drawable-hdpi-splashscreen.png" qualifier="hdpi" />

// Definition des ressources externes
<access origin="*" subdomains="true" /> # acceder à n'importe quelle ressource

# Autoriser l'application à acceder à certains services du telephone ex: sms, geolocalisation, etc...
<allow-intent href="..." />
<platform name="android">
  <allow-intent href="..." />
</platform>

```

### Crosswalk

```
  cordova plugin add cordova-plugin-crosswalk-webview
```
Après lancement d'un build, on obtient :
  - Deux Apk pour les architectures ARM et Intel based arch (x86)
  - Chaque Apk à une taille de ~20mo

Pour verifier que c'est bien crosswalk est bien utilisé dans l'application. Allez dabs `chrome://inspect` et dans la console executé la commande `navigator.userAgent`. Si crossWalk est dans la chaine affichée alors c'est OK.
