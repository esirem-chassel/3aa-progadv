# 0. Contexte

L'objectif ici est surtout de s'entraîner en UML.
On va ici imaginer une application de gestion de commande de marchandises pour notre restaurateur favori (quel qu'il soit).

Il faut donc penser :
- aux produits
- à l'état du stock de ces produits
- aux commandes de produits
- à la gestion de l'application elle-même

Nous étendrons ces éléments par la suite.

> [!Note]
> Nous allons utiliser les types "primitifs" suivants : int, double, string, list

# 1. Classe Produit

La classe Produit sera réutilisée un peu partout.
Créez donc cette classe en UML, en gardant en tête les principes de base de l'OO (notemment SOLID).

> [!Tip]
> En UML, on ne distingue PAS référence/pointeur d'une copie.
> Un objet est un objet.
> La seule distinction qui pourrait se faire serait dans la nature des relations;
> une composition utiliserait certainement des objets "pleins" là où une agrégation conserverait des pointeurs.

<details>
<summary>Proposition de solution</summary>

```mermaid
classDiagram
  class Product {
    -string name
    -double price
    +Product(string name)
    +Product setPrice(double price)
    +string getName()
    +double getPrice()
  }
```

</details>

# 2. Classe Application

Notre application va conserver un inventaire de produits, et va gérer l'affichage du menu principal, ainsi que préparer les différentes actions :
- ajout d'un produit
- recherche d'un produit par son nom
- modification du prix d'un produit

Modélisez donc cela en UML (attention à prendre en compte le lien avec `Product` !).

<details>
<summary>Proposition de solution</summary>

```mermaid
classDiagram
  class Product {
    -string name
    -double price
    +Product(string name)
    +Product setPrice(double price)
    +string getName()
    +double getPrice()
  }

  class Application {
    -list~Product~ inventory
    +Product createProduct(string name)
    +Product findProduct(string name)
    +Application setProductPrice(Product product, double price)
  }

  Product "0,n" --* Application
```

</details>

A partir de maintenant, il faudra penser à enrichier votre classe Application des différentes possibilités offertes.

# 3. Commandes

Notre application doit gérer les commandes.
Une commande concerne un seul produit, a une date de commande (obligatoire), une date de livraison (facultatif) ainsi qu'un prix unitaire et une quantité.
On doit pouvoir retrouver une liste de commandes à partir d'une date de commande, de livraison, ou d'un produit.

<details>
<summary>Proposition de solution</summary>

```mermaid
classDiagram
  class Product {
    -string name
    -double price
    +Product(string name)
    +Product setPrice(double price)
    +string getName()
    +double getPrice()
  }

  class Date {
    -int y
    -int m
    -int d
    +Date(int y, int m, int d)
    +int getY()
    +int getM()
    +int getD()
  }

  class Order {
    -Product product
    -int qtty
    -double unitPrice
    -Date orderDate
    -Date deliveryDate
    +Order(Product product, int qtty, double unitPrice, Date orderDate)
    +Order setDeliveryDate(Date deliveryDate)
    +Order setQtty(int qtty)
    +Order setUnitPrice(double unitPrice)
    +Product getProduct()
    +int getQtty()
    +double getUnitPrice()
    +Date getOrderDate()
    +Date getDeliveryDate()
  }

  class Application {
    -list~Product~ inventory
    -list~Order~ orders
    +Product createProduct(string name)
    +Product findProduct(string name)
    +Application setProductPrice(Product product, double price)
    +Order createOrder(Product product)
    +list~Order~ findOrders()
    +list~Order~ findOrdersByDeliveryDate(Date deliveryDate)
    +list~Order~ findOrdersByOrderDate(Date orderDate)
    +list~Order~ findOrdersByProduct(Product product)
  }

  Product "0,n" --* Application : inventory
  Product "1,1" --o Order : product
  Date "1,1" --* Order : order
  Date "0,1" --* Order : delivery
  Order "0,n" --* Application : orders
```

</details>

# 4. Stocks

Actuellement, l'inventaire est factice. Il s'agit en effet d'une liste de produits, sans indication de quantité.
On ne pourrait pas stocker la donnée de quantité dans un produit; en effet, cette donnée est décorrélée de l'identité d'un produit.
Nous allons donc créer une classe d'association entre `Produit` et `Application`, afin de gérer cette liaison.

<details>
<summary>Proposition de solution</summary>

```mermaid
classDiagram
  class Product {
    -string name
    -double price
    +Product(string name)
    +Product setPrice(double price)
    +string getName()
    +double getPrice()
  }

  class Date {
    -int y
    -int m
    -int d
    +Date(int y, int m, int d)
    +int getY()
    +int getM()
    +int getD()
  }

  class Order {
    -Product product
    -int qtty
    -double unitPrice
    -Date orderDate
    -Date deliveryDate
    +Order(Product product, int qtty, double unitPrice, Date orderDate)
    +Order setDeliveryDate(Date deliveryDate)
    +Order setQtty(int qtty)
    +Order setUnitPrice(double unitPrice)
    +Product getProduct()
    +int getQtty()
    +double getUnitPrice()
    +Date getOrderDate()
    +Date getDeliveryDate()
  }

  class Stock {
    -Product product
    -int qtty
    +Stock(Product product, int qtty)
    +Stock setQtty(int qtty)
    +int getQtty()
  }

  class Application {
    -list~Product~ productBase
    -list~Stock~ inventory
    -list~Order~ orders
    +Product createProduct(string name)
    +Product findProduct(string name)
    +Application setStock(Product product, int qtty)
    +int getStock(Product product)
    +Application setProductPrice(Product product, double price)
    +Order createOrder(Product product)
    +list~Order~ findOrders()
    +list~Order~ findOrdersByDeliveryDate(Date deliveryDate)
    +list~Order~ findOrdersByOrderDate(Date orderDate)
    +list~Order~ findOrdersByProduct(Product product)
  }

  Product "0,n" --* Application : productBase
  Stock "0,n" --* Application : inventory
  Product "1,1" --o Stock : product
  Product "1,1" --o Order : product
  Date "1,1" --* Order : order
  Date "0,1" --* Order : delivery
  Order "0,n" --* Application : orders
```

</details>

> [!Note]
> Il existe plusieurs façons de modéliser et implémenter cette solution.
> Normalement, une classe d'association a une certaine représentation; ici, elle est représentée ainsi car Mermaid, le langage de représentation de diagrammes, ne gère pas l'affichage d'une classe d'association.

# 5. Produits périssables (héritage)

On souhaite distinguer les produits périssables des non-périssables.
Un produit périssable a une durée de conservation, ainsi qu'une température maximale de conservation.
Ce qui signifie qu'un stock doit pouvoir indiquer, dans le cas d'une donnée périssable, la DLC.

<details>
<summary>Proposition de solution</summary>

```mermaid
classDiagram
  class Product <<Abstract>> {
    -string name
    -double price
    +Product(string name)
    +Product setPrice(double price)
    +string getName()
    +double getPrice()
    +bool isPerishable()*
  }

  class UnperishableProduct { }

  class PerishableProduct {
    -int livingDuration
    -int maxTemp
    +PerishableProduct setLivingDuration(int ld)
    +PerishableProduct setMaxTemp(int md)
    +int getLivingDuration()
    +int getMaxTemp()
  }

  class Date {
    -int y
    -int m
    -int d
    +Date(int y, int m, int d)
    +int getY()
    +int getM()
    +int getD()
  }

  class Order {
    -Product product
    -int qtty
    -double unitPrice
    -Date orderDate
    -Date deliveryDate
    +Order(Product product, int qtty, double unitPrice, Date orderDate)
    +Order setDeliveryDate(Date deliveryDate)
    +Order setQtty(int qtty)
    +Order setUnitPrice(double unitPrice)
    +Product getProduct()
    +int getQtty()
    +double getUnitPrice()
    +Date getOrderDate()
    +Date getDeliveryDate()
  }

  class Stock {
    -Product product
    -int qtty
    #bool perishable
    +Stock(Product product, int qtty)
    +Stock setQtty(int qtty)
    +int getQtty()
  }

  class PerishableStock {
    -Date dlc
    +PerishableStock(PerishableProduct product, int qtty)
    +PerishableStock setDlc(Date dlc)
    +Date getDlc()
  }

  class Application {
    -list~Product~ productBase
    -list~Stock~ inventory
    -list~Order~ orders
    +Product createProduct(string name)
    +Product findProduct(string name)
    +Application setStock(Product product, int qtty)
    +Application setPerishableStock(PerishableProduct product, int qtty)
    +int getStock(Product product)
    +Application setProductPrice(Product product, double price)
    +Order createOrder(Product product)
    +list~Order~ findOrders()
    +list~Order~ findOrdersByDeliveryDate(Date deliveryDate)
    +list~Order~ findOrdersByOrderDate(Date orderDate)
    +list~Order~ findOrdersByProduct(Product product)
  }

  Product <|-- UnperishableProduct
  Product <|-- PerishableProduct
  Product "0,n" --* Application : productBase
  Stock "0,n" --* Application : inventory
  Stock <|-- PerishableStock
  PerishableStock *-- "1" PerishableProduct
  Product "1" --o Stock : product
  Product "1" --o Order : product
  Date "1" --* Order : order
  Date "0,1" --* Order : delivery
  Order "0,n" --* Application : orders
```

</details>

> [!Note]
> Il existe plusieurs manières d'implémenter cet héritage.
> Dans des langages comme Java ou PHP, par exemple, nous n'avons pas besoin du booléen `perishable` car le langage permet de `cast` automatiquement sans perte via réflexion.
> De même, SOLID nous conseillerait ici de diviser `Stock` en deux entités héritées, et de créer ensuite deux listes séparées dans `Application`.

# 6. Staticité

La classe `Application` contient un grand nombre de données réutilisables.
Cette classe ne devrait être initialisée qu'une seule fois, et ses méthodes devraient appartenir à la classe elle-même, nous aux objets.

Nous allons pour cela ajouter de la staticité, transformant tous les élements en statique.

<details>
<summary>Proposition de solution</summary>

```mermaid
classDiagram
  class Product <<Abstract>> {
    -string name
    -double price
    +Product(string name)
    +Product setPrice(double price)
    +string getName()
    +double getPrice()
    +bool isPerishable()*
  }

  class UnperishableProduct { }

  class PerishableProduct {
    -int livingDuration
    -int maxTemp
    +PerishableProduct setLivingDuration(int ld)
    +PerishableProduct setMaxTemp(int md)
    +int getLivingDuration()
    +int getMaxTemp()
  }

  class Date {
    -int y
    -int m
    -int d
    +Date(int y, int m, int d)
    +int getY()
    +int getM()
    +int getD()
  }

  class Order {
    -Product product
    -int qtty
    -double unitPrice
    -Date orderDate
    -Date deliveryDate
    +Order(Product product, int qtty, double unitPrice, Date orderDate)
    +Order setDeliveryDate(Date deliveryDate)
    +Order setQtty(int qtty)
    +Order setUnitPrice(double unitPrice)
    +Product getProduct()
    +int getQtty()
    +double getUnitPrice()
    +Date getOrderDate()
    +Date getDeliveryDate()
  }

  class Stock {
    -Product product
    -int qtty
    #bool perishable
    +Stock(Product product, int qtty)
    +Stock setQtty(int qtty)
    +int getQtty()
  }

  class PerishableStock {
    -Date dlc
    +PerishableStock(PerishableProduct product, int qtty)
    +PerishableStock setDlc(Date dlc)
    +Date getDlc()
  }

  class Application {
    -list~Product~ productBase$
    -list~Stock~ inventory$
    -list~Order~ orders$
    +Product createProduct(string name)$
    +Product findProduct(string name)$
    +Application setStock(Product product, int qtty)$
    +Application setPerishableStock(PerishableProduct product, int qtty)$
    +int getStock(Product product)$
    +Application setProductPrice(Product product, double price)$
    +Order createOrder(Product product)$
    +list~Order~ findOrders()$
    +list~Order~ findOrdersByDeliveryDate(Date deliveryDate)$
    +list~Order~ findOrdersByOrderDate(Date orderDate)$
    +list~Order~ findOrdersByProduct(Product product)$
  }

  Product <|-- UnperishableProduct
  Product <|-- PerishableProduct
  Product "0,n" --* Application : productBase
  Stock "0,n" --* Application : inventory
  Stock <|-- PerishableStock
  PerishableStock *-- "1" PerishableProduct
  Product "1" --o Stock : product
  Product "1" --o Order : product
  Date "1" --* Order : order
  Date "0,1" --* Order : delivery
  Order "0,n" --* Application : orders
```

</details>

> [!Warning]
> Un bug de rendu Mermaid empêche la visualisation des éléments déclarés statiques comme étant... statiques (soulignés).
> Vérifiez la source au besoin.

Il semnble néanmoins ridicule de TOUT transformer en statique ici.
On souhaite n'avoir qu'une seule instance de la classe Application existant au même moment, cependant.
Créons donc un singleton :

<details>
<summary>Proposition de solution</summary>

```mermaid
classDiagram
  class Product <<Abstract>> {
    -string name
    -double price
    +Product(string name)
    +Product setPrice(double price)
    +string getName()
    +double getPrice()
    +bool isPerishable()*
  }

  class UnperishableProduct { }

  class PerishableProduct {
    -int livingDuration
    -int maxTemp
    +PerishableProduct setLivingDuration(int ld)
    +PerishableProduct setMaxTemp(int md)
    +int getLivingDuration()
    +int getMaxTemp()
  }

  class Date {
    -int y
    -int m
    -int d
    +Date(int y, int m, int d)
    +int getY()
    +int getM()
    +int getD()
  }

  class Order {
    -Product product
    -int qtty
    -double unitPrice
    -Date orderDate
    -Date deliveryDate
    +Order(Product product, int qtty, double unitPrice, Date orderDate)
    +Order setDeliveryDate(Date deliveryDate)
    +Order setQtty(int qtty)
    +Order setUnitPrice(double unitPrice)
    +Product getProduct()
    +int getQtty()
    +double getUnitPrice()
    +Date getOrderDate()
    +Date getDeliveryDate()
  }

  class Stock {
    -Product product
    -int qtty
    #bool perishable
    +Stock(Product product, int qtty)
    +Stock setQtty(int qtty)
    +int getQtty()
  }

  class PerishableStock {
    -Date dlc
    +PerishableStock(PerishableProduct product, int qtty)
    +PerishableStock setDlc(Date dlc)
    +Date getDlc()
  }

  class Application {
    -Application instance$
    -list~Product~ productBase
    -list~Stock~ inventory
    -list~Order~ orders
    -Application()
    +Application getInstance()$
    +Product createProduct(string name)
    +Product findProduct(string name)
    +Application setStock(Product product, int qtty)
    +Application setPerishableStock(PerishableProduct product, int qtty)
    +int getStock(Product product)
    +Application setProductPrice(Product product, double price)
    +Order createOrder(Product product)
    +list~Order~ findOrders()
    +list~Order~ findOrdersByDeliveryDate(Date deliveryDate)
    +list~Order~ findOrdersByOrderDate(Date orderDate)
    +list~Order~ findOrdersByProduct(Product product)
  }

  Product <|-- UnperishableProduct
  Product <|-- PerishableProduct
  Product "0,n" --* Application : productBase
  Stock "0,n" --* Application : inventory
  Stock <|-- PerishableStock
  PerishableStock *-- "1" PerishableProduct
  Product "1" --o Stock : product
  Product "1" --o Order : product
  Date "1" --* Order : order
  Date "0,1" --* Order : delivery
  Order "0,n" --* Application : orders
```

</details>

Ainsi, seuls `instance` et `getInstance` sont statiques.
Si pour une raison quelconque on souhaite "nettoyer" la mémoire en détruisant Application, alors on peut désormais le faire.

# 7. isPerishable (interface)

Actuellement, les Produits sont les mêmes sans distinction entre Perishable et non, dans les Orders.
Implémentez un distingo pour les produits périssables ou non pour les commandes également.

<details>
<summary>Proposition de solution</summary>

```mermaid
classDiagram
  class Product <<Abstract>> {
    -string name
    -double price
    +Product(string name)
    +Product setPrice(double price)
    +string getName()
    +double getPrice()
    +bool isPerishable()*
  }

  class UnperishableProduct { }

  class PerishableProduct {
    -int livingDuration
    -int maxTemp
    +PerishableProduct setLivingDuration(int ld)
    +PerishableProduct setMaxTemp(int md)
    +int getLivingDuration()
    +int getMaxTemp()
  }

  class Date {
    -int y
    -int m
    -int d
    +Date(int y, int m, int d)
    +int getY()
    +int getM()
    +int getD()
  }

  class Order {
    -Product product
    -int qtty
    -double unitPrice
    -Date orderDate
    -Date deliveryDate
    +Order(Product product, int qtty, double unitPrice, Date orderDate)
    +Order setDeliveryDate(Date deliveryDate)
    +Order setQtty(int qtty)
    +Order setUnitPrice(double unitPrice)
    +Product getProduct()
    +int getQtty()
    +double getUnitPrice()
    +Date getOrderDate()
    +Date getDeliveryDate()
  }

  class PerishableOrder {
    -Date dlc
    +PerishableOrder(PerishableProduct product, int qtty, double unitPrice, Date orderDate)
    +PerishableOrder setDlc(Date dlc)
    +Date getDlc()
  }

  class Stock {
    -Product product
    -int qtty
    #bool perishable
    +Stock(Product product, int qtty)
    +Stock setQtty(int qtty)
    +int getQtty()
  }

  class PerishableStock {
    -Date dlc
    +PerishableStock(PerishableProduct product, int qtty)
    +PerishableStock setDlc(Date dlc)
    +Date getDlc()
  }

  class Application {
    -Application instance$
    -list~Product~ productBase
    -list~Stock~ inventory
    -list~Order~ orders
    -Application()
    +Application getInstance()$
    +Product createProduct(string name)
    +Product findProduct(string name)
    +Application setStock(Product product, int qtty)
    +Application setPerishableStock(PerishableProduct product, int qtty)
    +int getStock(Product product)
    +Application setProductPrice(Product product, double price)
    +Order createOrder(Product product)
    +list~Order~ findOrders()
    +list~Order~ findOrdersByDeliveryDate(Date deliveryDate)
    +list~Order~ findOrdersByOrderDate(Date orderDate)
    +list~Order~ findOrdersByProduct(Product product)
  }

  Product <|-- UnperishableProduct
  Product <|-- PerishableProduct
  Product "0,n" --* Application : productBase
  Stock "0,n" --* Application : inventory
  Stock <|-- PerishableStock
  PerishableStock *-- "1" PerishableProduct
  Product "1" --o Stock : product
  Product "1" --o Order : product
  Date "1" --* Order : order
  Date "0,1" --* Order : delivery
  Date "0,1" --* PerishableStock : dlc
  Date "0,1" --* PerishableOrder : dlc
  Order <|-- PerishableOrder
  Order "0,n" --* Application : orders
```

</details>

On souhaite désormais ajouter une méthode unique à la fois pour les Produits et les Orders pour connaître la DLC d'un produit en stock ou commandé.
Une méthode comme `getProductDlc(???)` qui fonctionnerait sur un produit en stock ou en commende.
Pour cela, nous allons explicitement ajouter le fait que ces classes implémentent **forcément** un moyen de connaître la DLC, une méthode `getDlc`.
Cela passe par une interface, qu'on crée en indiquant nos méthodes à implémenter.
Puis, chaque classe implémentant cette interface **devra** implémenter ces méthodes.
Ainsi, on peut utiliser le nom de l'interface comme un nom de classe dans nos différentes méthodes.

<details>
<summary>Proposition de solution</summary>

```mermaid
classDiagram
  class Product <<Abstract>> {
    -string name
    -double price
    +Product(string name)
    +Product setPrice(double price)
    +string getName()
    +double getPrice()
    +bool isPerishable()*
  }

  class UnperishableProduct { }

  class PerishableProduct {
    -int livingDuration
    -int maxTemp
    +PerishableProduct setLivingDuration(int ld)
    +PerishableProduct setMaxTemp(int md)
    +int getLivingDuration()
    +int getMaxTemp()
  }

  class Date {
    -int y
    -int m
    -int d
    +Date(int y, int m, int d)
    +int getY()
    +int getM()
    +int getD()
  }

  class Order {
    -Product product
    -int qtty
    -double unitPrice
    -Date orderDate
    -Date deliveryDate
    +Order(Product product, int qtty, double unitPrice, Date orderDate)
    +Order setDeliveryDate(Date deliveryDate)
    +Order setQtty(int qtty)
    +Order setUnitPrice(double unitPrice)
    +Product getProduct()
    +int getQtty()
    +double getUnitPrice()
    +Date getOrderDate()
    +Date getDeliveryDate()
  }

  class PerishableOrder {
    -Date dlc
    +PerishableOrder(PerishableProduct product, int qtty, double unitPrice, Date orderDate)
    +PerishableOrder setDlc(Date dlc)
    +Date getDlc()
  }

  class Stock {
    -Product product
    -int qtty
    #bool perishable
    +Stock(Product product, int qtty)
    +Stock setQtty(int qtty)
    +int getQtty()
  }

  class PerishableStock {
    -Date dlc
    +PerishableStock(PerishableProduct product, int qtty)
    +PerishableStock setDlc(Date dlc)
    +Date getDlc()
  }

  class Application {
    -Application instance$
    -list~Product~ productBase
    -list~Stock~ inventory
    -list~Order~ orders
    -Application()
    +Application getInstance()$
    +Product createProduct(string name)
    +Product findProduct(string name)
    +Application setStock(Product product, int qtty)
    +Application setPerishableStock(PerishableProduct product, int qtty)
    +int getStock(Product product)
    +Application setProductPrice(Product product, double price)
    +Order createOrder(Product product)
    +list~Order~ findOrders()
    +list~Order~ findOrdersByDeliveryDate(Date deliveryDate)
    +list~Order~ findOrdersByOrderDate(Date orderDate)
    +list~Order~ findOrdersByProduct(Product product)
    +getProductDlc(HasDlc stockOrOrder)
  }

  class HasDlc <<Interface>> {
    +Date getDlc()*
  }

  Product <|-- UnperishableProduct
  Product <|-- PerishableProduct
  Product "0,n" --* Application : productBase
  Stock "0,n" --* Application : inventory
  Stock <|-- PerishableStock
  PerishableStock *-- "1" PerishableProduct
  Product "1" --o Stock : product
  Product "1" --o Order : product
  Date "1" --* Order : order
  Date "0,1" --* Order : delivery
  Date "0,1" --* PerishableStock : dlc
  Date "0,1" --* PerishableOrder : dlc
  Order <|-- PerishableOrder
  Order "0,n" --* Application : orders
  HasDlc <|.. PerishableStock : implements
  HasDlc <|.. PerishableOrder : implements
  HasDlc <.. Application
```

</details>

> [!Tip]
> Ici, l'exemple n'est pas forcément le meilleur, mais il permet de voir un cas d'usage.
> Une interface est un contrat passé, permettant d'affirmer que telle classe **DOIT** fournir une ou plusieurs méthodes, donc *est capable de* faire ce que l'interface déclare.
