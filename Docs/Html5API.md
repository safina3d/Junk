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

