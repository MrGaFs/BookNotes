# Creational Patterns

---

Now we will go deep in some creational patterns and there UML to them

## Table of Content

- [Creational Patterns](#creational-patterns)
	- [Table of Content](#table-of-content)
	- [Factory Design Pattern](#factory-design-pattern)
		- [What is Factory Design Pattern?](#what-is-factory-design-pattern)
		- [Factory Design Pattern UML](#factory-design-pattern-uml)
		- [Notes on Factory DP](#notes-on-factory-dp)
			- [When to use factory design Pattern](#when-to-use-factory-design-pattern)
			- [How To implement](#how-to-implement)
			- [Code samplte in __TypeScript__](#code-samplte-in-typescript)
	- [Abstract Factory Design Pattern](#abstract-factory-design-pattern)
		- [What is Abstract Factory Design Pattern?](#what-is-abstract-factory-design-pattern)
		- [abstract factory UML](#abstract-factory-uml)
		- [Notes on Abstract Factory Design Pattern](#notes-on-abstract-factory-design-pattern)
			- [When to use av](#when-to-use-av)

## Factory Design Pattern

### What is Factory Design Pattern?

This used if You don't Know excatly how you gonna work with and make your code more expandable for next updates

### Factory Design Pattern UML

![Factory png](Factory.png)

### Notes on Factory DP

#### When to use factory design Pattern

1. Use the Factory Method when you don’t know beforehand the exact types and dependencies of the objects your code should work with.

1. Use the Factory Method when you want to provide users of your library or framework with a way to extend its internal components.

1. Use the Factory Method when you want to save system resources by reusing existing objects instead of rebuilding them each time.

#### How To implement

1. Make all products follow the same interface. This interface should declare methods that make sense in every product.

1. Add an empty factory method inside the creator class. The return type of the method should match the common product interface.

1. In the creator’s code find all references to product constructors. One by one, replace them with calls to the factory method, while extracting the product creation code into the factory method.

1. You might need to add a temporary parameter to the factory method to control the type of returned product.

1. At this point, the code of the factory method may look pretty ugly. It may have a large switch operator that picks which product class to instantiate. But don’t worry, we’ll fix it soon enough.

1. Now, create a set of creator subclasses for each type of product listed in the factory method. Override the factory method in the subclasses and extract the appropriate bits of construction code from the base method.

1. If there are too many product types and it doesn’t make sense to create subclasses for all of them, you can reuse the control parameter from the base class in subclasses.

1. For instance, imagine that you have the following hierarchy of classes: the base Mail class with a couple of subclasses: AirMail and GroundMail; the Transport classes are Plane, Truck and Train. While the AirMail class only uses Plane objects, GroundMail may work with both Truck and Train objects. You can create a new subclass (say TrainMail) to handle both cases, but there’s another option. The client code can pass an argument to the factory method of the GroundMail class to control which product it wants to receive.

1. If, after all of the extractions, the base factory method has become empty, you can make it abstract. If there’s something left, you can make it a default behavior of the method.
   
#### Code samplte in __TypeScript__

```ts
/**
 * The Creator class declares the factory method that is supposed to return an
 * object of a Product class. The Creator's subclasses usually provide the
 * implementation of this method.
 */
abstract class Creator {
 /**
  * Note that the Creator may also provide some default implementation of the
  * factory method.
  */
 public abstract factoryMethod(): Product;

 /**
  * Also note that, despite its name, the Creator's primary responsibility is
  * not creating products. Usually, it contains some core business logic that
  * relies on Product objects, returned by the factory method. Subclasses can
  * indirectly change that business logic by overriding the factory method
  * and returning a different type of product from it.
  */
 public someOperation(): string {
  // Call the factory method to create a Product object.
  const product = this.factoryMethod();
  // Now, use the product.
  return `Creator: The same creator's code has just worked with ${product.operation()}`;
 }
}

/**
 * Concrete Creators override the factory method in order to change the
 * resulting product's type.
 */
class ConcreteCreator1 extends Creator {
 /**
  * Note that the signature of the method still uses the abstract product
  * type, even though the concrete product is actually returned from the
  * method. This way the Creator can stay independent of concrete product
  * classes.
  */
 public factoryMethod(): Product {
  return new ConcreteProduct1();
 }
}

class ConcreteCreator2 extends Creator {
 public factoryMethod(): Product {
  return new ConcreteProduct2();
 }
}

/**
 * The Product interface declares the operations that all concrete products must
 * implement.
 */
interface Product {
 operation(): string;
}

/**
 * Concrete Products provide various implementations of the Product interface.
 */
class ConcreteProduct1 implements Product {
 public operation(): string {
  return '{Result of the ConcreteProduct1}';
 }
}

class ConcreteProduct2 implements Product {
 public operation(): string {
  return '{Result of the ConcreteProduct2}';
 }
}

/**
 * The client code works with an instance of a concrete creator, albeit through
 * its base interface. As long as the client keeps working with the creator via
 * the base interface, you can pass it any creator's subclass.
 */
function clientCode(creator: Creator) {
 // ...
 console.log('Client: I\'m not aware of the creator\'s class, but it still works.');
 console.log(creator.someOperation());
 // ...
}

/**
 * The Application picks a creator's type depending on the configuration or
 * environment.
 */
console.log('App: Launched with the ConcreteCreator1.');
clientCode(new ConcreteCreator1());
console.log('');

console.log('App: Launched with the ConcreteCreator2.');
clientCode(new ConcreteCreator2());
```

## Abstract Factory Design Pattern

### What is Abstract Factory Design Pattern?

If I had some stuff that is siamilar in some stuff like if we had a chair and sofa and other stuff but they all share the same style and colors we can use this design pattern to make it easier to change the style.

### abstract factory UML

![abstractFactory.png](abstractFactory.png)

### Notes on Abstract Factory Design Pattern

#### When to use abstract factory design Pattern

Use the Abstract Factory when your code needs to work with various families of related products, but you don’t want it to depend on the concrete classes of those products—they might be unknown beforehand or you simply want to allow for future extensibility.

#### How to implement

1. Map out a matrix of distinct product types versus variants of these products.

1. Declare abstract product interfaces for all product types. Then make all concrete product classes implement these interfaces.

1. Declare the abstract factory interface with a set of creation methods for all abstract products.

1. Implement a set of concrete factory classes, one for each product variant.

1. Create factory initialization code somewhere in the app. It should instantiate one of the concrete factory classes, depending on the application configuration or the current environment. Pass this factory object to all classes that construct products.

1. Scan through the code and find all direct calls to product constructors. Replace them with calls to the appropriate creation method on the factory object.

#### Code sample

```ts
/**
 * The Abstract Factory interface declares a set of methods that return
 * different abstract products. These products are called a family and are
 * related by a high-level theme or concept. Products of one family are usually
 * able to collaborate among themselves. A family of products may have several
 * variants, but the products of one variant are incompatible with products of
 * another.
 */
interface AbstractFactory {
    createProductA(): AbstractProductA;

    createProductB(): AbstractProductB;
}

/**
 * Concrete Factories produce a family of products that belong to a single
 * variant. The factory guarantees that resulting products are compatible. Note
 * that signatures of the Concrete Factory's methods return an abstract product,
 * while inside the method a concrete product is instantiated.
 */
class ConcreteFactory1 implements AbstractFactory {
    public createProductA(): AbstractProductA {
        return new ConcreteProductA1();
    }

    public createProductB(): AbstractProductB {
        return new ConcreteProductB1();
    }
}

/**
 * Each Concrete Factory has a corresponding product variant.
 */
class ConcreteFactory2 implements AbstractFactory {
    public createProductA(): AbstractProductA {
        return new ConcreteProductA2();
    }

    public createProductB(): AbstractProductB {
        return new ConcreteProductB2();
    }
}

/**
 * Each distinct product of a product family should have a base interface. All
 * variants of the product must implement this interface.
 */
interface AbstractProductA {
    usefulFunctionA(): string;
}

/**
 * These Concrete Products are created by corresponding Concrete Factories.
 */
class ConcreteProductA1 implements AbstractProductA {
    public usefulFunctionA(): string {
        return 'The result of the product A1.';
    }
}

class ConcreteProductA2 implements AbstractProductA {
    public usefulFunctionA(): string {
        return 'The result of the product A2.';
    }
}

/**
 * Here's the the base interface of another product. All products can interact
 * with each other, but proper interaction is possible only between products of
 * the same concrete variant.
 */
interface AbstractProductB {
    /**
     * Product B is able to do its own thing...
     */
    usefulFunctionB(): string;

    /**
     * ...but it also can collaborate with the ProductA.
     *
     * The Abstract Factory makes sure that all products it creates are of the
     * same variant and thus, compatible.
     */
    anotherUsefulFunctionB(collaborator: AbstractProductA): string;
}

/**
 * These Concrete Products are created by corresponding Concrete Factories.
 */
class ConcreteProductB1 implements AbstractProductB {

    public usefulFunctionB(): string {
        return 'The result of the product B1.';
    }

    /**
     * The variant, Product B1, is only able to work correctly with the variant,
     * Product A1. Nevertheless, it accepts any instance of AbstractProductA as
     * an argument.
     */
    public anotherUsefulFunctionB(collaborator: AbstractProductA): string {
        const result = collaborator.usefulFunctionA();
        return `The result of the B1 collaborating with the (${result})`;
    }
}

class ConcreteProductB2 implements AbstractProductB {

    public usefulFunctionB(): string {
        return 'The result of the product B2.';
    }

    /**
     * The variant, Product B2, is only able to work correctly with the variant,
     * Product A2. Nevertheless, it accepts any instance of AbstractProductA as
     * an argument.
     */
    public anotherUsefulFunctionB(collaborator: AbstractProductA): string {
        const result = collaborator.usefulFunctionA();
        return `The result of the B2 collaborating with the (${result})`;
    }
}

/**
 * The client code works with factories and products only through abstract
 * types: AbstractFactory and AbstractProduct. This lets you pass any factory or
 * product subclass to the client code without breaking it.
 */
function clientCode(factory: AbstractFactory) {
    const productA = factory.createProductA();
    const productB = factory.createProductB();

    console.log(productB.usefulFunctionB());
    console.log(productB.anotherUsefulFunctionB(productA));
}

/**
 * The client code can work with any concrete factory class.
 */
console.log('Client: Testing client code with the first factory type...');
clientCode(new ConcreteFactory1());

console.log('');

console.log('Client: Testing the same client code with the second factory type...');
clientCode(new ConcreteFactory2());
```