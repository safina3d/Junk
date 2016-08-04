# Samba Server

### Plan
1. Installation
2. Configuration
    


### 1. Installation
Pour installer apache, Il faut lancer la commande :
`# apt-get install samba`


### 2. Configuration
Il faut ajouter un utilisateur avec la commande : `sudo useradd nom_utilisateur`
    
Puis attribuer un mot de passe samba `sudo smbpasswd -a nom_utilisateur`
    
Creer le dossier a partager 
```
    mkdir /srv/dossier
    chown /srv/dossier nom_utilisateur:nom_utilisateur
``` 

Ajouter dans /etc/samba/smb.conf
```
    [NOM_DOSSIER_PUBLIC]
        comment = ceci est un commentaire
        path = /srv/samba-share/zak
        public = yes
        browseable = yes
        writable = yes
        read only = no
        create mask = 0755
        valid users = zak, root
```        