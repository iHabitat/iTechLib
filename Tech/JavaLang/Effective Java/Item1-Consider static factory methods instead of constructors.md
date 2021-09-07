# Creating and destroying objects

## Item 1: Consider static factory methods instead of constructors

### what is **static factory method**?

A class can provide a public **static factory method**, which is simply a static method that returns an instance of the class.

An example from `Boolean`:

```java
public static Boolean valueOf(boolean b) {
 return b ? Boolean.TRUR : Boolean.FALSE;
}
```

> Note that a static factory method is not the same as the *Factory method* pattern from *Design Patterns*.

### Advantages of static factory method

#### 1. unlike constructors, they have names

You can give any names to the method what you want to give, because they are the methods of a class.

A class can have only a single constructor with a given signature. Programmers have been known to get around this restriction by providing two constructors whose parameter lists differ only in the order of their parameter types. This is a really bad idea.

Because they have names, static factory don't share the restriction discussed in the previous paragraph. **In cases where a class seems to require multiple constructors with the same signature, replace the constructors with static factory methods and carefully chosen names to highlight their differences**.  

#### 2. unlike constructors, they are not required to create a new object each time they're invoked

This allows **immutable classes** to use preconstructed instances, or to **cache instances** as they are constructed, and dispense them repeatedly to **avoid creating unnecessary duplicated objects**.

The ability of static factory methods to return the same object from repeated invocations allows classes to maintain strict control over what instances exist at any time.

> Classes that do this are said to be *instance-controlled*. There are several reasons to write instance-controlled classes. Instance control allows a class to guarantee that it is a **singleton** or **noninstantiable**. Also, it allows an immutable class to make the guarantee that no two equal instances exist: `a.equals(b)` if and only if `a==b`. If a class makes this guarantee, then its clients can use the `==` operator instead of the `equals(Object)` method, which may result in improved performance. Enum types provide this guarantee.

#### 3. unlike constructors, they can return an object of any subtype of their return type

This gives you great flexibility in choosing the class of the returned object.

**One application of this flexibility is that an API can return objects without making their classes public**. This technique lends itself to *interface-based frameworks*, where interfaces provide natural return types for static factory methods.

> Interfaces can't have static methods, so by convention, static factory methods for an interface named `Type` are put in a noninstantiable class named `Types`. For example, the class  `java.util.Collections` in Java Collections Framework and the Guava's `ImmutableList` class.

**Not only can the class of an object returned by a public static factory method be nonpublic, but the class can vary from invocation to invocation depending on the values of the parameters to the static factory**. This feature is indicated obviously by the class `java.util.EnumSet`.

The class of the object returned by a static factory method need not even exist at the time the class containning the method is written. Such flexible static factory methods from the basis of *service provider frameworks*, such as the Java Database Connectivity API（JDBC）.
