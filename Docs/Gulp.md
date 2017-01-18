# Gulp

#### gulp.task
Permet de creer une tache via la methode `task` :

```
gulp.task('mytask', [], function(){
	return gulp
		.src('./src/**/*.js');
});
```
- Le premier argument (ici 'myTask'), permet d'enregistrer la tache avec ce nom.
- L'agrument des dependances est optionnel, On peut donc ecrire `gulp.task('myTask', function(){....});`
- Les taches presentent en tant que dependances sont lancées en paralleles et non pas de façon sequentielle.


#### gulp.src
Correspond à la partie en amont. Il permet de definir les fichiers sources sur lequels on souhaite apporter des modifications.

```
gulp.src(glob [, options])
```
- **glob** : coresspond à chemin vers un ou plusieurs fichiers en utilisant un pattern.
- **options** : ici on peut preciser un objet contenant des options qu'on souhaite passer comme :
  - base : Permet de definir une base url ex : `{base: './src/'}`

#### gulp.dest
Permet de speciifier ou ecrire les fichiers de sorties.

#### gulp.watch
Permet de surveiller des fichiers et d'executer une fonction ou une tache.
```
gulp.watch(glob [,options], tasks);
```
- **glob** : coresspond à chemin vers un ou plusieurs fichiers en utilisant un pattern.
- **options** : ici on peut preciser un objet contenant des options qu'on souhaite passer 
- **tasks** : Tableau contenant la liste des nom de taches qu'on souhaite executer

exemple :
```
gulp.task('myWatchTask', function(){
	gulp.watch('./src/**/*.js', ['jshint', 'jsdoc']);

	// Executer une fonction
	gulp.watch('./src/**/*.js', function(event){
		console.log('Exec event ', event.type, event.path);
	});	
});
```


### 1. Installation
- Installer node
- Installer gulp de façon globale : `npm install -g gulp`
- Pour voir la liste des package installés de façon globale on peut utiliser la commande `npm list -g --depth=0`

Puis dans le Projet :
- Installer gulp localement* : `npm install gulp --save-dev`
- Creer le fichier `gulpfile.js` et importer le module gulp via l'instruction `var gulp = require('gulp');`

*L'option `--save-dev` permet de dire que la dependance ou package sera utilisé uniquement pour le developpement (devDependencies), tandis que `--save` permet de dire que cette dependance fait partie de l'execution (Dependencies).

#### 1.2 Executer notre premiere tache

```
var gulp = require('gulp');


gulp.task('first', function(){
    console.log('Hello world');
});
```
Puis dans l'interpreteur de commandes : `gulp first` pour executer notre tache *first*.


### 2. Quelques plugins
- gulp-plumber
- gulp-inject : Permet d'ajouter dynamiquement les liens vers les fichiers css et js dans la page html
- gulp-util : Qui contient un systeme de log
- gulp-print : Permet d'afficher les fichiers que le processus de gulp est entrain de modifier.
- gulp-if: Permet d'ajouter un systeme de condition et d'executer telle ou telle fonction.
- gulp-load-plugins : Permet de charger dynamiquement les plugins afin d'eviter plusieurs lignes au debut du gulpfile uniquement pour charger les plugins.