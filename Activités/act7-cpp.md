# Activité 5 : CPP Shop

## Objectifs

Cette activité a pour objectif de mettre en pratique les notions suivantes en C++ :

* séparation `.hpp` / `.cpp`
* compilation avec `Makefile`
* programmation orientée objet
* encapsulation
* héritage
* polymorphisme
* surcharge d’opérateurs
* gestion mémoire manuelle avec `new` / `delete`
* transition vers les smart pointers (`std::unique_ptr` / `std::shared_ptr`)
* manipulation de collections STL (`std::vector`)
* architecture logicielle propre

L'activité se base sur l'activité 5, en étant plus réduite sur la partie guidée.

---

# Structure du projet

Le projet devra respecter l’organisation suivante

```text
cpp-shop/
├── include/
│   ├── Product.hpp
│   ├── Application.hpp
│   └── ...
├── src/
│   ├── Product.cpp
│   ├── Application.cpp
│   ├── ...
│   └── main.cpp
├── Makefile
└── README.md
```

---

# Partie 0 : Makefile

Avant de commencer le développement, mettre en place un `Makefile` permettant de compiler le projet.

## Fonctionnalités attendues

Le `Makefile` devra permettre :

### Compiler

```bash
# compiler
make
# exécuter
make run
# nettoyer
make clean
```

---

## Exemple de Makefile

<details>
<summary>Proposition de solution</summary>

```Makefile
CXX = g++
CXXFLAGS = -Wall -Wextra -std=c++17 -Iinclude

SRC = $(wildcard src/*.cpp)
OBJ = $(SRC:.cpp=.o)
TARGET = shop

all: $(TARGET)

$(TARGET): $(OBJ)
	$(CXX) $(CXXFLAGS) -o $(TARGET) $(OBJ)

clean:
	rm -f $(OBJ) $(TARGET)

run: all
	./$(TARGET)
```

</details>

---

# Partie 1 : Classe `Product`

Créer une classe `Product` représentant un produit générique.

## Attributs

* `id` : décidé à l'initialisation, ne change jamais
* `name` : décidé à l'initialisation, ne change jamais
* `price` : peut être fourni à l'initialisé, peut être modifié, toujours supérieur à zéro après l'initialisation

Les attributs devront être privés.

---

## Méthodes attendues

Créez le constructeur, ainsi que les accesseurs nécessaires.
Ajoutez une méthode `void display();` qui effectuera un affichage de votre `Product`, sous une forme similaire à `[1] Clavier (89.99e)`.

<details>
<summary>Proposition de solution</summary>

```cpp
// Product.hpp
#pragma once
#include <string>
#include <iostream>
class Product
{
private:
	int id;
	std::string name;
	double price;
public:
	Product(int id, const std::string& name, double price);
	int getId();
	std::string getName();
	double getPrice();
	void setPrice(double price);
	void display();
};

// Product.cpp
#include "Product.hpp"

Product::Product(int id, const std::string& name, double price)
{
	this->id = id;
	this->name = name;
	if (price >= 0) {
		this->price = price;
	}
}

int Product::getId()
{
	return this->id;
}

std::string Product::getName()
{
	return this->name;
}

double Product::getPrice()
{
	return this->price;
}

void Product::setPrice(double price) {
	if (price > 0) {
		this->price = price;
	}
}

void Product::display()
{
	std::cout << "[" << this->id << "] " << this->name << " (" << this->price << "e)" << std::endl;
}

// main.cpp
#include "Product.hpp"

int main()
{
    Product* p = new Product(1, "ABC", 10.5);
    p->setPrice(22.5);
    p->display();
    return 0;
}

```

</details>

> [!Note]
> Ce que vous voyez avec l'argument `name` est ce qu'on appelle un passage par référence constante.
> C'est une "astuce" en C++ qui permet de passer non pas l'objet complet par copie, mais une référence (un mécanisme similaire aux pointeurs, mais différent).
> Effectuer ce passage en `const` permet d'éviter de répercuter toute modification sur l'objet initial.
> Ce mode de passage, possible évidemment uniquement pour les objets, a un léger effet bénéfique sur les performances.


> [!Caution]
> Attention à ne pas confondre l'opérateur `&` de passage par référence (dans les définitions/déclarations) avec l'opérateur `&` d'obtention d'une adresse mémoire.
> Attention également à n'utiliser ce mécanisme que lorsqu'il est "nécessaire" et non partout. Parfois, on a besoin d'une référence modifiable, d'un pointeur ou d'une copie d'objet.

Que constatez-vous ? Modifiez vos méthodes pour qu'elles soient, si pertinent, constantes.

---

# Partie 2 : Application

Créez la classe `Application` qui va contenir une liste de `Product`.
Cette liste sera un `std::vector` de pointeurs de `Product`, ce qui permettra, à l'instar de l'application en Java, d'éviter de dupliquer des instances de produits.
Cette classe permettra également de créer des produits, et possédera aussi une méthode d'affichage.

<details>
  <summary>Proposition de solution</summary>

```cpp
// Application.hpp
#pragma once
#include <vector>
#include <string>
#include "Product.hpp"
class Application
{
private:
	std::vector<Product*> products;
public:
	Application();
	void createProduct(int id, const std::string& name, double price);
	std::vector<Product*> getProducts() const;
	void display() const;
};

// Application.cpp
#include "Application.hpp"

Application::Application()
{
	this->products = std::vector<Product*>();
}

void Application::createProduct(int id, const std::string& name, double price)
{
	this->products.push_back(new Product(id, name, price));
}

std::vector<Product*> Application::getProducts() const
{
	return this->products;
}

void Application::display() const {
	for (Product* p : this->products) {
		p->display();
	}
}

// main.cpp
#include "Application.hpp"

int main()
{
    Application a = Application();
    a.createProduct(1, "ABC", 10.5);
    a.display();
    return 0;
}


```

</details>




---

# Partie 3 : Héritage

Créer deux classes spécialisées `UnperishableProduct` et `PerishableProduct`.
Cette dernière aura un attribut supplémentaire : `expirationDays`, de type entier, initialisé par défaut à 0, mais pouvant être fourni.
Surchargez, dans `PerishableProdict`, la méthode `display`, pour appeler la méthode de la classe parente, précédée de l'affichage de "P".

Testez d'abord `PerishableProduct`.

<details>
  <summary>Proposition de solution</summary>

```cpp
// UnperishableProduct.hpp
#pragma once
#include "Product.hpp"
class UnperishableProduct :
    public Product
{
};

// PerishableProduct.hpp
#pragma once
#include "Product.hpp"
class PerishableProduct :
    public Product
{
private:
    int expirationDays;
public:
    PerishableProduct(int id, const std::string& name, double price, int expirationDays);
    void display() const;
};

// UnperishableProduct.cpp
#include "UnperishableProduct.hpp"

// PerishableProduct.cpp
#include "PerishableProduct.hpp"

PerishableProduct::PerishableProduct(int id, const std::string& name, double price, int expirationDays)
	: Product(id, name, price)
{
	this->expirationDays = expirationDays;
}

void PerishableProduct::display() const {
	std::cout << "P";
	Product::display();
}

```

</details>

> [!Warning]
> Vous devez appeler le constructeur parent explicitement.
> Sinon, le constructeur par défaut serait appelé, pour "copier" l'objet.
> Or, celui-ci n'existe pas !

> [!Tip]
> Que remarquez-vous sur la méthode `display` ?
> Corrigez cela en créant une méthode intermédiaire renvoyant une chaîne de caractères, utilisée par `display`.

Testez le code suivant :

```cpp
#include "Application.hpp"
#include "PerishableProduct.hpp"
#include "UnperishableProduct.hpp"

int main()
{
    UnperishableProduct* p1 = new UnperishableProduct(1, "ABC", 12.1);
    p1->display();
    PerishableProduct* p2 = new PerishableProduct(2, "DEF", 21.5, 3);
    p2->display();
    return 0;
}
```

Que se passe-t-il ? Résolvez le problème.

<details>
  <summary>Proposition de solution</summary>

```cpp
// UnperishableProduct.hpp
#pragma once
#include "Product.hpp"
class UnperishableProduct :
    public Product
{
public:
    UnperishableProduct(int id, const std::string& name, double price);
};

// UnperishableProduct.cpp
#include "UnperishableProduct.hpp"

UnperishableProduct::UnperishableProduct(int id, const std::string& name, double price): Product(id, name, price)
{
}

```

</details>

## Virtual, override, héritage

Modifiez `Application` pour ajouter une méthode `void createPerishableProduct(int id, const std::string& name, double price, int expirationDays);`.
Le pointeur vers `PerishableProduct` sera stocké dans le même attribut `products`.

Testez votre méthode dans votre `main` :

```cpp
#include "Application.hpp"

int main()
{
    Application a = Application();
    a.createProduct(1, "ABC", 12.1);
    a.createPerishableProduct(2, "DEF", 21.5, 3);
    a.display();
    return 0;
}
```

Que se passe-t-il ?

Modifiez votre classe `Product` pour indiquer la méthode `display` comme étant `virtual`.
Puis ajoutez un identifiant `override` sur la méthode `display` de `PerishableProduct`.
Testez à nouveau !

Transformez `Product` en classe abstraite.

> [!Caution]
> Ce constructeur est très pratique. Conservons-le.
> Plaçons le destructeur en pure virtual à la place; il faut par contre, en ce cas, le définir dans le `.cpp`, quitte à n'avoir aucun corps !

<details>
  <summary>Proposition de solution</summary>

```cpp
// Product.hpp
#pragma once
#include <string>
#include <iostream>
class Product
{
private:
	int id;
	std::string name;
	double price;
public:
	Product(int id, const std::string& name, double price);
	int getId() const;
	std::string getName() const;
	double getPrice() const;
	void setPrice(double price);
	virtual void display() const;
	virtual ~Product() = 0;
};

// Product.cpp
Product::~Product() {} // est omis le reste du fichier !

```

</details>

Testez votre classe `Application` :

```cpp
#include "Application.hpp"

int main()
{
    Application a = Application();
    a.createProduct(1, "ABC", 12.1);
    a.createPerishableProduct(2, "DEF", 21.5, 3);
    a.display();
    return 0;
}
```

---

# Partie 4 : Surcharge de l’opérateur de flux

Afin de simplifier l’affichage des objets, nous allons surcharger l’opérateur :

```cpp
<<
```

---

## Objectif

Pouvoir écrire :

```cpp
std::cout << product << std::endl;
```

au lieu de :

```cpp
product.display();
```

---

Il existe deux solutions majoritaires pour surcharger des opérateurs :
- créer une surcharge commune **hors de la classe** pour la signature exacte liant un stream et la classe qu'on cherche à utiliser
- créer une méthode `friend`, déclarée dans la classe, mais définie hors de celle-ci

> [!Note]
> Dans le second cas, le mot-clef `friend` permet l'accès **depuis l'extérieur de la classe**.
> C'est parfois un avantage, mais reste une violation partielle du principe d'encapsulation.
> "partielle", car on indique l'objet concerné comme étant une référence constante.

Surchargez les opérateurs de flux pour la classe `Application` et pour la classe `Product` (et ses enfants !).
Vous devrez utiliser deux moyens différents.
Vous pourriez avoir besoin d'ajouter une méthode intermédiaire renvoyant la chaîne à afficher, ou encore modifier des visibilités de `private` à `protected`.

<details>
  <summary>Proposition de solution</summary>

```cpp
// Application.hpp
#pragma once
#include <vector>
#include <string>
#include "Product.hpp"
#include "UnperishableProduct.hpp"
#include "PerishableProduct.hpp"
class Application
{
private:
	std::vector<Product*> products;
public:
	Application();
	void createProduct(int id, const std::string& name, double price);
	void createPerishableProduct(int id, const std::string& name, double price, int expirationDays);
	std::vector<Product*> getProducts() const;
	void display() const;
};

std::ostream& operator<<(std::ostream& os, const Application& a);

// Product.hpp
#pragma once
#include <string>
#include <iostream>
class Product
{
protected:
	int id;
	std::string name;
	double price;
public:
	Product(int id, const std::string& name, double price);
	int getId() const;
	std::string getName() const;
	double getPrice() const;
	void setPrice(double price);
	virtual void display() const;
	virtual ~Product() = 0;
	virtual std::string getDisplay() const;
	friend std::ostream& operator<<(std::ostream& os, const Product& p);
};

// PerishableProduct.hpp
#pragma once
#include "Product.hpp"
class PerishableProduct :
    public Product
{
private:
    int expirationDays;
public:
    PerishableProduct(int id, const std::string& name, double price, int expirationDays);
    void display() const override;
    std::string getDisplay() const override;
};

// Application.cpp
void Application::display() const {
	for (Product* p : this->products) {
		p->display();
	}
}

std::ostream& operator<<(std::ostream& os, const Application& a)
{
	for (Product* p : a.getProducts()) {
		os << *p << std::endl;
	}
	return os;
}

// Product.cpp
std::string Product::getDisplay() const {
	return "[" + std::to_string(this->id) + "] " + this->name + " (" + std::to_string(this->price) + "e)";
}

std::ostream& operator<<(std::ostream& os, const Product& p) {
	return os << p.getDisplay();
};

// PerishableProduct.cpp
std::string PerishableProduct::getDisplay() const {
	return "[P" + std::to_string(this->id) + "] " + this->name + " (" + std::to_string(this->price) + "e)";
}

```

</details>

---

# Partie 5 : Passage aux smart pointers

La gestion manuelle de la mémoire est source d’erreurs,
et nécessite d'entretenir les pointeurs pour libérer manuellement les références inutilisées,
faute de quoi des fuites pourraient apparaître.

Nous allons maintenant remplacer les pointeurs classiques par des smart pointers.

---

## `std::unique_ptr`

Un `unique_ptr` possède un objet de manière exclusive.

---

## Exemple

<details>
<summary>Utilisation de unique_ptr</summary>

```cpp
#include <memory>

std::unique_ptr<UnperishableProduct> product =
    std::make_unique<UnperishableProduct>(1, "Clavier", 89.99);
```

</details>

---

## `std::shared_ptr`

Un `shared_ptr` permet à plusieurs objets de partager la possession d’un même objet.

---

## Exemple

<details>
<summary>Utilisation de shared_ptr</summary>

```cpp
#include <memory>

std::shared_ptr<UnperishableProduct> p1 =
    std::make_shared<UnperishableProduct>(1, "Clavier", 89.99);

std::shared_ptr<UnperishableProduct> p2 = p1;
```

</details>

---

## Action !

Modifiez vos pointeurs manuels vers des smart pointers.

<details>
  <summary>Proposition de solution</summary>

```cpp
// Application.hpp
class Application
{
private:
	std::vector<std::shared_ptr<Product>> products;
public:
	Application();
	void createProduct(int id, const std::string& name, double price);
	void createPerishableProduct(int id, const std::string& name, double price, int expirationDays);
	std::vector<std::shared_ptr<Product>> getProducts() const;
	void display() const;
};

// Application.cpp
#include "Application.hpp"

Application::Application()
{
	this->products = std::vector<std::shared_ptr<Product>>();
}

void Application::createProduct(int id, const std::string& name, double price)
{
	this->products.push_back(std::make_shared<UnperishableProduct>(id, name, price));
}

void Application::createPerishableProduct(int id, const std::string& name, double price, int expirationDays)
{
	this->products.push_back(std::make_shared<PerishableProduct>(id, name, price, expirationDays));
}

std::vector<std::shared_ptr<Product>> Application::getProducts() const
{
	return this->products;
}

void Application::display() const {
	for (std::shared_ptr<Product> p : this->products) {
		p->display();
	}
}

std::ostream& operator<<(std::ostream& os, const Application& a)
{
	for (std::shared_ptr<Product> p : a.getProducts()) {
		os << *p << std::endl;
	}
	return os;
}

```

</details>

