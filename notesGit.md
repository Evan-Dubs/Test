# Systèmes de gestion de versions : Git

## TP #1 : Introduction

- Création d'un dépôt sur le [GitLab de l'IUT](https://gitlab.iut-bm.univ-fcomte.fr/) [GitHub](https://github.com/), découverte de l'interface.

- Premier commit.

- Copie du dépôt : commande git clone.

  ```
  $ git clone https://gitlab.iut-bm.univ-fcomte.fr/LOGIN/DEPOT.git
  $ git clone git@gitlab.iut-bm.univ-fcomte.fr:LOGIN/DEPOT.git
  ```

   Avec GitHub:

  ```
  $ git clone https://github.com/LOGIN/DEPOT.git
  $ git clone git@github.com:LOGIN/DEPOT.git
  ```

- Interface graphique : gitk

- Efficacité ? Exemple avec le noyau Linux…

- Manuel : man, git help, [manuel en ligne](https://git-scm.com/docs)

  ```
  $ man git clone
  $ git help clone
  ```

- Configuration minimale :

  ```
  $ git config --global user.name "Prénom Nom"
  $ git config --global user.email "user‌@example.com"
  $ git config --global color.ui auto
  ```

## TP #2 : Commandes de base

- Consulter l'état de l'espace de travail :

  ```
  $ git status
  $ git diff
  $ git diff --word-diff
  $ git diff --cached
  ```

- Modifier ou ajouter un ou des fichiers :

  ```
  $ git add <fichier...>
  $ git commit
  $ git commit -a
  $ git commit -m "Message..."
  ```

- Revoir les commits :

  ```
  $ gitk
  $ git show
  $ git log
  $ git log -p
  $ git log --stat --summary
  $ git log --oneline
  ```

- Envoyer les commits vers le serveur :

  ```
  $ git push
  ```

- Recevoir les commits depuis le serveur :

  ```
  $ git fetch
  $ git pull
  ```

#### Exercices

1. *Pull simple*
   Faire un commit par l'interface web, puis un « git pull » pour l'intégrer localement.
2. *Fusion automatique*
   Faire un commit par l'interface web. Faire un commit en local **sur un autre fichier**.
   Tenter un « git push ». Recommencer avec « git pull » suivi de « git push ».
3. *Situation de conflit*
   Comme l'exercice 2, mais en modifiant (différemment) les mêmes parties d'un fichier.
   Un conflit apparaît au moment du « git pull », le résoudre.

## TP #3 : Commandes de base (suite) ; les branches

- Ajouter, supprimer des fichiers:

  ```
  $ git add <fichier...>
  $ git rm <fichier...>
  $ git mv <source...> <destination>
  ```

- Fichier .gitignore

- Mettre du travail de côté:

  ```
  $ git stash
  $ git stash show
  $ git stash show -p
  $ git stash pop
  $ git stash drop
  $ git stash list
  ```

- Les branches (NB: dans les versions récentes, la branche *master* s'appelle peut-être *main*):

  ```
  $ git branch experimental
  $ git branch
    experimental
  * master
  $ git checkout experimental
  # éditer des fichiers
  $ git commit -a
  $ git checkout master
  # éditer des fichiers
  $ git commit -a
  $ git merge experimental
  # conflits ?
  $ git diff
  # éditer les fichiers
  $ git commit -a
  $ gitk --all
  $ git branch -d experimental
  $ git branch -D mauvaise-idee
  ```

## TP #4 : Désigner un commit ; autres commandes utiles

- Différentes manières de désigner un commit (cf. [gitrevisions(7)](https://git-scm.com/docs/gitrevisions) pour les détails):

  ```
  $ git show ca6601f...                # identifiant, peut être abrégé
  $ git show experimental              # nom de branche
  $ git show HEAD                      # dernier commit
  $ git show <commit>^                 # parent du commit donné (ex. HEAD^)
  $ git show <commit>^^                # grand-parent du commit
  $ git show <commit>~4                # grand-grand grand-parent du commit
  $ git show <commit>^1                # 1er parent
  $ git show <commit>^2                # 2ème parent (ex. lors d'un merge)
  ```

- Étiqueter un commit:

  ```
  $ git tag v2.5 1b2e1d63ff
  ```

- Quelques exemples:

  ```
  $ git diff v2.5 HEAD
  $ git log v2.5..v2.6
  $ git log v2.5..
  $ git log v2.5.. Makefile
  $ git branch stable v2.5
  $ git show v2.5:Makefile
  ```

  

### Autres commandes utiles: 

- Rechercher un motif:

  ```
  $ git grep "hello"
  $ git grep "hello" v2.5
  ```

- Voir qui a modifié les lignes d'un fichier:

  ```
  $ git blame <fichier>
  ```

- Retrouver le commit à l'origine d'un problème (bug):

  ```
  $ git bisect start                   # démarrer une nouvelle recherche
  $ git bisect bad                     # la version courante n'est pas bonne
  $ git bisect good v2.6.13-rc2        # la version v2.6.13-rc2 était bonne
  # une version intermédiaire est extraite, il faut la tester
  $ git bisect good                    # si la version est bonne
  $ git bisect bad                     # si la version n'est pas bonne
  $ git bisect skip                    # pour tester une autre version
  # recommencer jusqu'à trouver le commit fautif
  $ git bisect reset                   # pour réinitialiser le dépôt à la fin
  ```

  [En suivant ce lien, vous trouverez un exemple permettant de mettre en pratique cette procédure](https://delicious-insights.com/fr/articles/git-bisect/). 

- Annuler un commit:

  ```
  $ git revert <commit>
  ```

## TP #5 : Soigner ses commits

exemples de commits sur de vrais projets, par exemple sur https://git.kernel.org/

exemple typique de commit : https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=d3ab78858f1451351221061a1c365495df196500

*pour faire des commits propres...*

- Sélectionner les morceaux à mettre dans un commit

  ```
  $ git add -p [<fichier>]
  ```

  → git présente les différentes modifications, et demande s'il faut les intégrer à l'index puis git commit pour valider

- Compléter le dernier commit

  ⇒ ATTENTION

  **il ne faut pas modifier des commits qui ont déjà été partagés (via un git push par exemple)**

  ```
  $ git commit --amend
  ```

- Supprimer des commits

  ```
  $ git reset --soft  <commit>         # ne touche pas aux fichiers, ni à l'index
  $ git reset --mixed <commit>         # l'index est réinitialisé
  $ git reset --hard  <commit>         # les fichiers et l'index sont aussi réinitialisés
  ```

  → <commit> est le dernier commit à conserver

  Exemples

  ```
  $ git reset --soft HEAD^             # supprime le dernier commit
  $ git reset --hard HEAD~3            # supprime les 3 derniers commits, et réinitialilse les fichiers
  $ git reset --hard                   # HEAD par défaut, supprime toutes les modifications non enregistrées
  ```

- Modifier les derniers commits (éventuellement avec *origin/main*)

  ```
  $ git rebase -i origin/master
  $ git rebase -i origin/main
  ```

  → permet de modifier les commits depuis origin/main, càd depuis le dernier commit sur le dépôt distant (github)

  → permet de modifier les commits locaux

  *modifier* : éditer le message; modifier le commit; fusionner des commits; supprimer des commits; changer l'ordre de commits; etc.

  exemple : 

  ```
  pick 03aaeb6b7e Declare function 'const'.
  pick 9cfb1e3cb5 Assert that *request != MPI_REQUEST_NULL, and remove useless tests.
  pick 75c3f46b3b Fix comments.
  pick f653524eb9 Fix return type for get_maxpid().
  pick 6b8d58b226 Introduce mc::mc_api (pull request 1 -- #349)
  pick 4ec65626ec Fix comment.
  
  # Rebasage de 49966c59fb..4ec65626ec sur 49966c59fb (6 commandes)
  #
  # Commandes :
  #  p, pick <commit> = utiliser le commit
  #  r, reword <commit> = utiliser le commit, mais reformuler son message
  #  e, edit <commit> = utiliser le commit, mais s'arrêter pour le modifier
  #  s, squash <commit> = utiliser le commit, mais le fusionner avec le précédent
  #  f, fixup <commit> = comme "squash", mais en éliminant son message
  #  x, exec <commit> = lancer la commande (reste de la ligne) dans un shell
  #  b, break = s'arrêter ici (on peut continuer ensuite avec 'git rebase --contine')
  #  d, drop <commit> = supprimer le commit
  #  l, label <label> = étiqueter la HEAD courante avec un nom
  #  t, reset <label> = réinitialiser HEAD à label
  #  m, merge [-C <commit> | -c <commit>] <label> [# <uniligne>]
  #          créer un commit de fusion utilisant le message de fusion original
  #          (ou l'uniligne, si aucun commit de fusion n'a été spécifié).
  #          Utilisez -c <commit> pour reformuler le message de validation.
  #
  # Vous pouvez réordonner ces lignes ; elles sont exécutées de haut en bas.
  #
  # Si vous éliminez une ligne ici, LE COMMIT CORRESPONDANT SERA PERDU.
  #
  # Cependant, si vous effacez tout, le rebasage sera annulé.
  #
  # Veuillez noter que les commits vides sont en commentaire
  ```

- Éviter un « merge » lors d'un git pull

  ```
  $ git pull --rebase
  ```

[Un petit flowchart pour illustrer...](http://justinhileman.info/article/git-pretty/)



***

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

→ la branche active (*) est main

on peut changer de branche : $ git checkout experimental

```
$ git checkout  experimental
Basculement sur la branche 'experimental'
$ git branch
* experimental
  main
```

on peut éditer des fichiers; puis commit -a

→ le commit est ajouté à la branche experimental

on peut basculer de nouveau sur « main » : $ git checkout main

on peut encore éditer des fichiers; puis git commit -a

→ ce commit est ajouté à la branche main

là, les deux branches ont divergé

on peut choisir d'intégrer dans main les modifications de experimental : $ git merge experimental

→ régler les conflits éventuels : git diff; éditer les fichiers; git commit -a

on peut maintenant choisir de supprimer la branche experimental : $ git branch -d experimental

on ne peut pas supprimer avec « -d » une branche qui n'a pas été fusionnée

il faut forcer avec « -D ». exemple : $ git branch -D mauvaise-idee

Différentes manières de désigner un commit (cf. [gitrevisions(7)](https://git-scm.com/docs/gitrevisions) pour les détails):

```
$ git show ca6601f...                # identifiant, peut être abrégé
$ git show experimental              # nom de branche
$ git show HEAD                      # dernier commit de la branche en cours
$ git show <commit>^                 # parent du commit donné (ex. HEAD^)
$ git show <commit>^^                # grand-parent du commit
$ git show <commit>~4                # grand-grand grand-parent du commit
$ git show <commit>^1                # 1er parent
$ git show <commit>^2                # 2ème parent (ex. lors d'un merge)
```

Étiqueter un commit:

```
$ git tag v2.5 1b2e1d63ff
```

Quelques exemples:

```
$ git diff v2.5 HEAD
$ git log v2.5..v2.6
$ git log v2.5..
$ git log v2.5.. Makefile
$ git branch stable v2.5
$ git show v2.5:Makefile
```

### Autres commandes utiles: 

Rechercher un motif:

```
$ git grep "hello"
$ git grep "hello" v2.5
Options intéressantes: -p (nom fonction/ligne où mot apparait) et -W (toute la méthode/fonction où le mot appraît)
```

Voir qui a modifié les lignes d'un fichier:

```
$ git blame <fichier>
```

Retrouver le commit à l'origine d'un problème (bug):

```
$ git bisect start                   # démarrer une nouvelle recherche
$ git bisect bad                     # la version courante n'est pas bonne
$ git bisect good v2.6.13-rc2        # la version v2.6.13-rc2 était bonne
# une version intermédiaire est extraite, il faut la tester
$ git bisect good                    # si la version est bonne
$ git bisect bad                     # si la version n'est pas bonne
$ git bisect skip                    # pour tester une autre version
# recommencer jusqu'à trouver le commit fautif
$ git bisect reset                   # pour réinitialiser le dépôt à la fin
```

[En suivant ce lien, vous trouverez un exemple permettant de mettre en pratique cette procédure](https://delicious-insights.com/fr/articles/git-bisect/). 

Annuler un commit:

```
$ git revert <commit>
```

