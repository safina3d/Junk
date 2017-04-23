# WebWorker

```
// Main script
var worker = new Worker('worker.js');
worker.postMessage('hello');
```

```
// Worker script
this.addEventListener('message', function (e) {
    try {
        var r = JSON.parse(e.data);
        console.log(r);
    }catch (err){
        console.log('Message: %s', e.data);
    }
});
```

```
//Utiliser d'autres scripts
importScripts('cheminRelatif/script1.js', 'autreChemin/script2.js');
```

### Les types
#### 1. Dedicated Worker
Peut etre géneré en utilisant `new Worker()`. Il est dedié à un contexte ou page unique.

#### 2. Shared Worker
Fonctionne comme un singleton. Il peut etre partagé entre plusieurs contextes (exemple: entre deux pages, deux onglets, une page et une iframe, etc...). Ce dernier est executé dans le contexte du navigateur

### Cycle de vie
#### 1. Dedicated Worker
1. Creation via l'instaruction `new Worker()`
2. Desctruction via :
  - Depuis le worker lui meme en utilisant la methode `close()`
  - Depuis l'exterieur en appelant `worker.terminate()`

#### 2. Shared Worker
1. Creation via l'instaruction `new SharedWorker()`
2. Connexion au worker en ecoutant l'evenement `connect` qui est declanché lorsqu'un contexte d'execution se connecte la premiere fois au SharedWorker. Pour acceder au log il faut passer par `chrome://inspect` et aller dans l'onglet *SharedWorkers*
3. Demarrage via linstruction `port.start()`
4. Desctruction

```
// Main script
var sheredWorker = new SharedWorker('worker.js');
worker.postMessage('hello');
```

```
// Worker script
this.addEventListener('message', function (e) {
    try {
        var r = JSON.parse(e.data);
        console.log(r);
    }catch (err){
        console.log('Message: %s', e.data);
    }
});
```

RPC: Remote Procedure Call
