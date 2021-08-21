---
description: >-
  La faille XSS est l'une des plus populaires et dangereuses. Elle consiste à
  injecter du code html et javascript dans une page web.
---

# Faille XSS - injection HTML/JS

Imaginons que ce qui suit est un endroit pour envoyer un message via une page de contact. En essayant de rentrer le code suivant, on peut détecter une faille XSS : 

```text
<script>alert("faille XSS");</script>
```

Le code entre les balises script est donc un code javascript qu'on a placé et qui s'exécute directement. Ce code n'est pas persistant : Il n'est pas stocké dans une base de données \(comme le serait un pseudo\).  
Par contre si il avait été persistant, tous les autres utilisateurs auraient vu ce code s'exécuter.  
**alert** va juste afficher un message, mais ça aurait pu être pire, comme par exemple une "iframe" qui remplace carrément toute la page par un autre site. Pour contrer cela, il faut donc toujours filtrer les données entrées par les utilisateurs.   
Utilisez donc les fonctions **htmlspecialchars** ou **htmlentities** en PHP.

