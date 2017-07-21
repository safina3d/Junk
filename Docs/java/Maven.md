# MAVEN






# PLAN :

- 1. INTRODUCTION A MAVEN
  - 1.1
  - 1.2
  - 1.3
  - 1.4
  - 1.5
  - 1.6
  - 1.7
  - 1.8

- 2. GESTION DES DEPENDANCES


### 1.1 Description :
Maven est :
  - Un outil de Build : comme Ant ou Make, permet de prendre notre code et de
  generer une librairie ex : jar, war, ear, etc..
  - Un outil de gestion des dependances.
  - Un outil de gestion de projet
  - Une approche uniformisée pour la construction d'applications.
  - Un Outil en ligne de commandes
  - Capable d'etre integrer dans un iDE


  #### Qu'est ce qu'un outil de build ?
    - Un outil de build permet :
      - La creation d'artefacts* deployables ex: jar, war, ear, etc..
      - Automatisation et Repetitions de taches ex: execution de tests
      - Deployment des artefacts sur le serveur ou dans des repositoies.
      - Independance vis a vis l'IDE
      - L'integration avec d'autre outils d'integration.

  #### Qu'est ce qu'un outil de gestion des dependances ?
    - Un outil de gestion de dependances permet :
      - Télécharger les dependances d'un projet depuis un repository centralisé,
       ce qui nous évite d'aller sur le web de rechercher nous meme jar par jar
       toutes les dependances du projet.
      - Le chargement automatique de dependances d'une dependances (*transitive dependancies*).
       ex: si mon projet utilise spring, les dependance de spring sont chargées
       automatiquement.
      - "Dependency scoping" : choisir le context ou le scope d'utilisation d'une dependance.
      par exemple pour mes tests j'utilise une dependance qui je ne veux surtout pas utilisé
      après le deployement.

    #### Qu'est ce qu'un outil de gestion de projet ?
    - Un outil de gestion de projet permet :
      - Le versioning de notre projet
      - Changement de logs
      - La documentation
      - Javadocs
      - Les rapports


### 1.2 L'ecosystem de Maven
  #### POM (Project Object Model) :
  - Le fichier pom.xml est un fichier xml qui permet de decrire, configurer et
  personnaliser un projet Maven.
  - Il permet de specifier des informations sur le projet, les plugins utilisés,
   les Goals, les dependances et les profiles.
  - Maven utilise ce fichier pour faire le build du projet.
  - Definit une "Adresse", Maven utilise un systeme de coordonnées. ou il specifie un ArtifactId, un groupId et un numero de version. ceci devient utile quand on essaye de retouver notre projet sur un systeme à distance.

  #### Repositories :
    - Les repositories contiennent les build des artifact(ex: plugins, jar, Archetypes) et des dependances(ex : spring),  de tout type.
    - Un repository peut etre local ou à distance. le repository local peut etre conciderer  comme un cache, par exemple si un autre projet à besoin de la meme dependance, elle sera deja disponible et plus besoin d'aller la chercher à distance.
    -
  #### Plugins & Goals :
    - Un plugin est une collection de GOALS.
    - un GOAL est une action qu'on execute. exemple : le plugin de compilation dispose de deux goals ou actions qui sont: la compilation du code et l'execution des tests.

  #### Cycle de vie et Phases :
    - Un cycle de vie est un ensemble de phases nommées (ou d'evenements).
    - les phases sont executées de façon sequentielle.
    - Maven dispose de 3 cycles de vies : clean, default et site.
    - Executer une phase revient à executer toutes les phases qui la precedent.


### 1.3 Installation :
  - Telechager maven depuis le site d'apache https://maven.apache.org/
  - Decompresser le zip.
  - Suivre les etapes indiquées dans le readme.txt


### 1.4 Creation d'un projet Maven :
  - Creation du pom.xml
    - On creer un fichier pom.xml, et on choisi un schema qui est http://maven.apache.org/xsd/maven-4.0.0.xsd
    - le tag **< project >** est la racine de notre fichier xml
    - On ajoute au sein de notre tag **< project >** les tags:
      - *modelVersion* : pour maven2/3, la valeur doit etre 4.0.0
      - *artifactId* : nom de l'artifact
      - *groupId* : nom du groupe
      - *version* : la version en cours

    exemple :
    ````
    <?xml version="1.0" encoding="UTF-8"?>
    <project>
    	<modelVersion>4.0.0</modelVersion>
    	<artifactId>maven-firest-test</artifactId>
    	<groupId>com.test.www</groupId>
    	<version>1.0.0</version>
    </project>
    ````

    Afin de compiler notre code on peut utiliser la commande ```mvn compile ```, Le resultat se trouve dans le dossier target.

    On peut ajouter d'autres informations dans notre pom.xml afin de donner plus d'informations sur notre projet ex :
    - *packaging* : permet de specifier le type d'artefact qu'on veut créer, ex : jar, war, ear. par default la valeur est *jar* si ce tag n'est pas ajouté. ex :
      - ```<package>jar</package>```
    - *name* : permet de donner un nom à notre projet ex:
      - ```<name>Mon super projet</name>```
    - *description* : permet de donner une description à notre projet
      - ```<description>bla bla bla projet</description>```
    - *url* : site web du projet
      - ```<url>http://www.maven-test.com</url>```
    - *licenses* : permet d'afficher la liste des licences relatives au projet. on peut avoir plusieurs licences pour le meme projet.
      ```
      <licenses>
        <license>
          <name>MIT</name>
          <comments>Le partage c'est bien.</comments>
        </license>
      </licenses>
      ```
    - *organization* : permet de specifier l'organisation deriere le projet
    ```
    <organization>
        <name>ZZE Maven Tester</name>
        <url>http://www.maven-test.com</url>
    </organization>
    ```
    - *developers* : permet de specifier la liste des developpeurs qui travaillent sur le projet.
    ```
    <developers>
        <developer>
          <name>ZZE</name>
          <email>tooto@abc.com</email>
        </developer>
    </developers>
    ```

    On peut generer un site qui contient les informations que nous avons specifier dans notre fichier pom.xml via la commande : ```mvn site```, le resultat se trouve dans le dossier *target*


### 1.5 Comment maven trouve les fichiers à compiler sans lui preciser leurs emplacement ?
Maven s'appuie sur le principe de "Convention over Configuration". Maven dispose d'une liste de chemins prédefini par convention (*standard directory layout*), et c'est dans cette qu'il va chercher les fichiers.
ex :
  - "src/main/java" est le chemin pour les sources Java
  - "src/main/ressources" : pour les librairies et autres differentes ressources
  - "src/main/config" : pour les fichiers de configuration.
  - "src8main/webapp" : pour les ressources de l'application web
  - "src/test/java" : les fichiers de test

  pour trouver la liste complete : https://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html

Cependant, Il peut nous arriver de travailler sur un projet qui ne respecte pas ses standards. il faut donc specifier les nouveaux emplacements dans le pom.xml, grace au tag **build** :

  ```
  <build>
      <sourceDirectory>src/nouveau_Chemin/java</sourceDirectory>
      <directory>autre_Chemin_De_Resultat/my_Target</directory>      
  </build>
  ```

### 1.6 Heritage :

  Tout comme Java, Maven dispose d'une relation d'heritage. Un fichier pom peut heriter d'un autre fichier pom. le fichier pom enfant peut ainsi redefinir certaines proprietes du fichier parent.

  Un autre point commun avec Java, est le fait que tout fichier pom herite d'un "super pom" qu'on peut trouver dans `$chemin_installation_maven/lib/maven-builder-x.y.z.jar`. Ce fichier jar contient le fichier pom parent de tous les autres.

  Si on souhaite voir le fichier pom enfant dans sa version complete, on peut utiliser la commande : `mvn help:effective-pom`

  On peut heriter depuis un autre fichier pom. pour cela il faut :
  - Dans le pom enfant : ajouter le tag **parent** dans lequel on va specifier les coordonnées du fichier pom parent  c.a.d *l'artifactId, le groupId et la version*.
  - Ensuite, il faut installer le pom parent dans le repository local avec la commande : `mvn install`



### 1.7 Profiles :
Permet de personnaliser les actions qu'on souhaite executer en fonction de l'environnement. Ceci grace au tag **profiles** qui contient un ou plusieurs **profile** , dont chacun dispose d'un identifiant et dans lequels on peut redefinir les proprietes existante

```
<profiles>
    <profile>
      <id>dev</id>
      <directory>my_dev_Target</directory>  
    </profile>
    <profile>
      <id>prod</id>
      <directory>my_prod_Target</directory>  
    </profile>   
</profiles>
```
Afin d'executer une action avec un profile donnée, Il faut utiliser l'option *-P* comme ceci : `mvn -P nom_du_profile compile`.

Cependant, Il peut arriver qu'on puisse pas definir le profile qu'on souhaite executer. Maven propose une solution qui est l'utilisation d'**Activator**, qui, en fonction de la valeur de certaines variables d'environnements ou autres va chosir le bon profile et l'executer.
ceci ce fait via l'ajout du tag `activation` au sein du tag profile (après id) comme ceci :
il peut prendre comme valeur les tags suivants :
  - *activeByDefault* : pour l'activer par default
  - *file* : verifier la presence d'un fichier.
  - *jdk* : verifier la version de la jdk.
  - *os* : verifier le systeme d'exploitation.
  - *property* : verifier la valeur d'une variable d'environnement.

```
<profiles>
    <profile>
      <id>dev</id>
      <activation>
        <property>
          <name>env.MA_VARIABLE</name>
          <value>DEV</value>
        </property>
      </activation>
      <directory>my_dev_Target</directory>  
    </profile>
    <profile>
      <id>prod</id>
      <directory>my_prod_Target</directory>  
    </profile>   
</profiles>
```

### 1.8 Generer un projet :
On peut generer un nouveau projet en partant de template existant dans les repositoies à distance en utilisant les commandes suivantes :
  - `mvn archetype:generate ` : permet de generer un projet basé sur un archetype (template)
  - Choisir le template quand c'est demandé par l'invite de commande
  - Precisier les valeurs des tags importants comme  artifactId, groupId, package, version, etc...
  - Afin de creer un projet eclipse depuis le projet creer par maven on peut utiliser la commande `mvn eclipse:eclipse`
  - puis importer le projet dans eclipse.


## 2. GESTION DES DEPENDANCES

### 2.1 Dependances simples :

La liste des dependances d'un projet se trouve dans le fichier pom.xml à l'interieur
du tag **dependencies**, chaque dependance est associée à un tag **dependency**
ou il faut indiquer le *groupId*, *artifactId*, la *version* et le *scope* comme ceci :

```
<dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>   
</dependencies>
```

Ces informations sont disponibles sur le site http://search.maven.org
Il suffit ensuite d'utiliser les commandes :
  - `mvn dependency:copy-dependencies` pour charger les dependances.
  - `mvn eclipse:eclipse` pour que le projet eclipse prenne en compte ces nouvelles dependances. (voir Referenced Librairies)


### 2.1 Dependances transitives (dependance d'une autre dependance) :
Maven se charge seul de charger les dependances des dependances!

### 2.2 Repositories à distance :
Lorsqu'on ajoute une dependance dans notre fichier pom.xml, Maven s'occupe de charger
cette dependance depuis l'un de ses repositoies à distance. pour voir l'adresse
de ces repositoies, utiliser la commande : `mvn help:effective-pom` pour afficher
le contenu complet du fichier pom.xml puis chercher le tag **repositories**

On peut dire à maven d'aller chercher dans d'autres repositories personnalisés.
Mais pour cela il faut aller dans la configuration de maven :
 - Aller dans le repertoire d'installation de maven
 - Ouvrir le fichier settings.xml qui se trouve dans le dossier conf : `conf/settings.xml`
 puis ajouter un *profile* qui contient les informations des nouveaux repositories comme ceci :

```
</profile>
  <id>mon_remote_perso</id>
  <repositories>
      <repository>
        <id>my_app_repository</id>
        <url>http://www.myhost.com/maven/super</url>
      </repository>
  </repositories>
</profile>
```
Il faut ensuite activer le profile ajouté en ajoutant un tag *activeProfiles*

```
<activeProfiles>
  <activeProfile>mon_remote_perso<activeProfile>
</activeProfiles>
```

### 2.3 Dependency Scope

Le *Dependency scope* permet de dire quand est ce qu'une dependance est disponible pour pouvoir etre utilisée.
Le scope peut etre ajouté dans le tag *dependency*.
Le scope par defaut pour toutes les dependances est *compile*, il permet de dire que cette dependance est disponible
durant le build, le test

Quelques scope :
  - *import* : rarement utilisé
  - *provided* : la dependance est dispoible en dev mais pas en deployement, par exemple l'api servlet, peut etre utilisé lors du dev mais après deployement elle est supprimé car elle est fournit par le serveur d'application type tomcat.
  - *runtime* : la dependance est utilisée lorsqu'on execute le systeme ou l'application. elle n'est donc pas utilisé pour la compilation.
  - *system* : la dependance est fournit par le system de fichier ou l'os (A Eviter!)
  - *test* :  la dependance est disponible pour les tests.
  - *compile* : disponible partout et tout le temps.

### 2.4 La Gestion des conflits :

Il arrive souvent qu'il y est conflit entre une dependance transitive et une autre de deux ou plusieurs depandances.
Maven est assez intelligent pour choisir les bonnes depandances. cependant si on veut lui forcer la main et garder une depandance qu'il n'a pas choisit,
on exclut les autres, en ajoutant le tag *exclusions* comme ceci :

```
<dependencies>
  <dependency>
    ...
    ...
    <exclusions>
      <exclusion>
        <groupId>id_du_groupe</groupId>
        <artifactId>id_de_artifact</artifactId>
      <exclusion>
    </exclusions>
  </dependency>
</dependencies>
```


## 3. CYCLE DE VIE ET PLUGINS

### 3.1 Cycle de vie

Un cycle de vie est un ensemble d'etapes à franchir afin de creer notre Artifact. Ces etapes sont ce qu'on appelle des *PHASES*.
Il existe 3 cycle de vie dans maven qui sont : **clean**, **default**, **site**. et chaque cycle de vie a ses propres phases. Quand un
cycle de vie est executé un goal est lié à une phase en particulier.

Exemple :
Lors de l'utilisation de la commande `mvn clean`, la phase *clean* est appelée mais aussi toutes les phases qui la precedent dans le cycle de vie *clean*.

On peut avoir plus de detail sur l'execution de cette commande en utilisant le plugin help avec la commande `mvn help:describe -Dcmd=clean`

On executer plusieurs phases dans la meme commande, par exemple `mvn clean install`

### 3.2 Les phases
Les cycles les plus importants dans maven :
  - **compile** : permet de compiler toutes les sources du projet.
  - **test-compile** : permet de compiler les tests du projet.
  - **test** : permet d'executer les tests. par defaut, le build est annulé si un test echoue, mais on peut le configurer pour ignorer l'echec de test et continuer, comme ceci :
  ```
  <build>
    <plugins>
      <groupId>com.test.www</groupId>
      <artifactId>mon_app</artifactId>
      <configuration>
        <testFailureIgnore>true</testFailureIgnore>
      </configuration>
    </plugins>
  </build>
  ```
  - **package** : prend le code compilé et creer avec un package selon le format choisit, ex: jar, war, ear.
  - **install** : prend le package et l'install dans le repository local pour qu'il puisse etre utilisé comme dependance dans d'autres projets en local.
  - **deploy** : prend le package final et le copie dans un repository à distance pour qu'il puisse etre partagé ou utilis" par d'autres developpeurs.

### 3.3 Plugins & Goals :
Un plugin contient plusieurs Goals, un Goal contient plusieurs taches. par analogy, un plugin correspond à une classe et un goal à une methode de cette classe.

pour appler un plugin on utilise la commande `mvn plugin:goal` ex: `mvn compiler:compile`
Afin d'avoir une description d'un plugin on peut utiliser la commande `mvn help:describe -Dplugin=compiler`

### 3.4 Plugins properties :
Afin d'ajouter des option à l'execution d'un plugin goal, on peut ajouter des *properties*.
On peut trouver la liste de ces proprietes via la commande : `mvn help:describe -Dcmd=nom_du_plugin:nom_du_goal -Ddetail`
Il suffit ensuite d'ajouter la propriete qu'on souhaite à l'execution comme ceci : `mvn nom_du_plugin:nom_du_goal -Dnom_propriete=true`
par exemple : `mvn compiler:compile -Dmaven.compiler.verbose=true`.

Autre methode, est d'utiliser le fichier pom.xml
```
<build>
  <pluginManagement>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.2</version>
        <configuration>
          <verbose>true</verbose>
        </configuration>        
      </plugin>
    </plugins>
  </pluginManagement>
</build>
```

### 3.5 Plugin personnalisé

Pour creer notre plugin, on utilise un template existant (archetype) via la commande :
`mvn archetype:generate -DgroupId=com.test.www -DartifactId=first-custom-plugin -DarchetypeArtifactId=maven-archetype-mojo -DarchetypeGroupId=org.apache.maven.archetypes`

On peut ensuite installer notre plugin afin qu'il soit disponible pour tous les projets en local, avec la commande :
`mvn install`

Afin d'executer notre plugin depuis un autre projet on peut utiliser :
- Methode 01 :
  - la commande : `mvn groupeId_du_plugin:artifactId_du_plugin:nom_du_goal`

- Methode 02 :
  - Le but est d'attribuer un nom à notre plugin afin qu'on puisse l'appeler de
  la façon : `mvn nom_du_plugin:nom_du_goal`
  - Il faut donc aller dans le fichier *pom.xml* du plugin et ajouter un tag **build**
  et on ajoute le plugin **maven-plugin-plugin** qui va nous permettre d'attribuer un nom (ou un prefixe)
  à notre plugin via le tag **goalPrefix**.
  ```
  <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>          
          <artifactId>maven-plugin-plugin</artifactId>
          <version>2.3</version>
          <configuration>
            <goalPrefix>nom_du_plugin</goalPrefix>
          </configuration>        
        </plugin>
      </plugins>
  </build>
  ```
  - Ne pas oublier d'installer le plugin à nouveau `mvn install` pour le rendre disponible pour les autres projets.

  - Maintenant, il faut aller dans le fichier *pom.xml* du projet qui va utiliser notre plugin
  et ajouter un tag **build*** qui va contenir la reference vers notre plugin

  ```
  <build>
      <plugins>
        <plugin>
          <groupId>groupeId_du_plugin</groupId>          
          <artifactId>artifactId_du_plugin</artifactId>
          <version>version_du_plugin</version>

          <executions>
            <execution>
              <id>id_arbitraire_du_plugin</id>
              <phase>compile</phase>
              <goals>
                <goal>nom_du_goal</goal>
              </goals>
            </execution>  
          </executions>  

        </plugin>
      </plugins>
  </build>
  ```
  - Maintenant on peut appeler notre plugin avec la commande `mvn nom_du_plugin:nom_du_goal`


## 4. GOALS & PLUGINS

Pour voir la liste des plugins disponibles dans Maven : http://maven.apache.org/plugins/index.html

### 4.1 clean plugin
`mvn clean`

### 4.2 Jar plugin
Permet de generer un jar depuis l'artefect. Ce plugin est utilisé lorsquu'on utilise
la commande `mvn package`, mais on peut aussi l'appeler de façon plus explicite comme ceci : `mvn jar:jar`

On peut preciser certaines options comme, le nom du jar avec l'option *finalName* et forcer la creation
du fichier jar meme si le projet n'a pas changé avec l'option *forceCreation* :
`mvn jar:jar -Djar:finalName=nom_du_jar_en_sortie -Djar.forceCreation=true`

### 4.3 Javadoc plugin
Permet de generer la Javadoc du projet
`mvn javadoc:javadoc`

### 4.4 Install & Deploy plugins
### 4.5 Surefire Plugin
### 4.6 War plugin


## 5. ARCHETYPES

### 5.1 Creer un archetype

#### Creation de l'archetype

Tout d'abord nous avons besoin d'un projet simple qu'on va transformer en archetype.
On peut utiliser la commande `mvn archetype:generate` pour generer notre projet.
puis `mvn eclipse:eclipse` pour le transformer en un projet eclipse pour qu'on puisse
l'importer.

Ensuite On ajoute toutes les classes qu'on souhaite qui seront disponible dans
notre template pour les autre utilisateurs.

Pour creer notre template, il faut utiliser la commande `mvn archetype:create-from-project`
Aller ensuite dans le dossier `target/generated-sources/archetype` puis lancer
la commande `mvn install` afin de le rendre disponible.

#### Utilisation de l'archetype

Comme pour tout autre archetype, lancer la commande `mvn archetype:generate`
puis selection le bon archetype que nous avons créé.



















## Commandes :
  - `mvn clean` : permet de nettoyer le dossier target
  - `mvn -P nom_du_profile commande` : permet d'executer une commande avec un profile donné.
  - `mvn archetype:generate ` : permet de generer un projet basé sur un archetype
  - `mvn dependency:copy-dependencies` : Telechager les dependances
  - `mvn eclipse:eclipse` pour que faire un nouveau build du projet eclipse et qu'il prenne en compte les nouvelles dependances.
  - `mvn help:effective-pom`: afficher le contenu complet du fichier pom.xml
  - `mvn -X commande`: -X option de debug
  - `mvn help:describe -Dcmd=clean`: voir la liste des plugins liée à une phase
  - `mvn help:describe -Dplugin=compiler`: voir le detail d'un plugin et surtout afficher la liste de ses goals.
  - `mvn plugin:goal` : executer le goal d'un plugin

## Lexique :
  - A
    - **Activator**:
    - **Archetype**:
    - **Artifact** :

  - G
    - **GOAL** : un goal correspond à une action ou une tache.
  - P
    - **pom file** : Project Object Model (pom.xml)
    - **Profile** :
    - **Plugin** : collection of goals (ou d'actions executées).
  - S
    - **Scope**

### Etapes de creation d'un nouveau projet Java avec maven

- Creer un fichier pom
