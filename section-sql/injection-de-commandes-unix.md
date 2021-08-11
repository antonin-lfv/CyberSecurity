# Injection de commandes UNIX

## Empiler les commandes

Imaginons que nous aillons un formulaire qui lance une commande Linux avec pour argument la saisie de l'utilisateur. L'idée, pour exploiter ce système, est d'essayer de rentrer d'autres commandes en les alignant sur une seule ligne, ce qui permettra par exemple d'afficher un fichier contenant des données privées.

Pour les commandes Linux, le template est fait ainsi :

```
$ commande1 arg1 opts1 ; commande2 arg2 opts2 ; commande3 arg3 opts3
```

Ainsi, après avoir saisi l'argument attendu du formulaire, on ajoute une autre commande. Si le formulaire demande une IP à ping, alors on pourrait essayer de rentrer la commande :

```
127.0.0.1 ; cat index.php
```

Si ce type de faille n'est pas pris en compte, alors on pourrait récupérer des données confidentielles comme des mots de passe.





