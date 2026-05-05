# 0. Contexte

L'objectif ici est de développer sous Java l'application préparée dans l'activité 2 (UML basics).
Cette activité est guidée, sous un contexte fixé.

## 0.1 Installation et configuration

> [!Note]
> Toute cette partie peut être suivie directement sur la [documentation de démarrage pour Java sur Visual Studio Code](https://code.visualstudio.com/docs/java/java-tutorial).
> Attention, cependant, à bien lire et comprendre avant de réaliser des opérations.
> Si la lecture et la compréhension ne sont pas votre fort, ne suiviez pas la documentation de VSC, mais continuez sur ce dépôt.

### 0.1.1 Installation d'un JDK

Avant toute chose, ouvrez un powershell (ou bash/dash sous Linux) et tapez `javac --version`. Si vous obtenez quelque chose comme `javac 21.0.1` ou supérieur, félicitations ! Vous avez déjà un JDK installé, donc vous pouvez passer à l'étape 0.1.2.

Si vous n'avez pas de JDK installé, il est temps d'en [télécharger](https://www.oracle.com/java/technologies/downloads/) et installer un, notemment via le site d'Oracle : [https://www.oracle.com/java/technologies/downloads/](https://www.oracle.com/java/technologies/downloads/) .

### 0.1.2 Visual Studio Code et Java Extension Pack

Ouvrez Visual Studio Code, sur une fenêtre vide. Créez un fichier `TestApp.java` dans un dossier nommé `TestApp`, et insérez le code minimaliste suivant :

```java
class TestApp {
    public static void main(String[] args) {
        System.out.println("Hello World");
    }
}
```

Enfin, dans Visual Studio Code, ouvrez le projet via "`File` > `Open Folder`" et ouvrez le dossier `TestApp`.
Visual Studio Code va détecter le type de projet et vous proposer d'installer `Extension Pack for Java` - acceptez, et tout va s'installer tranquillement.

> [!Tip]
> Si vous souhaitez installer vous-même `Extension Pack for Java`, vous pouvez l'installer vous-même via le marketplace (onglet Extensions de Visual Studio Code).
> Assurez-vous bien d'installer `Extension Pack for Java` ! Celui-ci devrait être publié par Microsoft.

## 0.2 Structure d'un projet

### 0.2.1 Visual Studio Code

Dans Visual Studio Code, un projet Java est automatiquement détecté tel quel.
Vous pouvez vérifier en regardant si un encart "Java Projects" est disponible (par défaut sous Explorer/Outline/Timeline, à gauche de l'écran de VSC) et si votre projet est bien dedans.
Les fichiers compilés (car Java est un langage compilé) sont envoyés dans un `Output Path` qui peut être configuré en réalisant un clic droit sur le nom de Java Project puis `Configure Classpath`.

### 0.2.2 bin / src

Souvent, on apprécie voir à la fois les fichiers source et les fichiers compilés.
Pour cela, on peut créer deux dossiers à la racine du project : `src` et `bin`.
On configure ensuite le `classpath` pour ajouter le `src` en `Source Path`, et `bin` en `Output Path`.
Attention en ce cas à retirer `.` des `Source Path`.

> [!Warning]
> Le dossier bin et son contenu ne doivent **jamais** être envoyés sur Git.
> Vous connaissez le `.gitignore`, utilisez-le !


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

Créez donc une méthode `askString` qui prendra en paramètre un `String`, affichera cette chaîne, et attendra une saisie utilisateur avant de renvoyer cette saisie utilisateur.
Testez votre classe dans votre `main`.

<details>
<summary>Proposition de solution</summary>

```java
import java.util.Scanner;

public class CLI {
    public String askString(String question) {
        String r = "";
        System.out.println(question);
        Scanner sc = new Scanner(System.in);
        r = sc.nextLine();
        sc.close();
        return r;
    }
}

// ... dans le main
CLI cli = new CLI();
String name = cli.askString("Quel est votre nom ?");
System.out.println("Hello, "+name);

```

</details>

# 3. Pour les entiers

Faisons donc `askInt`, qui fait la même chose pour des entiers !


# 4. Avec vérifications

Modifions 
