# Injection de commandes UNIX

## Empiler les commandes

Imaginons que nous aillons un formulaire qui lance une commande Linux avec pour argument la saisie de l'utilisateur. L'idée, pour exploiter ce système, est d'essayer de rentrer d'autres commandes en les alignant sur une seule ligne, ce qui permettra par exemple d'afficher un fichier contenant des données privées.

Pour les commandes Linux, il existe 3 moyens :

#### 1. Avec « ; » qui exécute les commandes sans se soucier du succès des commandes précédentes

```bash
$ commande1 arg1 opts1 ; commande2 arg2 opts2 ; commande3 arg3 opts3
```



#### 2. Avec « && » qui exécute les commandes seulement si les précédentes ont réussi

```
$ commande1 arg1 opts1 && commande2 arg2 opts2 && commande3 arg3 opts3
```



#### 3. Avec « \|\| » qui exécute les commandes seulement si les précédentes ont échoué 

```
$ commande1 arg1 opts1 || commande2 arg2 opts2 || commande3 arg3 opts3
```



Ainsi, après avoir saisi l'argument attendu par le formulaire, on ajoute une autre commande avec le moyen qui vous convient. Si le formulaire demande une IP à ping, alors on pourrait essayer de rentrer la commande :

```
127.0.0.1 ; sort index.php 
```

Cette commande va ainsi afficher index.php sans que le fichier soit compilé. 

De la même manière, si on devait rechercher un mot clé en particulier dans un fichier, comme un flag, alors on pourrait utiliser la commande [grep](http://www.linux-france.org/article/man-fr/man1/grep-1.html) :

```
127.0.0.1 ; grep "flag" index.php 
```

{% hint style="success" %}
Si ce type de faille n'est pas pris en compte sur l'application, alors on pourrait récupérer des données confidentielles comme des mots de passe.
{% endhint %}





