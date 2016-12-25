# Concurrent
Chapter2	Thread Safety

Building concurrent programs require the correct use of threads and locks. Write thread-safe code is, as its core, about managing access to state, and in particular to shared, mutable state. Informally,  an object's state is data, stored in state variables such as instance or static fields. By shared, we mean that a variable could be accessed from multiple threads; by mutable, we means that its value could change during its lifetime.

Whether an object needs to be thread-safe depends on whether it will be accessed from multiple threads. This is a property of how the object is used in a program, not what it does. Making an object-safe requires using synchronization to coordinate access to its mutable state.

The primary mechanism for synchronization in Java is the synchronized keyword, which provides exclusive locking, but the term “synchronization” also includes the use of volatile variables, explicit locks, and atomic variables.

What is Thread Safety?
A class is thread-safe if it behaves correctly when accessed from multiple threads, regardless of the scheduling or interleaving of the execution of those threads by the runtime environment, and with no additional synchronization or other coordination on the part of the calling code.

Stateless objects are always thread-safe.

Atomicity
•	Race Conditions
•	To ensure thread safety, check-then-act operations(like lazy initialization) and read-modify-write operations(like increment) must always be atomic.
•	Where practical, use existing thread-safe objects, like AtomicLong, to manage your class's state.

Locking
•	To preserve state consistency, update related state variables in a single atomic operation.
•	Intrinsic locks
A synchronized block has two parts: a reference to an object that will serve as the lock, and a block of code to be guarded by that lock. A synchronized method is shorthand for a synchronized block that spans an entire method body, and whose lock is the object on which the method is being invoked. (Static synchronized method use the Class object for the lock)
•	Every Java object can implicitly  act as a lock for purpose of synchronization; these built-in locks are called intrinsic locks or monitor lock.
•	Intrinsic locks in Java acts as mutexes (or mutual exclusion locks), which means that at most one thread may own the lock.
•	Reentrancy
Because intrinsic locks are reentrant, if a thread tries to acquire a lock that it already holds, the request succeeds. Reentrancy means that locks are acquired on a per-thread rather than per-invocation basis. Reentrancy is implemented by associating with each lock an acquisition count and an owning thread.

Guarding State with Locks
•	It is a common mistake to assume that synchronized needs to be used only when writing to shared variables; this is simply not true.
•	For each mutable state variable that may be accessed by more than one thread, all access to that variable must be performed with the same lock held. In this case, we say that the variable is guarded by that lock.
•	Every shared, mutable variable should be guarded by exactly one lock. Make it clearly to maintainers which lock that is.
•	A common locking convention is to encapsulate all mutable state with an object and to protect it from concurrent access by synchronizing any code path that accesses mutable state using the object's intrinsic lock. This pattern is used by many thread-safe classes, such as Vector and other synchronized collection classes.
•	For every invariant that involves more than one variable, all the variables involved in that invariant must be guarded by the same lock.

Liveness and Performance
	Avoid holding locks during lengthy computations or operations at risk of 	not completing quickly such as network or console I/O.


Chapter3	Sharing Objects

Locking and Visibility
Locking is not just about mutual exclusion; it is also about memory visibility. To ensure that all threads see the most up-to-date values of shared mutable variables, the reading and writing threads must synchronized on a common lock.

Volatile Variables
Java also provides an alternative, weaker form of synchronization, volatile variables, to ensure that updates to a variable are propagated predictably to other threads. When a field is declared volatile, the compiler and runtime are put on notice that this variable is shared and that operation on it should not be reordered with other memory operations. Volatile variable are not cached in register or in caches where they are hidden from other processors, so a read of a volatile variable always returns the most recent write by any thread.

Locking can guarantee both visibility and atomicity; volatile variables can only guarantee visibility.

Thread Local
A more formal means of maintaining thread confinement is ThreadLocal, which allows you to associate  a per-thread value with a value-holding object. Thread-Local provides get and set accessor methods that maintain a separate copy of the value for each thread that uses it, so a get returns the most recent value passed to set from the currently executing thread. Thread-local variables are often used to prevent sharing in designs based on mutable Singletons or global variables.

Immutability
Immutable objects are always thread-safe.
