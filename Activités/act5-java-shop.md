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
Testez votre classe dans votre `main`, pour demander son nom à l'utilisateur.

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

Faisons donc `askInt`, qui fait la même chose pour des entiers, afin de demander à l'utilisateur son âge !

> [!Caution]
> Pour le moment, effectuez SOIT le test du nom, SOIT le test de l'âge, mais pas les deux.

<details>
<summary>Proposition de solution</summary>

```java
public int askInt(String question) {
    int r = 0;
    System.out.println(question);
    Scanner sc = new Scanner(System.in);
    r = sc.nextInt();
    sc.close();
    return r;
}

// in main
int r = cli.askInt("Age ?");
System.out.println("Age= "+String.valueOf(r));
```

</details>


# 4. Réusage

Testez de demander le nom, puis l'âge... Que se passe-t-il ?

Cette erreur survient parce que l'on clôt le flux d'entrée.
Il faudrait ne pas le clore, pour commencer, mais aussi, tant qu'à faire, utiliser un unique `Scanner`, qu'on ne rouvrirait pas à chaque fois.

Utilisez donc le constructeur de votre classe CLI pour ouvrir le `Scanner`, que vous allez utiliser dans vos différentes méthodes.
Profitez-en pour tester vos procédures dans une boucle, qui continuera tant que l'âge saisi ne sera pas égal à 0.

<details>
    <summary>Proposition de solution</summary>

```java
public class CLI {
    private Scanner scanner;
    public CLI() {
        this.scanner = new Scanner(System.in);
    }

    public String askString(String question) {
        String r = "";
        System.out.println(question);
        r = this.scanner.nextLine();
        return r;
    }

    public int askInt(String question) {
        int r = 0;
        System.out.println(question);
        r = this.scanner.nextInt();
        return r;
    }
}

// fichier principal

class TestApp {
    public static void main(String[] args) {
        CLI cli = new CLI();
        boolean co = true;
        do {
            String name = cli.askString("Quel est votre nom ?");
            System.out.println("Hello, "+name);
            int r = cli.askInt("Age ?");
            System.out.println("Age= "+String.valueOf(r));
            co = (r > 0);
        } while(co);
    }
}

```

</details>

Que constatez-vous ? Cela peut arriver car `nextInt`, contrairement à `nextLine`, ne "consomme" pas le saut de ligne, qui se retrouve comme parasitant le prochain appel à `nextLine`. Comment résoudriez-vous cela ?

<details>
    <summary>Proposition de solution</summary>

```java
public int askInt(String question) {
    int r = 0;
    System.out.println(question);
    r = this.scanner.nextInt();
    this.scanner.nextLine(); // remove any trailing EOL
    return r;
}
```

</details>

On peut toujours créer plusieurs instances de la classe `CLI`, ce qui pourrait poser des soucis de conflits (puisqu'on se base sur le même flux), et semble peu logique : une seule instance de `CLI` devrait exister à un instant T. Comment résoudriez-vous ce problème ?

## 0.6 Affichage

Nous allons maintenant rajouter une méthode d'affichage, `CLI print(String txt[, boolean eol = true])`. Celle-ci affichera `txt`, avec un saut de ligne si l'argument `eol` est fourni à `true` (ou s'il est omi). Cette méthode renverra l'objet en lui-même, ce qui permettra de chaîner les appels (ex : `cli.print(a, false).print(b, false).print(c);` ).

<details>
    <summary>Proposition de solution</summary>

```java
public CLI print(String txt) {
    return this.print(txt, true);
}

public CLI print(String txt, boolean eol) {
    if(eol) {
        System.out.println(txt);
    } else {
        System.out.print(txt);
    }
    return this;
}
```

</details>

## 0.7 Saisie d'un décimal

Ajoutez une méthode pour la saisie d'un décimal. Testez vos méthodes dans votre `main`.

> [!Warning]
> Rencontrez-vous des problèmes pour détecter un point ET une virgule ?
> C'est normal, [il existe des solutions](https://stackoverflow.com/questions/42332273/scanning-doubles-when-i-separate-decimals-with-comma-or-dot) mais à vous de trouver ce qui vous convient !

# 1. Application Produit

En se basant sur votre conception dans l'[activité 2](https://github.com/esirem-chassel/3aa-progadv/blob/main/Activit%C3%A9s/act2-uml-basics.md), commencez à développer votre application.
Il est conseillé de créer un nouveau projet dédié, et de reprendre votre classe CLI du projet précédent.

## 1.1 Classe Produit

### 1.1.1

Créez une classe `Product` répondant au diagramme UML de la classe Produit.

<details>
    <summary>Proposition de solution</summary>

```java
public class Product {
    private String name;
    private double price;
    public Product(String name) {
        this.name = name;
    }

    public Product setPrice(double price) {
        this.price = price;
        return this;
    }

    public String getName() {
        return this.name;
    }

    public double getPrice() {
        return this.price;
    }
}
```

</details>

> [!Caution]
> Vous remarquerez que j'ai changé le nom de `Produit` vers `Product`.
> Cette étape peut se produire avant ou après la modélisation UML à votre convenance; j'ai tendance néanmoins à vous conseiller de réaliser la traduction durant la modélisation.


Effectuez un test de création dans votre classe principale de deux objets Product de même nom, que vous comparerez (et afficherez le résultat).

<details>
    <summary></summary>

```java
Product p1 = new Product("A");
Product p2 = new Product("A");
System.out.println(p1 == p2);
```

</details>

Que constatez-vous ?

## 1.1.2

En plus de créer votre classe Produit tel que demandé auparavant, créez une méthode `String toString()` qui renverra l'objet sous la forme `<nom> (<prix>)`.

> [!Tip]
> Une méthode `String toString()` en Java est quelque peu magique : elle sera appellée automatiquement dès que vous essayerez de "convertir" votre objet en chaîne de caractères,
> y compris, par exemple, quand vous tenterez de l'afficher en console !

<details>
    <summary>Proposition de solution</summary>

```java
public class Product {
    private String name;
    private double price;
    public Product(String name) {
        this.name = name;
    }

    public Product setPrice(double price) {
        this.price = price;
        return this;
    }

    public String getName() {
        return this.name;
    }

    public double getPrice() {
        return this.price;
    }
    
    @Override
    public String toString() {
        return this.getName()+" ("+String.valueOf(this.getPrice())+")";
    }
}
```

</details>

## 1.2 Classe Application

### 1.2.1 Création produit

La classe `Application` contient notemment une liste de produits.
Créons un tableau simple pour commencer. Dans votre constructeur de la classe `Application`, initialisez donc votre tableau :
```java
private Product[] inventory;
public Application() {
    this.inventory = new Product[]{};
}
```

Créez la méthode `Product createProduct(String name)`, qui va :
- initialiser un nouveau produit
- ajouter ce produit dans le tableau `inventory`
- renvoyer le produit créé

Quel problème rencontrez-vous ?

Eh oui ! En Java, comme en C++, la taille des tableaux est fixé à la compilation. Heureusement, Java, comme C++, dispose de structures dédiées pour gérer des tableaux à taille dynamique.
Une classe qui permet de faire cela est l'`ArrayList`. Ces structures génériques (qui existent aussi de manière similaire en C++ !) prennent un "paramètre" indiquant le type d'objet stocké, via des chevrons, comme ceci : `ArrayList<int>` (pour une liste d'entiers).

> [!Note]
> `ArrayList` est une classe contenue dans le package `java.util`. Il vous faut donc `import java.util.ArrayList`.

Modifions donc notre classe `Application` pour stocker un `ArrayList` de `Product`s.

<details>
    <summary>Proposition de solution</summary>

```java
public class Application {
    private ArrayList<Product> inventory;

    public Application() {
        this.inventory = new ArrayList<Product>();
    }

    Product createProduct(String name) {
        Product p = new Product(name);
        this.inventory.add(p);
        return p;
    }
}
```
</details>


> [!Warning]
> Ceci (l'usage d'`ArrayList` et non d'un array basique) est un détail d'implémentation, ce qui signifie que c'est un élément propre au langage.
> Vous ne devez donc PAS le faire apparaître dans votre UML. Ce n'est pas une modification du système, mais un détail d'implémentation lié au langage.

### 1.2.2 Tout Objet

Nous allons tester, et voir un effet du "tout objet" adopté par Java.

Créez donc une méthode `String toString()` dans `Application` comme suit :

```java
public String toString() {
    String r = "";
    for(Product p: this.inventory) {
        r += p.toString() + "\n";
    }
    return r;
}
```

Dans votre `main`, ajouter l'instanciation d'un nouvel objet `Application`, et créez `p1` et `p2` respectivement de nom "ABC" et "DEF".
Modifiez ensuite le prix de `p1`, et affichez (grâce à `toString`) votre objet `Application`. Que constatez-vous ?

> [!Important]
> Java passe TOUS les arguments par valeur par défaut.
> Mais comme la "valeur" d'un objet est en réalité sa référence en mémoire (cf. section 1.1.1 ), ces échanges d'objets permettent d'accéder à l'objet réellement stocké.
> Modifier le contenu d'un objet est nommé "mutation"; on dit qu'un objet est muté.

### 1.2.3 Méthode `findProduct`

Implémentez la méthode recherchant un produit à partir de son nom. Si le produit n'est pas trouvé, on renverra la référence vers rien : `null`.

<details>
    <summary>Proposition de solution</summary>

```java
Product findProduct(String name) {
    Product r = null;
    for(Product p: this.inventory) {
        if(p.getName().equals(name)) {
            r = p;
        }
    }
    return r;
}
```

</details>

### 1.2.4 Méthode `setProductPrice`

Cette méthode avait été ajouté pour permettre de directement modifier le prix d'un produit.
Dans la conception, nous avons indiqué prendre en paramètre un produit et un prix. Implémentez la méthode.
Que remarquez-vous ?

Modifiez votre méthode pour qu'elle prenne en paramètre un nom de produit et un prix à la place.

<details>
    <summary>Proposition de solution</summary>

```java
Application setProductPrice(String name, double price) {
    Product p = this.findProduct(name);
    if(p != null) {
        p.setPrice(price);
    }
    return this;
}
```

</details>

### 1.2.5 Garde-fou : homonyme

Nous avons câblé de plus en plus de méthodes sur le fait que le nom d'un produit est unique. Est-ce le cas actuellement ?

Nous allons ajouter une exception lors de la création d'un produit : si celui-ci existe déjà, nous refuserons la création et jetterons une exception.

Créez une classe `ProductDuplicateException`, qui héritera de la classe de base de Java `Exception`, la classe de base des exceptions.

Le constructeur de votre classe prendra le nom du produit en paramètre, et appellera le constructeur parent avec le message "The Product XXX already exists" (en remplaçant évidemment XXX par le nom du produit).

> [!Tip]
> Rappel : l'appel à la méthode parente se fait via `super`.

<details>
    <summary>Proposition de solution</summary>

```java
public class ProductDuplicateException extends Exception {
    public ProductDuplicateException(String productName) {
        super("The product "+productName+" already exists");
    }
}
```

</details>

Maintenant, jetez cette exception dans votre méthode `createProduct` si un produit de ce nom existe déjà.
Que remarquez-vous, via votre IDE ?
Choisissez l'option d'ajouter l'instruction `throws`.

> [!Tip]
> Rappel : on jette une exception avec le mot-clef `throw`, en initialisation un nouvel objet de type `Exception`.

<details>
    <summary>Proposition de solution</summary>

```java
Product createProduct(String name) throws ProductDuplicateException {
    if(null != this.findProduct(name)) {
        throw new ProductDuplicateException(name);
    }
    Product p = new Product(name);
    this.inventory.add(p);
    return p;
}
```

</details>

Si vous tentez maintenant d'utiliser la méthode `createProduct`, vous remarquerez que votre IDE demande à ce que vous interceptiez l'exception, comme avant.
Si vous ne le faites pas, votre programme peut planter sans aucun contrôle, et ceci déplaît à Java.
Saisissez le code suivant, et testez. Que constatez-vous ?

```java
public static void main(String[] args) {
    Application app = new Application();
    try {
        Product p1 = app.createProduct("A");
        Product p2 = app.createProduct("A");
        Product p3 = app.createProduct("B");
    } catch(Exception e) {
        e.printStackTrace();
    }
    System.out.println(app);
}
```

# 2. Classe Date

Une classe nécessaire pour la suite est la classe Date.
Mais cette classe existe déjà dans Java !

Testez le code suivant :

```java
public static void main(String[] args) {
    Date d = new Date(2026, 5, 10);
    System.out.println(d);
}
```

Vous devriez constater deux anomalies.

<details>
    <summary>A voir</summary>

Pour commmencer, le constructeur `Date` prenant ces paramètres en compte est *déprécié*.
Cela signifie qu'il sera retiré, tôt ou tard, de Java.
Son usage est donc logiquement déconseillé.
Généralement, il y a de bonnes raisons à ces dépréciations, et on peut constater laquelle ici : l'année indiquée est... 3926 ??
La classe `Date` se base en effet sur une considération de `Date` commençant en 1900, pour permettre des dates de 1900 à... 2000.
Cette classe a également de nombreux bugs et soucis au fil de l'ajout de fonctionnalités (comme les TZ ou la DST), qui ont amené à la création d'alternatives.

</details>

Contrairement à la recommandation, utilisons à la place [la "nouvelle" alternative](https://www.oracle.com/technical-resources/articles/java/jf14-date-time.html) : `LocaleDate`.

<details>
    <summary>Proposition de solution</summary>

```java
LocalDate d = LocalDate.of(2026, 5, 10);
System.out.println(d); // 2026-05-10
```

</details>




