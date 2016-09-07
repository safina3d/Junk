# SSH et Telnet

	1. SSH, Introduction 
	2. Debugging
    3. Telnet

## SSH, Introduction

SSH (Secure Shell)

SSH fonctionne avec 3 modes :

#### 1. Authentification RSA rhost (depreciée)
Possible si le serveur à distance contient l'un de ces fichiers 
	- /etc/hosts.equiv
	- /etc/ssh/shosts.equiv
	- /home/USR_NAME/.rhosts
	- /home/USR_NAME/.shosts	
Ces fichiers contiennent une entrée identifiant le client qui essaye de se connecter avec l'utilisateur en cours.

#### 2. Authentification par la paire clés publique/privée :

Pour generer la paire de clé via la commande `ssh-keygen`

##### Quelques chemins utiles
- Emplacement des clés ssh de l'utilisateur : `/home/USR_NAME/.ssh/`
- Emplacement des clés ssh du systeme : `/etc/ssh/ `
- Fichiers de configuration :
	- Comportement du client : `/etc/ssh/ssh_config`
	- Comportement du serveur : `/etc/ssh/sshd_config`

##### Etapes pour se logger en utilisant les clés privée/publique
 - Se placer dans le dossier .ssh du client `/home/USR_NAME/.ssh`
 - Generer la paire clé publique/privée via la commande `ssh-keygen`
 - Copier le contenu de la clé publique generée : `cat id_rsa.pub`
 - Se connecter au serveur en utilisant son mot de passe : `ssh user@ip_address`
 - Se placer dans le dossier .ssh de l'utilisateur avec lequel on s'est connecté `/home/USR_NAME/.ssh`
 - Ouvrir le fichier authorized_keys avec editeur de texte : `nano authorized_keys`
 - Coller la clé publique copiée precedement 	

##### Focntionnement
Lorsque la machine client envoie un requete au serveur qui ouvre une nouvelle session, le serveur ssh envoie un nombre aleatoire crypté en utilisant la clé publique enregistrée du client. Si le client arrive à decrypté ce nombre via sa clé privée alors le serveur lui permet d'ouvrir une nouvelle session.

Par defaut, la connexion ssh reste etablie meme inactive et ceci grace à l'option `TCPKeepAlive yes` presente dans le fichier de configuration de ssh du serveur `/etc/ssh/sshd_config`.
SSH stocke les identifiants de chaque clients qui s'est connecté dans un fichier qui est `./ssh/known_hosts`
#### 3. Authentification par mot de passe

## Debugging

Lorsqu'on rencontre un probleme en utilisant ssh, il peut etre utile d'activer l'option verbose pour voir ce qui se passe `ssh -v user@address_ip`.

On peut aussi lancer le serveur ssh en mode debug afin d'avoir plus de details lorsqu'un client se connecte `sudo /usr/sbin/ssh -d -p 2020`.

#### Quelques erreurs frequentes

##### Probleme de connexion via ssh (connection refused)
- Verifier que ssh est installer correctement sur le serveur et le client.
- Verifier le statut du service de sshd sur le serveur `# systemctl status sshd`.

##### Probleme de connexion via ssh (No route to host)
- Verifier la connexion (on peut utiliser la commande ping pour voir si on se connecte bien).
- Verifier que le serveur est bien lancé.
- Verifier que la connexion n'est pas bloquée par le firewall.
- Pour voir les echanges entre serveur et client `# tcpdump -n -i wlan1 tcp port 22 and host server_ip_address`.


##### Probleme de connexion via ssh (paire clés publique/privée non reconnu)
- Verifier les droits sur le dossier .ssh ainsi que les clés publiques/privées
- clé privée : droit uniquement pour l'utilisateur et absent pour le reste (600)
- dossier .ssh : droits à (700).


## Telnet
Pour se connecter au serveur via Telnet
```
$ telnet
telnet> open -l USR_LOGIN SERVER_IP_ADDRESS
```

