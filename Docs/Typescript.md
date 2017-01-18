# TypeScript


## 1. Installation
Installer le compilateur typescript
```
npm install -g typescript
```
Pour compiler manuellement les fichier .ts
```
tsc nom_du_fichier.ts
```
On peut ajouter l'option *-w* pour faire un watch et compiler automatiquement les fichiers ts.

On peut aussi definir un fichier de configuration pour le compilateur *tsconfig.json*, exemple 
```
{
  "compilerOptions" : {
    "target" : "es5"
  }
}
```


### Nouveautés
- Parametres par defaut pour les fonctions
```
function test(a=1, b=2){
	
}
test();
```
- Template strings : Permet d'injecter des variables ou des expressions dans une string
```
var data = {
	name : "toto",
	age : 99
};
var display = "Hello, ${data.name}"; 
```
- Declaration de variables:
le mot clé *let* permet de definir des variables locales
le mot clé *const* permet de definir des constantes

- La boucle for...of permet de recuperer les valeurs d'une collection directement sans passer par les indexes.
```
var data = ["a", "b", "c"];
for(var letter of data){
	console.log(letter);
}
```
- Lambda expression
- Destructuring
```
// Decomposition d'un tableau
var data = ['toto', 'titi', 25];
var [nom, prenom, age] = data; // creation de trois variables avec les valeurs contenues dans le tableau


//Permutation
var a = 1;
var b = 3;
[a,b] = [b,a];

```
- Spread operator : representé par 3 points, il permet :
Dans le cas d'une fonction d'avoir un nombre variable d'arguments inconnu d'avance
Dans le cas des tableaux d'effectuer des operations comme inserer le contenu d'un tableau dans un autre ou la concatenation

- Computed properties : 


### Specifier le type
```
var toto: string = ....
function calc(x: number, y:number) : number {...}
function calc(x: (number|string), y:(number|any[]) : number {...}
```

- Surcharger une methode
```

function totalLength(x:string, y:string): number
function totalLength(x:any[], y:any[]): number
function totalLength(x:(string|any[]), y:(string|any[])): number{...}
```

### Custom types
Javascript permet de creer des types custom avec 3 solutions
- Les interfaces
- Les classes
- Les enums

#### Interface
```
interface Animal {
	name : string;
	age : number;
	color? : string; // le ? signifie que cette propriete est optionnelle
	eat(): void;
	(message:string) : void; // correspond a une fonction anonyme 
}

var dog: Animal = {};
```

#### Enums
```
enum State {
	OK = 1, 
	READY = 2,
	DONE = 3
}

var my = State.DONE;
```

#### Classes
```
class Animal {
	static id: number = 0;
	name : string;
	age: number;
	constructor() {
		this.name = "cat";
		this.age = 2;
	}


	clalculateAge(){
		return this.age;
	}

}
```

