# 0. Contexte

L'objectif ici est de développer sous Java l'application préparée dans l'activité 2 (UML basics).
Cette activité est guidée, sous un contexte Eclipse.

## 0.1 Installation Eclipse

Le téléchargement d'Eclipse est disponible sur [leur site web](https://www.eclipse.org/downloads/packages/).

L'installation effectue toutes les installations tierces nécessaires, y compris un JRE.

> [!Note]
> Même si Eclipse installe un JRE et, possédant ses propres outils de compilation, n'a pas besoin d'un JDK pour fonctionner,
> vous pourriez avoir besoin d'utiliser d'autres outils, comme des gestionnaires de packaging (Maven..).
> Dans ce genre de cas, l'installation d'un JDK pourrait être nécessaire.
> De même, vous pouvez, dans Eclipse, choisir quelle JRE/JDK utiliser, et quelle version !

## 0.2 Création d'un projet

Dans Eclipse, effectuez un "New > New Java Project" pour commencer un nouveau projet Java.
Vous pouvez sélectionner un nouveau JRE, si celui par défaut ne vous convient pas.
Laissez les éléments par défaut (default location, Create separate...) et ne cochez ni le working set, ni le module.

## 0.3 Création de la première classe

Dans votre projet, dans `src`, créez une nouvelle classe Java.

Laissez tous les éléments par défaut (y compris le package, qui doit être vide, donc "default" sur le coté), et nommez-la idéalement comme votre projet (attention, pas d'espace ou de caractères spéciaux !). Vous pouvez cocher "public static void main..." pour qu'Eclipse vous génère automatiquement le code nécessaire à votre première classe "principale".


# 1. Classe principale

En Java, la classe principale est d'un nom libre, mais celle-ci doit comporter une méthode statique `main`, idéalement sous le prototype `public static void main(String[] args)`.

Seule une classe avec ce main doit exister dans votre projet. C'est votre classe principale, qui sera lancée lors de l'exécution de votre programme.

> [!Note]
> Dans la liste des bonnes pratiques forcées par Java, la règle de "1 classe = 1 fichier".
> Vous ne pouvez définir plus d'une classe par fichier.
> Il existe des "astuces" pour créer des classes internes, mais ceci est un élémennt pour des besoins très spécifiques.

# 2. Classe de manipulation de flux d'entrée

La saisie utilisateur peut être lue via le flux `System.in`.
Différents utilitaires permettent d'interagir avec ce flux, dont le plus connu est `Scanner` :

```java
Scanner sc = new Scanner(System.in);
int i = sc.nextInt(); // obtient le prochain entier lu
```

Nous allons commencer par réaliser une abstraction de ces méthodes, en utilisant une classe `CLI` chargé de l'interaction avec l'utilisateur via la console.
