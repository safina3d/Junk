# CSS
###### Cascading Style Sheets

### Quelques notes
- Les navigateurs lisent les selecteurs Css de droite à gauche.
- Le navigateur ajoute un `margin: 8px` au body qu'on peut supprimer
- Si il y a conflit entre deux ou plusieurs règles Css visant le même élément, le navigateur applique un calcul simple. Chaque règle est associée à une valeur.  
 ```c
 !important         => 10000
 Inline Style       => 1000
 ID                 => 100
 Class/Pseudo Class => 10
 élément            => 1
 ```  
 Il n'a ensuite qu'a faire la somme de ces règles pour chaque selecteur. Le selecteur dont la valeur est la plus élevée est choisi. Exemple:
  - `#main div .quote` est converti en `100 + 1 + 10 = 111`
  - `div .quote` en `1 + 10 = 11`
  - `.quote` en `10`
    - C'est le premier sélecteur `111` qui est donc choisi.

-----

### 1. Les selecteurs

- #### Descendant Selector :
  - Cibler des éléments selon leurs emplacement au sein d'autres éléments
  - Il suffit de concatener des selecteurs en les separant avec un __espace__.
  - Exemple d'utilisation : `div p a {...}`
  - Quelques remarques :
    - Pas plus de 3 niveaux, sinon inneficace
    - Attention aux performances
    - Difficulté de maintient et de surcharge

- #### Adjacent Selector
  - __elA + elB__ : Permet de cibler l'élement (elB) qui est adjacent à l'élément A.  
Ex : `h1 + p {....}`  

- #### Sibling
  - __elA ~ elB__ : Permet de cibler l'élement (elB) qui est frere de l'élément A, même si d'autres éléments sont entre les deux.  
Ex : `h1 ~ p {....}`  

- #### Pseudo classes
  - __:not()__: Permet de nier un selecteur et donc cibler son inverse
  - __:first-child__ : Permet de ciblier le premier élément
  - __:last-child__ : Permet de ciblier le dernier élément
  - __:nth-child(n)__ :
    - Permet de ciblier le N-ieme élément
    - On peut remplacer la valeur entre parentese par une expression.  
    Ex : `li:nth-child(2n + 1)` qui va sélectionner tous elemments impaires.
  - __p:first-line__ : Permet de cibler la premiere ligne d'un paragraphe.
  - __p:first-letter__ : Permet de cibler la premiere lettre d'un paragraphe
  - __:lang(fr)__: Permet de cibler les éléments dont la langue est le français (via l'attribut `lang`)

- #### Attribute values
  L'élement est ciblé si :
  - __[attr]__ : l'attribut existe
  - __[attr="foo"]__ : La valeur de l'attribut vaut exactement _foo_
  - __[attr*="foo"]__ : La valeur de l'attribut contient _foo_
  - __[attr~="foo"]__ : La valeur de l'attribut séparée par des espaces contient _foo_
  - __[attr^="foo"]__ : La valeur de l'attribut commence par _foo_
  - __[attr|="foo"]__ : La valeur de l'attribut séparée par des tirets contient _foo_
  - __[attr$="foo"]__ : La valeur de l'attribut se termine par _foo_

-------

### Box Model
- `box-sizing: content-box` (_valeur par defaut_). La largeur totale est la somme de toutes ses composantes i.e width + paddings + borders, donc, **largeur totale > width **
- `box-sizing: border-box` la largeur totale est celle definie par la proporieté **width**

### Transformations
  - __Scale__: `transform: scale(1.5)`
  - __Rotation__: `transform: rotate(90deg)`

------

### Correction du probleme lié au float
Si tous les éléments contenu dans un élément sont floatant alors ce dernier va avoir une hauteur de zero. Ce qui peut poser probleme pour les élements suivants. Pour corriger ce probleme plusieurs techniques sont possibles :
- Ajouter une div à la fin avec une classe qui contient `clear: both`
- Ajouter `overflow: hidden` à l'élément parent
- Ajouter `float: left` à l'élément parent
- Utiliser le hack __clear_fix__ sur l'élément parent
```css
/* Clearfix hack */
.clearfix:before, .clearfix:after {
    content: ' ';
    display: table;
}
.clearfix:after {
    clear: both;
}
```
