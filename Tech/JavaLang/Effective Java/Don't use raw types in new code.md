
# Item23: Don't use raw types in new code

## Example: non-generics collections

Before release 1.5, this would have been an exemplary[^exemplary] collection declaration:

```java
// Now a raw collection --- don't do this!
// My stamp collection. Contains only Stamp instance.
private final Collection stamps = ...;
// Erroneous insertion of coin into stamp collection
stamps.add(new Coin(...));

for(Iterator i = stamps.iterator(); i.hasNext();) {
 Stamp s = (Stamp)i.next(); // Throws ClassCastException
 ... // Do something with the stam
}
```

**As mentioned throughout this book, it pays to discover errors as soon as possible after they are made, ideally at compile time.** In this case, you don't discover the error until JVM throws `ClassCastException` at runtime.

## Example: generics collections

With generics, you replace the  comment with an improved type declaration for the collection that tell the compiler the information that was previously hidden in the comment:

```java
// Parameterized collection type -- type safe
private final Collection<Stamp> stamps = ...;
```

When `stamps` is declared with a parameterized type, the erroneous insertion generates a compile-time error message that tells you exactly what is wrong.

As an added benefit, you no longer have to cast manually when removing elements from collections. **The compiler inserts invisible casts for you and guarantees that they won't fail.**

```java
for (Stamp s : stamps) { // No cast
 ... // Do something with the stamp
} 
```

or

```java
for(Iterator<Stamp> i = stamps.iterator(); i.hasNext(); ) {
 Stamp s = i.next(); // No cast necessary
 ... // Do something with the stamp
}
```

**If you use raw types, you lose all the safety and expressiveness benefits of generics**.

## Using `List<Object>`

While you shouldn't use raw types such as `List` in new code, it is fine to use types that are parameterized to allow insertion of arbitrary objects, such as `List<Object>`.

> Difference between the raw type `List` and the parameterized type `List<Object>`:  
the former has opted out of generic type checking, the latter has explicitly told the compiler that it is capable of holding objects of any type.

There are subtyping rules for generics, and `List<String>` is a subtype of the raw type `List`, but not of the parameterized type `List<Object>`. As a consequence, **you lose type safety if you use a raw type like `List`, but not if you use a parameterized type like `List<Object>`.

## For unknown elements

You might be tempted to use a raw type for a collection whose element type is unknown and doesn't matter. For example:

```java
// Use of raw type for unknown element type -- don't do this!
static int numElementsInCommon(Set s1, Set s2) {
 int result = 0;
 for(Object o1: s1) {
  if(s2.contains(o1)) {
   result++;
  }
 }
 return result;
}
```

If you want to use a generic type but you don't know or care what the actual type parameter is, you can use the **unbounded wildcard types** with a question mark. For example, the unbounded wildcard type for the generic type `Set<E>` is `Set<?>`(read "set of some type"). It is the most general parameterized `Set` type, capable of holding any set.

```java
//Unbounded wildcard type -- typesafe and flexible
static int numElementsInCommon(Set<?> s1, Set<?> s2) {
 int result = 0;
 for(Object o1 : s1) {
  if(s2.contains(o1)) {
   result++;
  }
 }
 return result;
}
```

**You can't put any element (other than `null`) into a `Collection<?>`.**

## Exceptions

There are two minor exceptions to the rule that you should not use raw types in new code, both of which stem from the fact that generic type information is erased at runtime.

- **You must use raw types in class literals.** In other words, `List.class`, `String[].class` and `int.class` are all legal, but `List<String>.class` and `List<?>.class` are not.

- The second exception to the rule concerns the `instanceof` operator. Because generic type information is erased at runtime, it is illegal to use the `instanceof` operator on parameterized types other than unbounded wildcard types. The use of unbounded wildcard types in place of raw types does not affect the behavior of the `instanceof` operator in any way. In this case, the angle brackets and question marks are just noise. **This is preferred way to use the `instanceof` operator with generic types:

```java
//Legitimate use of raw type -- instanceof operator
if (o instanceof Set) { // Raw type
 Set<?> m = (Set<?>)o; // Wildcard type
 ...
}
```

Note that once you've determined that `o` is a `Set`, you must cast it to the wildcard type `Set<?>`, not the raw type `Set`. This is a checked cast, so it will not cause a compiler warning.

## Summary

Using raw types can lead to exceptions at runtime, so don't use them in new code. They are provided only for compatibility and interoperability with legacy code that predates the introduction of generics.

[^exemplary]: serving as a desirable model; representing the best of its kind
