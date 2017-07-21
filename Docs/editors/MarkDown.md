# MarkDown
Last edit: 21.07.2017

### Paragraphes
- Séparer les lignes par une ligne vide pour creer un nouveau paragraphe.
- Le fait d'ajouter deux espaces à la fin d'une ligne permet de passer le texte qui suit à la ligne suivante sans créer de nouveaux paragraphes.

### Headers
- Le nombre de "#" definit le niveau du Heading.  
- Maximum 6 niveaux.  
- Les deux premiers niveaux peuvent replacer par un soulignement du texte par les caractères `=` ou `-`

### Emphases
- Pour ecrire en _Italique_, il suffit de mettre le texte entre entre deux `_` ou `*`
- On peut échapper ces deux caractères si besoin.
- Si on double ces caracteres on obtient un texte en __Gras__.

### Quotes
- Pour utiliser les qutotes, Il faut preceder le texte par le caractère `>`
- Si on souhaite decouper notre __quote__ il suffit d'ajouter deux éspaces à la fin de la ligne.

### CodeSource
- Sur une ligne `
- Sur plusieurs lignes ```
- On peut indenter notre code  avec 4 éspaces
- Certains derivés du markdown permettent la coloration syntaxique. Pour cela il suffit de mettre le nom du langage juste après les trois premiers Backtick
```java
  System.out.println("Hello world")
```

### Listes
- Pour afficher une liste non ordonnée, il suffit de preceder chaque element par `*` `+` ou `-`.
- Pour les liste ordonnées, il suffit de precder les elements par un nombre et un point.
- Pour avoir des listes imbriquées, il suffit d'indenter le texte avec deux espaces selon le niveau.

### Horizontal Rules
- Pour afficher une ligne Horizontale il suffit de mettre de repeter trois fois l'un des caractères suivants: `_` `-` `*`

### liens
- Les Urls et adresses mails sont automatiquement convertis en liens.
- Pour afficher un lien il suffit de respecter le format `[Label](URL "Tooltip description")`
- On peut utiliser des references vers des liens. Il faut tout d'abord declarer le lien comme une variable comme suit `[Var_Label]:URL "Tooltip description"` puis l'utiliser dans la page `[Url_Label][Var_Label]`

### Images
- Pour afficher une image il suffit de respecter le format `![Label](URL "description")`   
![Hello](http://placehold.it/500x50 "Ceci est une description")
- Idem que les liens, on peut utiliser des references vers des images.

### Tableaux
```
|A|B|C|
|-|-|-|
|1|2|3|
```
