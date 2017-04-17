Https
=====

-   Confidentialité, Intégrité, Authenticité

### Redirection

1.  Le client demande une page en Http

2.  Le serveur répond avec un code `301 (Moved Permanently)` qui contient la
    nouvelle URL en Https via l'attribut `Location`

3.  Le client demande la nouvelle URL.

Remarques :

-   Deux requêtes sont lancées coté client

-   La première requête (non sécurisée) présente un risque potentiel.

Solution ?

Utiliser le header http Strict transport Security (HSTS)

(HSTS) est un mécanisme de politique de sécurité proposé pour HTTP, permettant à
un serveur web de déclarer à un agent utilisateur (comme un navigateur web),
compatible, qu'il doit interagir avec lui en utilisant une connexion sécurisée
(comme HTTPS). La politique est donc communiquée à l'agent utilisateur par le
serveur via la réponse HTTP, dans le champ d'en-tête nommé
« Strict-Transport-Security ». La politique spécifie une période de temps durant
laquelle l'agent utilisateur doit accéder au serveur uniquement de façon
sécurisée.

Fonctionnement

1.  Le client demande une page en Http

2.  Le serveur répond avec un code `307 (Internal Redirect)` de taille 0 octet,
    avec un response header : `Non-Autoritative-Reason : HSTS`

3.  Le client demande la nouvelle URL sécurisée, qui contient un response header
    `strict-transport-security : max-age=2592000`. Ce qui signifie que durant
    un mois (soit 2592000 secondes) le navigateur ne peut communiquer avec le
    serveur qu’en utilisant une communication sécurisée Https.

Ceci étant dit, un problème subsiste. La toute première requête n’est toujours
pas sécurisée. Ou TOFU (Trust On First Use). On fait confiance au serveur lors
de la toute première connexion non sécurisée.

On peut résoudre ce problème en modifiant le header comme suit : `strict-transport-security : max-age=3156000 ; includeSubDomains ; preload`
puis soumettre le site pour qu’il soit ajouté à liste des sites uniquement
accessibles en https : <https://hstspreload.org>

#### Mettre à jours les contenus non sécurisés

Solution1 : Modifier les liens pour qu’ils utilisent https au lieu de http

Solution2 : Utiliser des liens sans preciser le protocole, exemple
`href=//www.test.com/sources.jpg`

Solution3 : Utiliser le méta-tag `<meta http-equiv=’Content-Security-Policy’
content=‘upgrade-insecure-requests’>` qui existe aussi en response header.

Une fois qu’on est sûr d’avoir mis à jour tous liens, on peut ajouter le méta
tag `<meta http-equiv=’ Content-Security-Policy’
content=’block-all*mixed*content’>`
