# 💉 - Injections SQL

L’injection SQL, ou SQLi, est un type d’attaque sur une application web qui permet à un attaquant d’insérer des instructions SQL malveillantes dans l’application web, pouvant potentiellement accéder à des données sensibles dans la base de données ou détruire ces données.l’injection SQL a été découverte pour la première fois par Jeff Forristal en 1998.

### Les objectifs des injections SQL peuvent être multiples:

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



### Dans cette rubrique : 

{% page-ref page="injection-par-union.md" %}

{% page-ref page="injection-de-commandes-unix.md" %}



