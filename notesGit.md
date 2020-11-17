ajouter des fichiers : $ git add fichier(s)

supprimer des fichiers : $ git rm fichier(s)

→ supprime effectivement les fichiers, et note la suppression dans l'INDEX; il faut valider avec git commit

déplacer/renommer des fichiers : $ git mv source(s) destination

→ déplace ou renomme effectivement les fichiers; le note dans l'INDEX; il faut valider avec git commit

NB: le renommage d'un fichier est enegistré par git comme une suppression+ajout

ça peut se voir avec : $ git log --summary

on peut demander à git de chercher à « deviner » s'il y a eu renommage avec : $ git log --summary -M

Fichier .gitignore ...

pour ignorer des fichiers, par exemple : fichiers temporaires (*~, *.bak);

des fichiers confidentiels (mot de passe);

résultat de compilation (*.o, *.class)

on peut les lister dans un fichier appelé .gitignore

détails dans : $ man gitignore

NB: en général, le fichier .gitignore est lui enregistré dans le dépôt

mettre du travail de côté...

commande : $ git stash

→ toutes les modifications en cours sont rangées dans un coin

on peut consulter avec : $ git stash show

éventuellement : $ git stash show -p

pour reprendre ce qui a été mis de côté : $ git stash pop

pour supprimer sans reprendre : $ git stash drop

voir la liste complète avec : $ git stash list

identifiant de la forme stash@{1}

les branches...

pour voir les branches avec gitk : $ gitk --all

exemple : $ git branch experimental

→ crée une nouvelle branche appelée « experimental »

lister les branches : $ git branch

```shell
$ git branch
  experimental
* main
```

​	