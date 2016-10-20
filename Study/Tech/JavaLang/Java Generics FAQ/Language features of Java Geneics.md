# Generic and Parameterized Types

## Fundamentals

### What is a parameterized or generic type?

> A generic type is a type with formal type parameters. A parameterized type is an instantiation of a generic type which actual arguments.

A generic type is a **reference type** that has one or more type parameters. These type parameters are later replaced by type arguments when the generic type is instantiated ( or declared).

- Example of a generic type:
 
```java
interface Collection<E> {
	public void add(E e);
	public Iterator<E> iterate();}
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
		this.second = y;	}
	public X getFirst() {
		return first;	}
	public Y getSecond() {
		return second;	}	
	public void setFirst(X x) {
		this.first = x;	}
	public void setSecond(Y y) {
		this.second = y;	}}
```
### Are there any types that cannot have type parameters?

> All types, except enum types, anonymous inner classes and exception classes, can be generic...

Almost all reference types can be generic. This includes **classes**, **interfaces**, **nested (static) classes**, **nested interfaces**, **inner (non-static) classes**, and **local classes**.

The following types cannot be generic:

- **Anonymous inner classes**: They can implement a parameterized interface or extend a parameterized class, but they cannot themselves be generic classes. A generic anonymous inner class would be nonsensical. Anonymous class do not have a name, but the name of a generic class is needed for declaring an instantiation of the class  and providing  the type arguments. Hence, generic anonymous classes would be pointless.
- **Exception types**: A generic class must not directly or indirectly be derived from class `Throwable`. Generic exceptions or errors are disallowed because the exceptions handling mechanism is a runtime mechanism and the Java virtual machine doesn't know anything about Java generics. The JVM would not be capable of distinguishing between different instantiation of generic exception types. Hence, generic exception types would be pointless.
- **Enum types**: Enum type cannot have type parameters. Conceptually, an enum and its enum values are `static`. Since type parameters cannot be used in any static context, the parameterization of an enum type would be pointless.