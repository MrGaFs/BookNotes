# Creational Patterns
---

Now we will go deep in some creational patterns and there UML to them 

## Table of Content

1. [Factory Design Pattern](# "Factory Design Pattern")

## Factory Design Pattern

This used if You don't Know excatly how you gonna work with and make your code more expandable for next updates 

![Factory png](Factory.png)

### Notes on Factory DP

#### When to use factory design Pattern

1.  Use the Factory Method when you don’t know beforehand the exact types and dependencies of the objects your code should work with.

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