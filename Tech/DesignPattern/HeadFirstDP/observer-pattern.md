# The dark side of java.util.Observable:
Observable is a class, not an interface, and worse, it doesn't even implement an interface. Unfortunately, the java.util.Observalbe implementation has a number of problems that limit its usefulness and reuse.
1. Observable is a class
Because it is a class, you have to subclass it. That means you can't add on the observable behavior to an existing class that already extends another superclass. This limits its reuse.
Because there isn't an Observable interface, you can't even create your own implementation that plays well with Java's built-in Observer API.
2. Observable protects crucial methods
the setChanged() method is protected, so this means you can't call setChanged() unless you've subclassed Observable. This means you can't even create an instance of the Observable class and compose it with your own objects, you have to subclass. The design violates “favor composition over inheritance” design priciple.
