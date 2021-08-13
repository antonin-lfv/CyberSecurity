# 💉 - Injections SQL

L’injection SQL, ou SQLi, est un type d’attaque sur une application web qui permet à un attaquant d’insérer des instructions SQL malveillantes dans l’application web, pouvant potentiellement accéder à des données sensibles dans la base de données ou détruire ces données.l’injection SQL a été découverte pour la première fois par Jeff Forristal en 1998.



## Techniques d'injection SQL

Les objectifs des injections SQL peuvent être multiples:

* accéder à des données auxquelles on ne devrait pas avoir accès,
* modifier des données,
* effacer des données,
* lire/écrire sur le système de fichier,
* exécuter des commandes systèmes.

Par exemple, dans le cas d'une requête SELECT, l'objectif peut être de :

* modifier les critères de recherche, par exemple contourner une authentification,
* ajouter aux résultats des informations se trouvant dans d'autres tables pour accéder à des données auxquelles on ne devrait pas avoir accès.

Une injection SQL dans une requête UPDATE peut permettre de :

* modifier des données avec des valeurs choisies
* modifier de manière plus globale les informations \(Modification des clauses WHERE\)

Une requête DELETE peut être modifiée à des fins de dénis de service.



## Exploitation des messages d'erreurs

L'exploitation d'une faille par injection SQL peut être facilitée par la présence de message d'erreur dans l'application. Cherchons à rendre la requête SQL invalide dans le cas d'un site Web demandant une authentification.

```php
$sql="SELECT login FROM users WHERE login='".$login."' AND pass='".$pass."'";
$result = mysql_query ($sql) or die ("Invalid query".mysql_error());
if($row = mysql_fetch_array ($result))
{
  print "Identified as ".htmlspecialchars($row['login']);
  ...
}
```

Suite à la saisie d'une simple quote dans ce formulaire, la requête devient invalide et provoque l'affichage d'un message d'erreur:

```text
You have an error in your SQL syntax; check the manual that corresponds to your
MySQL server version for the right syntax to use near '''' AND pass=''' at line 1
```

Ce script est donc vulnérable et nous apprenons que le serveur SQL est MySQL. Mais dans l'hypothèse où le paramètre se retrouve entre double-quote, il faut aussi tester l'utilisation de double-quote. Cependant le serveur où le script s'exécute peut être configuré pour ne pas afficher de message d'erreur \(Mettre display\_errors = Off dans php.ini par exemple\) ou bien utiliser des try/catch \(C\#, Java, PHP5 ...\) pour gérer les exceptions, les anomalies de fonctionnement et donner l'impression d'un fonctionnement normal de l'application.



### Dans cette rubrique : 

{% page-ref page="injection-par-union.md" %}

{% page-ref page="injection-de-commandes-unix.md" %}



