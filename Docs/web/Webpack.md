#Webpack


### Installation
```npm install -g webpack```
```npm install webpack --save-dev```

### Compilation
```webpack my_js_file.js bundle.js```
Si webpack n'est pas installé de façon globale, on peut utiliser la version locale
```./node_modules/.bin/webpack my_js_file.js bundle.js```

On peut aussi activer aussi l'option watch via la commande `webpack -w`
### Configuration
Il faut creer le fichier **webpack.config.js**
```
module.exports = {
    entry:  './js/main.js',
    output: {
        filename: './bundle.js'
    }
};
```
puis executer la commande `webpack`.


### Webpack Loaders
```npm install babel-loader babel-core --save-dev```