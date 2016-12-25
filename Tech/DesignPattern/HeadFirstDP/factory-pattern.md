# Factory Pattern
When you see “new”, think “concrete”.

First principle deals with change and guides us to identify the aspects that vary and separate them from what stays the same.

Factories handle the details of object creation.

Factory Method Pattern defined
The Factory Method Pattern defines an interface for creating an object, but lets subclasses decide which class to instantiation to subclasses.

The Dependency Inversion Principle
Depend upon abstractions, Do not depend upon concrete classes.
It suggests that our high-level components should not depend on our low-level components; rather, they should both depend on abstractions.

An Abstract Factory gives us an interface for creating a family of products. By writing code that uses this interface, we decouple our code from the actual factory that creates the products. That allows us to implement a variety of factories that produce products meant for different contexts – such as different regions, different operating system, or different look and feels.

Abstract Factory Pattern defined
The Abstract Factory Pattern provides an interface for creating families of related or dependent objects without specifying their concrete classes.
