# Generic and Parameterized Types

## Fundamentals

### What is a parameterized or generic type?

> A generic type is a type with formal type parameters. A parameterized type is an instantiation of a generic type which actual arguments.

A generic type is a **reference type** that has one or more type parameters. These type parameters are later replaced by type arguments when the generic type is instantiated ( or declared).

- Example of a generic type:

```java
interface Collection<E> {
 public void add(E e);
 public Iterator<E> iterate();
}
```

- Example of a parameterized type:

```java
Collection<String> collection = new LinkedList<String>();
```

### How do I define a generic type?

> Like a regular type, but with a type parameter declaration attached.

A generic type is a reference type that has one or more type parameters. In the definition of the generic type, the type parameter section follows the type name. It is a comma separated list of identifiers and is delimited by angle brackets.

Example:

```java
class Pair<X, Y> {
 private X first;
 private Y second;
 public Pair(X x, Y y) {
  this.first = x;
  this.second = y;
 }
 public X getFirst() {
  return first;
 }
 public Y getSecond() {
  return second;
 } 
 public void setFirst(X x) {
  this.first = x;
 }
 public void setSecond(Y y) {
  this.second = y;
 }
}
```

### Are there any types that cannot have type parameters?

> All types, except enum types, anonymous inner classes and exception classes, can be generic...

Almost all reference types can be generic. This includes **classes**, **interfaces**, **nested (static) classes**, **nested interfaces**, **inner (non-static) classes**, and **local classes**.

The following types cannot be generic:

- **Anonymous inner classes**: They can implement a parameterized interface or extend a parameterized class, but they cannot themselves be generic classes. A generic anonymous inner class would be nonsensical. Anonymous class do not have a name, but the name of a generic class is needed for declaring an instantiation of the class  and providing  the type arguments. Hence, generic anonymous classes would be pointless.
- **Exception types**: A generic class must not directly or indirectly be derived from class `Throwable`. Generic exceptions or errors are disallowed because the exceptions handling mechanism is a runtime mechanism and the Java virtual machine doesn't know anything about Java generics. The JVM would not be capable of distinguishing between different instantiation of generic exception types. Hence, generic exception types would be pointless.
- **Enum types**: Enum type cannot have type parameters. Conceptually, an enum and its enum values are `static`. Since type parameters cannot be used in any static context, the parameterization of an enum type would be pointless.

### How is a generic type instantiated?

> By providing a type argument per type parameter.

The type argument list is a comma separated list that is delimited by angle brackets and follows the type name. The result is a so-called **parameterized type**.

#### example of a generic type

```java
class Pair<X, Y> {
 private X first;
 private Y second;
}
```

#### example of a concrete parameterized type

```java
public void printPair(Pair<String, Long> pair) {} 
```

and:

```java
Pair<String, Long> pair = new Pair<String, Long>("maxsize", 1024L);
```

#### wildcard instantiations

In addition to concrete instantiation there so-called **wildcard instantiations**. They do not have concrete types as arguments, but so-called **wildcards**. A wildcard  is a syntactic construct with a `?` that denotes not just one type, but a family of types.

Example of a wildcard parameterized type:

```java
public void printPair(Pair<?, ?> pair) {
....
}
Pair<?, ?> pair = new Pair<String, Long>("maximum", 1024L);
printPair(pair);
```

#### raw types

It is permitted to leave out the type arguments altogether and not specify type arguments at all. A generic type without type arguments is called **raw type** and is only allowed for reasons of compatibility with non-generic  Java code.

> Use of raw types is discouraged.

### Why do instantiations of generic type share the same runtime type?

> Because of **type erasure**.

the compiler translates the generic and parameterized types by a technique called **type erasure**. Basically, the compiler elides all information related to type parameters and type arguments.

For instance, `ArrayList<String>` and `ArrayList<Long>` have the same **raw type** `ArrayList` after the **type erasure**.

### Can I cast to a parameterized type?

> Yes, you can, but under certain circumstances it is not type-safe and the compiler issues an "unchecked" warning.

All instantiations of a generic type share the same runtime type representation, namely representation of the **raw type**. For instance, the instantiations of generic type `List`, such as `List<String>`, `List<Long>`, `List<Date` etc. have different **static types** at compile time, but the same **dynamic type** `List` at runtime.

A cast consists two parts:

- a static type check performed by the compiler at compile time
- a dynamic type check performed by the Java Virtual Machine at runtime.

The static part sorts out nonsensical casts, that can not succeed. Such as the cast from `String` to `Date` or from `List<String>` to `List<Date>`.

The dynamic part uses the runtime type information and performs a type check at runtime. It raises a `ClassCastException` if the dynamic type of the object is not the target type (or a subtype of the target type) of the cast. Examples of casts with a dynamic part are cast from `Object` to `String` or from `Object` to `List<String>`. There are the so-called **downcasts**, **from a supertype down to a subtype.**

Not all casts have a dynamic part. **Some casts are just static casts and require no type check at runtime**. Examples are the casts between primitive types, such as the cast from `long` to `int`, or `byte` to `char`. Another example of static casts are the so-called **upcasts**, **from a subtype up to a supertype**, such as the casts from `String` to `Object` or from `LinkedList<String>` to `List<String>`. **Upcasts are casts that are permitted, but not required**. They are automatic conversions that the compiler performs implicitly, even without an explicit cast expression in the source code, which means, the cast is not required and usually omitted. However, if an upcast appears somewhere in the source code then it is a purely static cast that does not have a dynamic part.

**Type casts with a dynamic part are potentially unsafe, when the target type of the cast is a parameterized type**. The runtime type information of a parameterized type is non-exact, because all instantiations of the same generic type share the same runtime type representation. The virtual machine cannot distinguish between different instantiations of the same generic type. Under circumstances the dynamic part of a cast can succeed although it should not.

Example of unchecked cast:

```java
void m1() {
 List<Date> list = new ArrayList<Date>();
 ...
 m2(list);
}
void m2(Object arg) {
 ...
 List<String> list = (List<String>) arg;  //unchecked warning
 ...
 m3(list);
 ...
}
void m3(List<String> list) {
 ...
 String s = list.get(0); // ClassCastException
 ...
}
```

The cast from `Object` to `List<String>` in method `m2` looks like a cast to `List<String>` , but actually is a cast from `Object` to the raw type `List`. It would succeed even if the object referred to were a `List<Date>` instead of a `List<String>`.

After this successful cast we have a reference variable  of type `List<String>` which refers to an object of type `List<Date>`. When we retrieve elements from that list we would expect `String`s, but in fact we receive `Date`s and a `ClassCastException` will occur in a place where nobody had expected it.

We are prepared to cope with `ClassCastException`s when there is a cast expression in the source code, but we do not expect `ClassCastException`s when we extract an element from a list of strings. This sort of unexpected `ClassCastException` is considered a violation of the type-safety principle. In order to draw attention to the potentially unsafe cast the compiler issues an "unchecked" warning when it translates the dubious cat expressions.

As a result, **the compiler emits "unchecked" warnings for every dynamic cast whose target type is a parameterized type**. Note that an upcast whose target type is a parameterized type does not lead to an "unchecked" warning because the upcast has no dynamic part.

### Can generic types have static members?

> Yes.

**Generic types can have static members, including static fields, static methods and static nested types**. Each of these static members exists once per enclosing type, that is, independently of the number of objects of the enclosing type and regardless of the number of instantiations of the generic type that may be used somewhere in the program. The name of the static member consists - as is usual for static members - of the scope (package and enclosing type) and the member's name. If the enclosing type is generic, then the type in the scope qualification must be the raw type, not a parameterized type.

## Concrete Instantiations

### What is a concrete parameterized type?

> A instantiation of a generic type where all type arguments are concrete type rather than wildcards.

Examples of concrete parameterized types are `List<String>` or `Map<String, Date>`, not `List<? extends Number>` or `Map<String, ?>`.

### Is `List<Object>` a supertype of `List<String>`?

> No, different instantiations of the same generic type for different concrete type arguments have no type relationship.

#### Arrays are covariant but parameterized types are not covariant

The array `Object[]` is a supertype of the array `String[]`, because `Object` is a supertype of `String`. This type relationship is known as **covariance**. The super-subtype-relationship of the component types extends into the corresponding array types. No such a relationship exists for instantiations of generic types.

#### Various consequences of lacking the super-subtype-relationship

Example:

```java
void printAll(List<Object> list) {
 for(Object o : list) {
  System.out.println(o);
 }
}
ArrayList<String> list = new ArrayList<String>();
... fill list ...
printAll(list);  //error
```

A `ArrayList<String>` object cannot be passed as argument to a method that ask for `ArrayList<Object>` because the two types are instantiations of same generic type, but for different type arguments, and for this reason they are not compatible with each other.

On the other hand, instantiations of different generic types for the same type argument can be compatible.
Example:

```java
void printAll(Collection<Object> c) {
 for(Object o : c) {
  System.out.println(o);
 }
}
List<String> list = new ArrayList<String>();
... fill list ...
printAll(list); //fine
```

A `List<Object>` is compatible to a `Collection<Object>` because the two types are instantiations of a generic supertype and its generic subtype and the instantiations are for the same type argument `Object`.

> Compatibility between instantiations of the same generic type exist only among wildcard instantiations and concrete instantiations that belong to the family of instantiations that the wildcard instantiation denotes.

### Can I use a concrete parameterized type like any other type?

> Almost.

They can **not** be used for the following purposes:

- for creation of arrays
- in exception handling
- in a class literal
- in a `instanceof` expression

### Can I create an array whose component type is a concrete parameterized type?

> No, because it is not type-safe.

Arrays are **covariant**, so `Object[]` is a supertype of `String[]` and a string array can be accessed through a reference variable of type `Object[]`.

Example of covariant arrays:

```java
Object[] objArr = new String[10]; //fine
objArr[0] = new String();
```

The runtime type information regarding the component type is used when elements are stored in an array in order to ensure that no "alien" elements can be inserted.

Example of array store check:

```java
Object[] objArr = new String[10];
objArr[0] = new Long(0L); //Compiles; fails at runtime with ArrayStoreException
```

The reference variable of type `Object[]` refers to a `String[]`, which means that only strings are permitted as elements of the array. When an element is inserted into the array, the information about the array's component type is used to perform a type check - the so-called **array store check**.

Problems arise when an array holds elements whose type is a concrete parameterized type. Because of **type erasure**, parameterized types do not have exact runtime type information. As a consequence, the array store check does not work because it uses the dynamic type information regarding the array's (non-exact) component type for the array store check.

Example of array store check in case of parameterized component type:

```java
Pair<Integer, Integer>[] intPairArr = new Pair<Integer, Integer>[10]; //illegal
Object[] objArr = intPairArr;
objArr[0] = new Pair<String, String>(","); //should fail, but would succeed
```

Since we are trying to add a `Pair<String, String>` to a `Pair<Integer, Integer>[]` we would expect that the type check fails. However, the JVM cannot detect any type mismatch here: **at runtime, after type erasure, `objArr` would be have the dynamic type `Pair[]` and the element to be stored has the matching dynamic type `Pair`. Hence the store check succeeds, although it should not.

In order to prevent programs that are not type-safe all arrays holding elements whose type is a concrete parameterized type are illegal. For the same reason, arrays holding elements whose type is a wildcard parameterized type are banned, too. Only arrays with an **unbounded wildcard parameterized type** as the component type are permitted.

### Can I declare a reference variable of an array type whose component type is a concrete parameterized type?

> Yes, you can, but you should not, because it is neither helpful nor type-safe.

You can declare a reference variable of an array type whose component type is a concrete parameterized type. Arrays of such a type must not be created. Hence, this reference variable cannot refer to an array of its type. All that it can refer to is `null`, an array whose component type is a non-parameterized subtype of the concrete parameterized type, or an array whose type is the corresponding raw type. Neither of these case is overly useful, yet they are permitted.

Example of an array reference variable with parameterized component type:

```java
Pair<String, String>[] arr = null;  // fine
arr = new Pair<String, String>[2]; // error: generic array creation
```

the code shows that **a reference variable of type `Pair<String, String>[]` can be declared, but the creation of such an array is rejected.** But we can have the reference variable of type `Pair<String, String>[]` refer to an array of a non-parameterized subtype.

Example of another array reference variable with parameterized component type:

```java
class Name extends Pair<String, String> {...}
Pair<String, String>[] arr = new Name[2]; // fine
```

Which raises the question: how useful is such an array variable if it never refers to an array of its type? Let's consider an example:

Example of an array reference variable refering to array of subtypes; not recommended:

```java
void printArrayOfStringPairs(Pair<String, String>[] pa) {
 for (Pair<String, String> p : pa) {
  if (p != null) {
   println(p.getFirst() + " " + p.getSecond());
  }
 }
}
Pair<String, String>[] createArrayOfStringPairs() {
 Pair<String, String>[] arr = new Name[2];
 arr[0] = new Name("Tim", "Duncan"); //fine
 arr[1] = new Pair<String, String>("a", "b"); //fine, (but cause ArrayStoreException)
 return arr;
}
void extractStringPairFromArray(Pair<String, String>[] arr) {
 Name name = (Name)arr[0];
 Pair<String, String> p1 = arr[1]; //fine
}
void test() {
 Pair<String, String>[] arr = createArrayOfStringPairs();
 printArrayOfStringPairs(arr);
 extractStringPairsFromArray(arr);
}
```

Example improved:

```java
void printArrayOfStringPairs(Pair<String, String>[] pa) {
 for (Pair<String, String> p : pa) {
  if (p != null)
   println(p.getFirst() + " " + p.getSecond());
 }
}
Name[] createArrayOfStringPairs() {
 Name[] arr = new Name[2];
 arr[0] = new Name("tim", "duncan");
 arr[1] = new Pair<String, String>("a", "b"); // error
 return arr;
}
void extractStringPairsFromArray(Name[] arr) {
 Name name = arr[0]; // fine (needs no cast)
 Pair<String, String> p1 = arr[1];
}
```

Since an array reference variable whose component type is a concrete parameterized type can never refer to an array of its type, such a reference variable does not really make sense. No matter how you put it, you should better refrain from using array reference variable whose component type is a concrete parameterized type. **Note, that the same holds for array reference variable whose component type is a wildcard parameterized type**. Only array reference variable whose component type is an **unbounded wildcard** parameterized type make sense.
