# 0. Contexte

L'objectif ici est de développer sous Java l'application préparée dans l'activité 2 (UML basics).

# 1. Classe principale

main ...

# 2. Classe de manipulation de flux d'entrée

La saisie utilisateur peut être lue via le flux `System.in`.
Différents utilitaires permettent d'interagir avec ce flux, dont le plus connu est `Scanner` :

```java
Scanner sc = new Scanner(System.in);
int i = sc.nextInt(); // obtient le prochain entier lu
```

Nous allons commencer par réaliser une abstraction de ces méthodes, en utilisant une classe `CLI` chargé de l'interaction avec l'utilisateur via la console.
