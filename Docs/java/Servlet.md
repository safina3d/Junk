# Servlet

Avant l'arrivée des Servlets et autres alternatives, on utilisiait CGI (Common Gateway Interface) pour avoir des contenus dynamiques.

#### Limitations de CGI:

- Le serveur web doit lancer lui meme les scripts CGI pour chaque requete ce qui est tres lourd et couteux.
- Les scripts CGI ne peuvent garder une connection vers la base de données mais doivent creer de nouvelles connexion à chaque requete.

### 1.1 Servlet ?
Une Servlet est une class Java utilisée afin d'etendre les capabilitées du serveur qui heberge l'application qui repond au requetes envoyées par le client.

Avantages des Servlets par rapport au CGI ?
- Performances : les servlets sont executées dans le meme processus que le serveur et n'ont donc pas besoin de creer de nouveaux processus separés.

- Multi-Plateformes
- Securité : Le java securité manager assure un certains nombre de restrictions afin de proteger les ressources sur le serveur.
- Librairie : Les Servlets peuvent utilisées toutes les classes Java disponibles.


Les Servlet n'ont pas de methode main. Ils donc aidé par une autre entité qui :
- Permet d'instancier les Servlet
- Permet de recuperer les requetes et renvoyer les reponses.
- Permet de Gerer la vie, la mort et les ressources des Servlets.

Cette entité est ce qu'on appele un Conteneur de Servlet ou Container ou (ou meme Servlet engine) par exemple : Tomcat, Jetty, JWS et Resin

### 1.2 Fonctionnement :

- Le Servlet container recoie une requete du client et remarque qu'elle est destinée à une servlet.
- Le Servlet Container crée ensuite deux objets qui sont request (HttpServletRequest) et response (HttpServletResponse).
- Le Servlet Container selectionne la bonne servlet en se basant sur l'url dans la requete et creer un Thread pour cette requete qui lui passe les deux objets request et response.

- Le servlet container appele ensuite la methode service de la Servlet en prenat en compte le type de requete reçue (GET, POST, etc...) qui appelra les methodes correspondantes doGet, doPost, etc... Ces derniere genere la reponse en la stockant dans l'objet response.

- Le Servlet Container (qui dispose deja de references vers les deux objets request et response) recupere le contenu de la reponse et renvoie une HttpResponse au client.

### 1.3 Cycle de vie d'une Servlet

#### La classe Servlet :

Les Servlet repose sur une interface avec les methodes suivantes : 

- destroy() : cette methode permet de nettoyer les ressources utilisées par la servlet et assure que toute persistance et synchronoisée avec l'etat de la Servlet en cours.
- getServletConfig() : Permet de recuperer un objet contenant la configuration de la Servlet.
- getServletInfo() : Permet de recuperer une chaine qui decrit la Servlet tel que l'auteur, la version, etc..
- init() : Permet de d'initialiser la Servlet. elle est executée une seule fois avant le traitement de la requete.
- service() : Permet de gerer la requete et la reponse.

Toute implementation repose donc soit sur l'interface, ou etend une autre classe qui l'implemente au prealable tel que GenericServlet ou HttpServlet.

#### Cycle de vie
Le cycle de vie d'une Servlet designe tout le processus suivi depuis sa creation jusqu'a sa creation. Ce cycle est controllé par le Servlet Container. Le cycle de vie est composé par les etapes suivantes :

- Chargement de la classe Servlet
- Creation d'une instance de cette classe (singleton)
- Appel de la methode init() 
- Appel de la methode service()
- Appel de la methode destroy()

Les 3 premiere etapes sont executée seulement une seule fois lorsque la Servlet est chargée.
Une Servlet n'est chargée que lorsqu'une requete lui est destinée est reçue. Ce qui permet de charger uniquement les Servlets utilisées. Cependant, on peut forcer le Container a charger des Servlet au demarrage.

La methode service() est ensuite appelée plusieurs fois (une fois pour chaque requete reçue).

La methode destory() est appelée lorsque le Container n'a plus besoin de cette derniere et essaye de la decharger de la memoire.


#### Pourquoi utiliser HttpServlet ?

la class GenereicServlet est indepedante de tout protocol. Elle peut donc gerer toute sorte de protocol tel que Http, Smtp, ftp, etc... Cependant, les GenericServlet ne permettent pas de gerer les données session.
Le HttpServlet a été concu afin de gerer le protocol HTTP et supporte tous les verbes Http tel que : Get, Post, Put, etc... 

Afin de traviller de pouvoir l'utiliser, la classe doit etendre la classe HttpServlet
```
import javax.servlet.http.HttpServlet;
...
public class MyServlet extends HttpServlet {
	.....
}
...
```

Chaque methode redefinie ( doGet, doPost, etc..) dispose de deux arguments qui sont request (HttpServletRequest), response (HttpServletResponse) et peux declancher deux exceptions qui sont ServletException, IOException.

L'argument request de type HttpServletRequest dispose de plusieurs methodes qui sont :
- getParameter() : Permet de recuperer la valeur d'un parametre depuis la requete.
- getParameterValues() : Permet de recuperer toutes les valeurs d'un parametre sous forme d'un tableau de string.
- getParameterMap() : Permet de recuperer tous  les parametres sous forme de java.util.Map
- getParameterNames() : Permet de recuperer une enumeration contenant tous les noms des parametres.


Differences entre Get et Post :

| GET | POST |
|-----|------|
| Refresh de la page sans effet | Les données sont envoyées à nouveau au serveur. |
| Les requetes peuvent etre bookmarquée | Les requetes ne peuvent pas etre bookmarquées. |
| Les requetes peuvent etre mises en cache | Les requetes ne peuvent pas etre mises en cache |
| Les parametres et leurs valeurs sont retenus par l'historique du navigateur | Les parametres ne sont pas retenus par le navigateur |
| Taille limitée à 2048 caracteres | Pas de restriction au niveau de la taille |
| Supporte uniquement les caracteres ASCII | Pas*
 de restriction sur le type des données envoyées. |
| Moins securisé | Un peu plus securisé |


### 2 Gestion des requetes et reponses Http
#### Request headers

Le client envoie au serveur plusieurs informations utiles dans la partie Header de la requete. On peut voir ces information via quelques methodes comme :

- getHeader(String name) : Permet de recuperer la valeur d'une propriete contenu dans le header.
- getHeaderNames() : permet de recuperer une enumeration qui contient la liste de toutes les proprietes du Header.
- getAuthType() :  Permet de recuperer le schema d'authentification utilisée pour proteger la servlet
- getRemoteUser() : Permet de recuperer le nom de l'utilisateur qui a envoyé la requete si ce dernier s'est authentifié
- getRemoteAddr() : Permet de recuperer l'adresse ip du client
- getContentLength () : Permet de recuperer la valeur la taille du header
- getContentType() : Permet de recuperer le type du contenu
- getDateHeader(String name)
- getIntHeader(String name) 

Afin de reduire la taille des reponses, on peut utiliser le systeme de compression GZIP. pour cela deux solutions :
- Dans la configuration du serveur, activer l'option *compression* comme ceci : `<Connector compression="on" ...>`

- Ou dans le corps de reponse de la Servlet comme ceci : 
```
public void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	response.setContentType("text/html");		
	PrintWriter out;
		
	if(isGzipSupported(request) && !isGzipDisabled(request) ){
		out = getGzipWriter(response);
		response.setHeader("Content-Encoding", "gzip");
	}else{
		out = response.getWriter();
	}

	out.println(".........");
	out.close();
}

public static boolean isGzipSupported(HttpServletRequest request){
	String encodings = request.getHeader("Accept-Encoding");
	return (encodings != null ) &&(encodings.contains("gzip"));
}
		
public static boolean isGzipDisabled(HttpServletRequest request){
	String flag = request.getParameter("disableGzip");
	return (( flag!=null) && (!flag.equalsIgnoreCase("false")));
} 
	
public static PrintWriter getGzipWriter(HttpServletResponse response) throws IOException {
	return (new PrintWriter(new GZIPOutputStream(response.getOutputStream())));
}
```

#### Response headers
Les headers de la reponse sont tout aussi importants. c'est ici qu'on peut donner des precisions sur la nature de la reponse. Pour cela on peut utiliser des methodes comme :

- setHeader(String name, String value) : Permet d'ajouter une propriete avec sa valeur
- setContentType(String type) : Permet de definir le type de reponse
- containsHeader(String name) : Permet de verifier si le Header dispose la propriete ciblée

##### Status Code
Les status code permettent d'identifier la cause d'un probleme. On peut definir ces codes via les methodes :

- setStatus(int code) : Permet de definir le code de retour
- sendError(int code, String msg) : Permet de definir un code d'erreur ainsi qu'un message
- sendRedirect(String url) : demander au client a faire une nouvelle requete vers l'url specifiée.


### 3. Les Filtres

Les filtres ont été introduits depuis la version 2.3 des Servlets. Un equivalent existait deja avant mais de façon non standardisée qui constiste en l'utrilisation du *Servlet Chaining*. Concretement, un filtre est code qui est executé avant ou après avoir generer une ressource, il permet en general de manipuler la requete ou la reponse du serveur. Niveau implementation, les filtres utilises une implementation du pattern **Decorator**.

**Pourquoi utiliser des filtres ?**

- Enregistrer des informations sur les requetes reçus ex: adresses IP
- Faire des conversions, manipulations ou compression de données
- Encryption et decryption
- Validation des données
- Authentifications et Autorisations
- Audit d'acces à des données sensibles
- Systeme d'email

#### 3.1 Cycle de vie des Filtres 

Le cycle de vie des filtres est tres similaire à celui des Servlets. Le Servlet container :

- Charge la classe du filtre
- Crée une instance du filtre
- Appele la methode init() du filtre qui sera executée une seule fois
- Appele la methode doFilter() qui est la methode principale du filtre à chaque requete ou reponse reçues
- Appele la methode destroy() lorsque filtre n'est plus utilisé

Niveau implementation : 
```
import javax.servlet.Filter;

public class MyFilter implements Filter {
	...
	public void init(FilterConfig fc) throws ServletException {
		....
	}

	public void doFilter(ServletRequest request, ServletResponse, FilterChain chain) throws IOException, ServletException {
		....
	}

	protected void destroy(){
		...
	}	
}

```

#### 3.2 Mapping du filtre dans web.xml

Une fois qu'on a definie notre filtre, il faut l'associer à une ou plusieurs servlets dans le deplyement descriptor file (web.xml) selon le formalisme suivant :
```
<filter>
	<filter-name>NomDuFiltre</filter-name>
	<filter-class>FilterClassName</filter-class>
	<init-param>
		<param-name>test-param</param-name>
		<param-value>3</param-value>
	</init-param>

	<filter-mapping>
		<filter-name>NomDuFiltre</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>
</filter>
```

### 4. Gestion des exceptions
Chaque fois qu'une Servlet declanche une exception, le Servlet Container recherche dans la configuration de l'application (web.xml) pour la traiter. Et c'est la balise `<error-page>` qui permet de gerer cela comme ceci : 

```
<error-page>
	<error-code>404</error-code>
	<location>/My404ErrorServlet</location>
</error-page>
```

Ceci dit, si on souhaite traiter le cas d'une exception on peut utiliser l'ecriture suivante
```
<error-page>
	<exception-type>javax.servlet.ServletException</exception-type>
	<location>/MyErrorServlet</location>
</error-page>
```

La servlet qui s'occupe de la gestion d'exception est definie comme n'importe quelle autre Servlet et peut aussi etre affecter à plusieurs exceptions avec des types ou code differents.
Cette derniere peut acceder à certianes informations lui permettant d'avoir des details sur la nature de l'erreur, par exemple : 

- Javax.servlet.error.status_code : Cet attribut permet de recuperer le code d'erreur sous forme d'un nombre entier.
- Javex.servlet.error.exception_type : Cet attribut permet de recuperer le type de l'exception
- Javax.servlet.error.message : Permet de recuperer le message d'erreur
- Javax.servlet.error.request_uri : Permet de recuperer des informations sur l'url qui a fait appele à la Servlet.
- Javax.servlet.error.exception : Permet de recuperer des informations sur l'exception
- Javax.servlet.error.servlet_name : Permet de recuperer le nom de la Servlet.


### 5. Session Data

L'Http est un protocol dit "stateless", ceci signifie que le serveur ne maintient aucune information sur le client et ne peut donc le reconnaitre meme après deux requetes successives.
Plusieurs techniques peuvent etre utilisées pour combler ce probleme.

#### 5.1 Les champs cachés

Dans un formulaire Html on peut ajouter des inputs de type hidden.

Avantages : 
- Supporter par tous les navigateurs

Inconvenients :
- Implementation complexe
- Devient obligatoire pour etre transmit à la servlet suivante.

#### 5.2 Reecriture d'URL

La reecriture d'url consiste en l'ajout d'informations additionnelles à la fin de l'url, notamment via l'utilisation de la balise html <a>.
Avantages : 
- Supporter par tous les navigateurs
- Ne depends pas de l'activation de certaines fonctionnalités

Inconvenients :
- Fonctionne uniquement avec des liens
- On peut transmettre uniquement des informations de type texte
- Implementation complexe, car si on souhaite ajouter ou meodifier des données il faut le faire pour tous les liens existants avec un risque d'erreur ou d'oublie.

#### 5.3 Cookies

Les cookies sont utilisées pour stocker des informations generées depuis le serveur chez le client. On peut distinguer deux types de cookies :
- Les cookies basés sur la proporiete "expires" : representés par les cookies de sessions ou temporaires, ces derniers restent existant tant que l'utilisateur n'a pas fermé le navigateur.
- Les cookies permanants ou persistants qui reste meme après fermeture du navigateur et jusqu'a arriver à leur date d'expiration.

##### Fonctionnement
Lorsque le client envoie sa premiere requete au serveur, ce dernier traite sa demande et lui envoie en plus un cookie dans le Header de la reponse qui sera stocké sur le poste client sous forme d'un fichier. Pour les requetes suivantes, le client renvoie le contenu du cookie dans le Header de la requete.


##### Creation
Coté programmation, il faut 

- Creer un objet en utilisant la classe Cookie qui existe dans le package javax.servlet.http via la commande 
```
Cookie monCookie = new Cookie("key", "value");
```

- Il faut ensuite definir sa durée de vie comme ceci `monCookie.setMaxAge(3000);`. Si la valeur passée est :
 - Egale a zero, alors le cookie est supprimé immediatement
 - Superieur a zero, alors le cookie est concideré persistant et ne sera supprimé qu'une fois sa date limite atteinte
 - Inferieur à zero, alors le cookie est concideré comme temporaire et sera supprimé à la fermeture du navigateur

- Il ne reste plus qu'a ajouter le cookie à la reponse comme ceci
```
response.addCookie(monCookie);
```

Pour retrouver les informations stokees par le cookie
```
request.getCookies(); // Retourne un tableau de Cookies
```

Il existe plusieurs methodes qui permettent de manipuler les cookies, par exemple :
- setDomain(String pattern) : Permt de d'attribuer un domaine pour lequel s'applique le cookie
- getDomain() : Permt de recuperer le nom du domaine qui s'applique au cookie
- setMaxAge(int age) : Pour definir la duree de vie du cookie, en secondes.
- getMaxAge() : Permet de recuperer la durree de vie du cookie
- getName() : Permet de recuperer le nom du cookie
- setValue(String newValue) : Permet d'attribuer une valeur 
- setPath(String uri) : Permet d'attibuer un chemin ou sera appliquer le cookie. si cette propriete n'est pas definie, alors le cookie sera ppliquer à toutes les url de la meme page.
- getPath() : Permet de recuperer le chemin ou est appliquer le cookie
- setSecure(boolean flag) : Permet d'indiquer si le cookie doit etre envoyé sur un canal securisé ou pas de type SSL.
- setComment(String comment) : Permet d'attibuer un commentaire permettant d'indiquer le role du cookie
- getComment() : Permtet de recuperer le commentaire du cookie.


Avantages :
- Implmentation facile
- Meilleure performance
- On peut stocker un grand nombre d'informations.

Inconvenients : 
- Les cookies dependent des capacités et configuration du navigateur.
- Technique non conseillée pour stocker des informations sensibles.
- Peut contenir uniquement des informations textuelles.

#### 5.4 Les sessions
Les sessions permettent de maintenir des informations de façon temporaire coté serveur et qui sont accessible via les Servlets qui composent l'application.

##### Syntaxe
Pour creer une HttpSession (Qu'il faut appeler avant avant d'envoyer n'importe qquel contenu au client) : 
```
HttpSession session = request.getSession();
```

##### Fonctionnement
Lorsque le client envoie la premiere fois une requete au serveur, ce dernier crée un identifiant unique appelé "SessionID" et alloue un espace memoire pour sauvegarder les informations liées à ce dernier. Ce SessionID est ensuite envoyé au client sous forme de cookie, et chaque requete du client il sera inclut dans le Header de la requete.

Plusieurs methodes permettent de manipuler la session qui sont :
- getAttribute(String name) : Permet de recuperer la valeur de l'attribut ciblé
- getAttributeNames() : Permet de recuperer une enumeration de tous les noms d'attributs contenus dans la session
- getCreationTime() : Permet de recuperer le moment ou la session a été créé
- getId() : Permet de recuperer l'ID de la session
- getLastAccessedTime()
- getMaxInactiveInterval(): Permet de recuperer la valeur de la durée maximale de la session
- invalidate() : Permet de supprimer la session et toutes les ressources qui lui sont liées
- isNew() : Permet de recuperer de savoir si le client est nouveau ou pas
- removeAttribute(String name) : Permet de supprimer un objet dont le nom est definie
- setAttribute(String name, object value) Permet d'attribut un nom d'attribut et une valeur
- setMaxInactiveInterval(int interval) : duree de la session

Avantages :
- Implmentation facile
- On peut stocker du texte et des objets.
- Données stockées sont securisées vue qu'elles se trouvent coté serveur.

Inconvenients : 
- Non conseillé pour stocker des données pour une longue durée ou d'une taille importante
- Les sessions dependent des cookies et donc des navigateurs


### 6. Upload de fichier

Avant JavaEE 6, on devait utiliser une librairie externe (ex: apache common live upload) afin de supporter cette fonctionnalité. Mais depuis, JavaEE dispose de sa propre API mais necessaaite certains prerequis comme : 
- Utilisation de JavaEE 6 ou superieur avec Servlet 3.0
- Apache tomcat 7.0 ou superieur

Niveau code HTML:
```
<form method="POST" enctype="multipart/form-data" action="UploadServlet">
	<input name="file" type="file" multiple />
	<input type="submit" value="Upload" name="btnSubmit" />
</form>
```

Coté serveur, il faut une Servlet qui va s'occuper de la gestion de l'upload. Cette doit disposer de l'annotation `@MultipartConfig` comme suit :

```
@MultipartConfig(
		fileSizeThreshold = 1024*1024*2,
		location = "/myFiles/",
		maxFileSize= 1024*1024*5,
		maxRequestSize= 1024*1024*30		
)
```
*Remarque* : Toutes les tailles de fichiers sont en octets d'ou la multiplication par 1024.

**Part Interface** : represente une part dans le multipart form-data. elle definie certaines methodes pour travailler avec les donnees entrantes :
- getInputStream() : Permet de recuperer un objet Stream permetant de lire le contenu du part.
- getSize() : Permet de recuperer la taille des données uploadées en octet
- WRITE(String nomDuFichier) : Permet d'enregister les données uploadées à l'emplacement definie par l'annotation `@MultipartConfig`.

De plus, les methodes faisant partie de l'objet request (HttpServletRequest), specialement conçues pour l'upload comme :
- getParts() : Permet de recuperer une collection d'objets Part.
- getPart(String name) : Permet de recuperer un objet Part en se basant sur sont nom.


### 7. Deployment

Structure d'un projet
```
NomApp
  |_ WebContent
    |_ index.html
    |_ WEB-INF
      |_ web.xml
      |_ lib              // fichiers .jar uniquement utilisatble par ce projet
      |_ classes          // fichiers .class
```

Il existe deux façons pour deployer des applications Servlet :

##### Deploiement à chaud
Permet de deployer et undeployer les applications web sans redemarrer le serveur, par exemple en utilisant Tomcat Manager.
Avantages : Permet d'eviter des serveurs de productions de redemarrer 
Inconvenients : Peut causer des erreurs comme "out of memory"

##### Deploiement à froid
Redemarrer le serveur entierement afin de prendre en charge les changements.
Avantages : 
- Pas de cache
- Pas d'erreurs de memoire (Out of memory)

Inconvenients : 
- Cout du redemarrage/interruption du serveur


### 8. Debugging & Logging

Pour le logging, on peut utiliser l'implementation deja presente dans les Servlet comme suit
```
ServletContext  conntext = getServletContext();
if(....){
	context.log("Une erreur");
}
```
##### Log4j

Avantages :
- Pensé pour etre utilisé comme framework de logging
- Contient des loggers next-generation asynchrones
- Garbage free pour les applications Standalone et low Garbage pour application web ce qui reduit la pression sur le Garbage collector et ameliore la performance
- Facilité d'amelioration via un systeme de plugins
- Support des lambda expressions, Internationalisation, Message Objects et Custom log level


### 9. Internationalisation

##### Detection de la langue

L'objet Locale (java.util.Locale) represente une zone geographique. Cette classe est utile pour afficher des devises et format de dates qui varient selon les pays. Pour cela, cette derniere fournie plusieurs methodes comme :

- getDefault() : Permet de recuperer la valeur par defaut depuis l'instance en cours de la jvm qui met cette valeur lors de son deramarrage en se basant sur la machine hote. 
- getAvailableLocales() : Permet de recuperer un tableau des locales installées
- getDisplayLanguage() : Permet de recuperer la langue qui affichée pour l'utilisateur
- getAvailableLocales() : Permet de recuperer
- ....

Pour detecter les preferences du client on peut uiliser 
```
Locale locale = request.getLocale();
String language = locale.getLanguage();
```

##### Mise en place du Locale :
- Creer un dossier i18n au meme niveau que les sources
- Ajouter les traductions sous forme de fichiers properties : resourceBundle_fr_FR.properties
- Utiliser ces ressources dans le code java :
```
ResourceBundle resourceBundle = ResourceBundle.getBundle("i18n.resourceBundle", request.getLocale());
String title = resourceBundle.getString("title");
```

### 10. Annotations :

##### @WebServlet : 
Cette annotation se definie juste au dessu du nom de la classe, est utilisée pour declarer une Servlet, elle remplace la partie qui definie la Servlet dans le fichier *web.xml*.
```
<servlet>
	<servlet-name>NomDeLaServlet</servlet-name>
	<servlet-class>com.test.MaServlet</servlet-class>
</servlet>

<servlet-mapping>
	<servlet-name>NomDeLaServlet</servlet-name>
	<url-pattern>/DoSomething</url-pattern>
</servlet-mapping>
```

|Attributs|Obligatoire|Description|
|---|---|---|
|name="NomDeLaServlet" | |Represente le nom de la Servlet. S'il n'est specifié, c'est le nom de la classe qui sera utilisé |
|value="/DoSomething", urlPatterns="/DoSomething" | Oui | On doit fournir au minimum une seule url, soit en utilisant l'attribut *value* ou *urlPattern* mais pas les deux au meme temps. l'attribut *value* est recommandé lorsque ce dernier est le seul à etre definie. sinon, il faut utiliser *urlPattern*. On peut associer plusieurs urls à notre Servlet en  utilisant la syntaxe `urlPatterns={"/first", "/second"}`|
|description |Non| Represente une description de la Servlet |
|displayName |Non| Represente une le nom d'affichage de la Servlet |
|asynSupported |Non| Permet de dire si la Servlet supporte l'utilisation d'operations asynchrones |
|loadOnStartup |Non| Cet attribut permet de dire au ServletContainer de charger la Servlet selon un ordre definie par la valeur, plus la valeur est faible, plus la Servlet est chargée en premier. Si la valeur est negative ou indefinie alors le ServletContainer peut charger la Servlet à tout moment.  |
|initParams|Non|Permet de definir des valeurs à l'initialisation pour la Servlet. |
|smallIcon|Non|Retourne une petite icone associée à la Servlet |
|largeIcon|Non|Retourne une grande icone associée à la Servlet |


##### @WebInitParam
Cette annotation est utilisée pour declarer une initialisation de parametres qu'on souhaite passer à la Servlet ou à un filtre. Elle remplace à son tour la partie qu'on peut trouver dans le fichier *web.xml*.
```
<init-param>
	<param-name>utilisateur</param-name>
	<param-value>toto</param-value>
</init-param>
```
Pour recuperer cette valeur coté Servlet :
```
String user = getServletConfig().getInitParameter("utilisateur");
```

L'equivalent de cette manipulation en utilisant les annotation via @WebInitParam :
```
@WebServlet(
  name="Hello",
  urlPatterns="/HelloServlet",
  initParams={
  	@WebInitParam(name="utilisateur", value="toto")
  	@WebInitParam(name="email", value="toto.titi@societe.com")  	
  }
)
```

##### @WebFilter
Cette annotation permet de declarer un filtre ce qui remplace la partie dans *web.xml*. Elle necessite au moins une urlPattern sur laquelle le filtre va s'appliquer. Tous les autres attributs sont facultatifs.

- @WebFilter ("/*") : Permet d'appliquer le filtre sur toutes les Servlets.
- @WebFilter (servletNames="/HelloServlet") : Permet d'appliquer le filtre uniquement à la Servlet nommée.
- @WebFilter (servletNames={"/ServletOne", "/ServletTwo"}) : Permet d'appliquer le filtre à une liste de Servlets.
- initParams : identique à celui des Servlets.
 


### 11. Servlet Asynchrone

##### 11.1 Limites des Servlets traditionnelles
Le Servlet container utilise en general un thread par requete client (depuis le ThreadPool) et ainsi de suite pour les nouvelles requetes. Lorsque une requete est traitée, le thraed est liberé et retourne dans le pool en attendant de nouvelles connexions. Mais une limitation à ce systeme subsiste. Si une requete prend plus de temps que prevu, par exemple suite à un accès base de données ou à un service à distance, alors le thread en charge de cette requete va passer en mode **idle**. Ces scenarios representent des operations bloquantes ce qui limite la "*scalabilité*"  de l'application. Les nouvelles requetes reçues sont maintenue en file d'attente en attandant que le processus se termine. Il faut donc s'assurer qu'aucun Thread associé à une requete ne soit en mode idle et surtout les utiliser pour traiter de nouvelles requetes.

##### 11.2 Modele Asynchrone
Chaque fois que le Servlet Container recoit une requete, un thread est associé à cette derniere. Une fois qu'une requte est traitée, le meme thread est associé à une autre requete.
Si une operation devient bloquante alors elle est attribuée à un contexte d'execution asynchrone via la creation d'un nouveau thread et libere immediatement l'ancien thread pour qu'il revienne au thread pool.

Pour activer l'execution asynchrone sur une Servlet, il faut ajouter l'attribut `asyncSupported=true` comme suit : 
```
@WebServlet(urlPatterns="/HelloServlet", asyncSupported=true)
```
Si on souhaite recupere une instance de contexte asynchrone, il faut utiliser la methode `startAsync` comme suit :

```
AsyncContext aContext = request.startAsync();
```
Cet appel met la requette dans un mode asynchrone et assure que la reponse n'est pas envoyée après la fin de la methode service(). Après la fin de l'operation bloquante, on doit ensuite generer la reponse dans le contexte asynchrone  ou l'envoyer à une autre Servlet.

- void start(Runnable run) : Permet de dire au container de d'executer le code dans une nouveau thread
- getRequest()
- getResponse()
- complete()
- dispatch(String path)






