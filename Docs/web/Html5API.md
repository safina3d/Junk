# HTML5 API

	1. Audio Video API
	2. Canvas
	3. Drag & Drop
	4. Geolocalisation
	5. Web deconnecté


## 1. Audio Video API

```
<video src="video.url" controls width="500" poster="image.png" loop > 
	ici un lecteur flash ou un message d'erreur 
</video> 
<audio src="audio.url" controls loop > 
	un autre message d'erreur 
</audio>
```

#### Attributs

###### controls 
> Permet d'afficher la barre de controle. l'apparence de cette derniere depend du navigateur.

###### width, height
> Permet de definir la hauteur et largeur du lecteur en px (utiliser le css pour changer d'unité). Si seul un des deux attributs est defini, le ratio est respecté. Si aucun n'est defini les dimensions sont celle de la video, ou de l'attribut poster, sinon 300x300.

###### poster
> Permet d'afficher une image qui representative de la video

###### loop, autoplay
> Respectivement Lecture en boucle, et demarrage automatique.

###### preload
> Prend 3 valeurs :
> *auto* : c'est le navigateur qui decide de charger ou pas la video
> *none* : pas de chargement automatique
> *metadata* : permet d’allerchercher la première image et les dimensions d’une vidéo, et d’afﬁcher la durée totale de la video


#### Media Queries 
Une video peut avoir plusieurs resolutions et c'est le navigateur qui va selectionner celle qui est la plus adaptée à la connexion de l'utilisateur. comment definir plusieurs sources pour la meme video ?

Pour plus d'informations : https://developer.mozilla.org/fr/docs/Web/CSS/Requ%C3%AAtes_m%C3%A9dia/Utiliser_les_Media_queries
###### La balise <source> 
```
<video controls> 
	<source src="hd_video"></source>
	<source src="md_video"></source> 
	<source src="sd_video"></source> 
</video>
```

## 6.IFrame

### Atrributs

```
src="page.html"    // URL de la page à afficher
width="200"        // Largeur de l'iframe en px
height="300"       // Hauteur de l'iframe en px
frameborder="1"    // Valeure 1 pour afficher une bordure autour de l'iframe sinon 0 pour l'enlever
marginheight="20"  // Permet d'ajouter un espace en haut et bas de l'iframe par rapport à son container
marginwidth="10"   // Permet d'ajouter un espace à droite et à gaucge de l'iframe par rapport à son container
name="muiframe"    // Permet d'attribuer un nom à l'iframe, utilisé lorsqu'on souhaite ouvrir des liens (via l'attribut target) dans une iframe bien precise
srcdoc="<h1>hello</h1>" // Permet d'afficher le rendu du code html dans l'iframe
longdesc=""
scrolling="auto/yes/no"        // Autoriser ou non l'utilisation des scrollbar dans l'iframe
```

#### L'attribut sandbox
Cet attribut permet d'appliquer certaines restrictions au contenu de l'iframe. 
```
<iframe src="test.html" sandbox></iframe>
```
Si on ajoute cet attribut sans lui preciser de valeur, alors toutes les restrictions sont appliquées :
- Les scripts sont desactivés
- Les formulaires ne peuvent etre envoyés
- Le contenu ne peut utiliser les plugins
- Le contenu est concideré comme une seule origine et ne peut avoir acces aux données stockés dans les cookies
- Ne peut ouvrir de nouvelles fenetres
- Ne peut naviguer son top level parent
- Les actions automatiques ex video autoplay sont bloquées.
- Les APIs sont bloquées aussi

Si on souhaite activer certaines fonctionnalités, il suffit d'attribuer une ou plusieurs valeurs à l'attribut sandbox. les valeurs possibles sont :
- allow-forms
- allow-pointer-lock
- allow-popups
- allow-same-origin
- allow-scripts
- allow-top-navigation

##### allow-same-origin
la politique du **Same origin** implique qu'une page peut acceder à une autre page seulement si les deux pages font parties de la meme origine. l'Origine est determiné à partir de l'URI, le port et le nom du domaine. Le fait d'ajouter dont cette valeur `sandbox="allow-same-origin"`, permet d'acceder à des pages de la meme origine.

##### allow-forms
Autorise la submission des formulaires.

##### allow-top-navigation
La top navigation nous permet d'ouvrir les liens dans la page principale (qui contient l'iframe)
