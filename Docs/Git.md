git reset Head
git checkout -- nomFichier : revenir a la derniere version commité
git commit -a -m "description commit" : add + commit en une seule commande 
git reset --soft HEAD^ : annuler le dernier commit et repasse en staging
git reset --soft HEAD^^ : annuler les deux derniers commit et repasse en staging
git commit --amend -m "description commit": ajouter les fichiers en staging  au dernier commit

git remote add <nomDuRemote> <url> : ajouter un nom de repository à distance 
ex: git remote add origin https://github....
git remote rm <nomDuRemote> : pour supprimer un remote 

git remote -v : afficher le remote repo lié au repo local

git push -u <nomDuRemote> <branche>

// Branches

git branch: Afficher la liste des branches locales
git branch -r: Afficher la liste des branches du remote
git branch <nomBranche> : Creer une branche mais reste sur la branche en cours
git checkout <nomBranche>: Pour changer de branche
git checkout -b <nomBranche>: Permet de creer une branche et de basculer sur cette derniere
git branch -d <nomBranche>: Pour supprimer une branche locale
git branch -D <nomBranche>: Forcer la suppression d'une branche locale
git push origin :<nomBranche>: Pour supprimer une branche distante

// Merge
1- Se placer sur la branche qui principal (celle qui va recevoir le merge)
2- Lancer la commande git merge <nomBranche>

Remarque
Si aucun commit n'a été fait sur la branche principale entre temps le merge est facile pour git, on parle de fast-forward.
sinon git effectue un merge recursif.

git remote show <nomDuRemote>: Afficher la liste des branches du remote et voir si elles sont trackées ou pas/


// Tags
git tag: Afficher la liste des Tags