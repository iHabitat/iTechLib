# Single Generics

Generics implement the concept of **parameterized types**, which allow multiple types.

## A tuple library 

One of the things you often want to do is return multiple objects from a method call. The `return` statement only allows you to specify a single object, so the answer is to create an object that holds the multiple objects that you want to return.

This concept is called **a tuple**, and it is simply a group of objects wrapped together into a single object. The recipient of the object is allowed to read the elements but not put new ones in.(This concept is also called a **Data Transfer Object** or **Messenger**.)

Tuples can typically be any length, but each object in the tuple can be of a different type.

## Generic Methods

You can also parameterize methods within a class. The class itself may or may not be generic-this is independent of whether you have a generic method. 

A generic method  allows the method to vary independently of the class. As the guideline, you should use generic methods "whenever you can."  In addition, if a method is `static`, it has no access to the generic type parameters of the class, so if it needs to use genericity it must be a generic method.

> Note that with a generic class, you must specify the type parameters when you instantiate the class. But with a generic method, you don't usually have to specify the parameter types, because the compiler can figure that out for you. This is called **type argument inference**.

## The mystery of erasure

For example, although you can say `ArrayList.class`, you can not say `ArrayList<Integer>.class`. Actually, `ArrayList<String>` and `ArrayList<Integer>` are the same type `ArrayList`.

> There's no information about **generic parameter types** available inside generic code.

Java generics are implemented using **erasure**. This means that any specific type information is erased when you use a generic. Inside the generic, the only thing that you know is that you're using an object. So `List<String>` and `List<Integer>` are, in fact, the same type at runtime.

### The C++ approach

The bound `<T extends HasF>` says that `T` must be of type `HasF` or something derived from `HasF`. 

We say that generic type parameter erasures to **its first bound**(it's possible to have multiple bounds). The compiler actually replaces the type parameter with its erasure, so in the above case, `T` erasures to `HasF`, which is the same as replacing `T` with `HasF` in the class body.

> Generics are only useful when you want to use type parameters that are more "generic" than a specific type(and all its subtypes)-that is, when you want code to work across multiple classes. As a result, the type parameters and their application in useful generic code will usually be more complex than simple class replacement. However, you can't just say that anything of the form `<T extends HasF>` is therefor flawed. For example, if a class has a method that returns `T`, then generics are helpful, because they will then return the extract type:

```java
class ReturnGenericType<T extends HasF> {
	private T obj;
	public ReturnGenericType(T x) {
		obj = x;	}
	public T get() {
		return obj;	}}
```

### Migration compatibility

In an erasure-based implementation, generic types are treated as **second-class types** that cannot be used in some important contexts. This generic types are present only during static type checking, after which every generic type in the program  is erased by replacing it with a non-generic upper bound. For example, type annotations such as `List<T>` are erased to `List`, and ordinary type variables are erased to `Object` unless a bound is specified.

The core motivation for erasure is that it allows generified clients to be used with non-generified libraries, and vice versa. This is often called **migration compatibility**.

### The problem with erasure

The cost of erasure is significant. Generic types cannot be used in operations that explicitly refer to runtime types, such as casts, `instanceof` operations and `new` expressions. Because all the **type information** about the parameters is lost, whenever, you're writing generic code you must constantly be reminding yourself that it only appears that you have **type information** about a parameter. So when you write a piece of code like this:

```java
class Foo<T> {
	T var;}
```

it appears that when you create an instance of `Foo`:

```java
Foo<Cat> f = new Foo<Cat>();
```

the code in class `Foo` ought to know that it is now working with a `Cat`. The syntax strongly suggests that the type `T` is being substituted everywhere throughout the class. But it isn't, and you must remind yourself, "No, it's just an `Object`." whenever you're writing the code for the class.

> to turn off the warning, Java provides an annotation, the one that you see in the listing:
`@SuppressWarnings("unchecked)`

## Compensating for erasure

Anything that requires the knowledge of the exact type at runtime won't work because of erasuing:

```java
public class Erased<T> {
	private static final int SIZE = 100;
	public static void f(Object arg) {
		if(arg instance of T) {}   //Error
		T var = new T(); //Error
		T[] array = new T[SIZE]; //Error
		T[] array = (T)new Object[SIZE] //unchecked warning	}}
```

For example, the attempt to use `instanceof` in the previous program fails because the type information has been erased. If you introduced a type tag, a dynamic `isInstance()` can be used instead.

### Creating instances of types

The attempt to create a `new T()` in **Erased.java** won't work, partly because of erasure, and partly because the compiler cannot verify that `T` has a default (no-arg) constructor. The solution in Java is to pass in a **factory object**, and use that to make the new instance. A convenient factory object is just the `Class` object, so if you use a type tag, you can use `newInstance()`  to create a new object of that type.

### Arrays of generics

The type token `Class<T>` is passed into the constructor in order to recover from the erasure, so that we can create the actual type of array that we need, although the warning from the cast must be suppressed with `@SuppressWarnings("unchecked")`. Once we do get the actual type, we can return it and get the desired results. The runtime type of array is the exact type `T[]`.

## Bounds

Bounds allows you to place constraints on the parameter types that can be used with generics. Although this allows you to enforce rules about the types that your generics can be applied to, a potentially more important effect is that you can call methods that are in your bound types.

Because erasure removes type information, the only methods you can call for an unbounded generic parameter are those available for `Object`. If, however, you are able to constrain that parameter to be a subset of types, then you can call the methods int that subset. To perform this constraint, Java generics reuse the `extends` keywords. It's important for you to understand that `extends` has a significant different meaning in the context of generic bounds than it does ordinarily.

## Wildcards

> A particular behavior of arrays: **You can assign an array of a derived type to an array reference of the base type.**

This makes sense to the compiler, because it has a `Fruit[]` reference--why shouldn't it allow a `Fruit` object, or anything descended from `Fruit`, such as `Orange`, to be placed into the array? So at compiled time, this is allowed. The runtime array mechanism, however, knows that it's dealing with an `Apple[]` and throws an exception when a foreign type is placed into the array.

This arrangement for arrays is not so terrible, because you do find out at runtime that you've inserted an improper type. But one of the primary goals of generics is to move such error detection to compile time.

The real issue is that we are talking about the type of the container, rather than the type that the container is holding. Unlike arrays, generics do not have built-in covariance. This is because arrays are completely defined in the language and can thus have both compile-time and runtime checks built in, but with generics, the compiler and runtime system cannot know what you want to do with your types and what the rules should be.

### How smart is the compiler?

When you specify an `ArrayList<? extends Fruit>`, the argument for `add()` becomes `'? extends Fruit'`. From that description, the compiler cannot know which specific subtype of `Fruit` is required there, so it won't accept any type of `Fruit`. It doesn't matter if you upcast the `Apple` to a `Fruit` first - the compiler simply refuses to call a method (such as `add()`) if a wildcard is involved in the argument list.

With `contains()` and `indexOf()`, the arguments are of type `Object`, so there are no wildcards involved and the compiler allows the call. This means that it's up to the generic class designer to decide which calls are "safe", and to sue `Object` types for their arguments. To disallow a call when the type is used with wildcards, use the type parameter in the argument list.
 
### Contravariance

It's also possible to go the other way, and use **supertype wildcards**. Here you say that the wildcard is bounded by any base class of a particular class, by specifying `<? super MyClass>` or even using a type parameter: `<? super T>` (although you can not give a generic parameter a supertype bound; that is, you cannot say `<T super MyClass>`). This allows you to safely pass a typed object into a generic type. Thus, with supertype wildcards you can write into a `Collection`.

You can thus begin to think of subtype and supertype bounds in terms of how you can "write"(pass into a method) to a generic type, and "read"(return from a method) from a generic type.

> Supertype bounds relax the constraints on what you can pass into a method.

