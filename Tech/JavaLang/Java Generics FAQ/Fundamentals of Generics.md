# Table of Contents

- [What are java generics?][]
- [What is primary purpose of Java generics?][primary-purpose]
- [What is benefit of using Java generics?][java-generics-benefit]

## What are java generics? ##

> Java Generics, sometimes used as a plural noun(generics) and sometimes as a singular noun(Generics), is a language feature of Java that allows for the definition and use of **generic types and methods.**
 
Generic types or methods differ from regular types and methods in that they have **type parameters**.

Generic types are instantiated to form **parameterized types** by providing actual type arguments that replace the formal type parameters. For example: `LinkedList<E>` is a generic type, `E` is a type parameter. Instantiations, such as `LinkedList<String>` or `LinkedList<Integer>` are called **parameterized types**, and `String` and `Integer` are the respective actual type arguments.

## What is primary purpose of Java generics? [primary-purpose] ##

> Java generics were invented primarily for implementation of generic collections.

A more reasonable goal is to have a single implementation of the collection class and use it to hold elements of different types. In other words, rather than implementing a class `IntegerList` and `StringList`, we want to have one generic implementation `List` that can be easily used in either case, as well as in other unforeseen cases.

This is what generics for: **the implementation of one generic class can be instantiated for a variety of types**.

The use of Java generics language features were initially motivated by the need to have a mechanism for efficient implementation of homogenous collections, but the language feature is not restricted to collections. For examples:

- the weak and soft references in package `java.lang.ref`, which are special purpose references to objects of a particular type represented by a type parameter.
- the interface `Callable` in package `java.util.concurrent`, which represents a task and has a `call` method that returns a result of a particular type represented by a type parameter.
- the class `Class` in package `java.lang` is a generic class, whose type parameter denotes the type that `Class` object represents.
	
## What is the benefit of using Java generics? [java-generics-benefit] ##

> Early error detection at compile time.

Using parameterized type such as `LinkedList<String>`, instead of `LinkedList`, enables the compiler to perform more type checks and requires fewer dynamic casts. This way errors are detected earlier, in the sense that they are reported at compile-time by means of a compiler error message rather than at runtime by means of an exception.

## What does type-safety mean? [what-does-type-safety-mean] ##

> In Java, a program is considered type-safe if it compiles without errors and warnings and does not raise any unexpected `ClassCastException`s at runtime.

--- Angelika Langer. *Java Generics Frequently Asked Questions*, 2015
