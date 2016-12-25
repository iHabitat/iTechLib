# Java Language Specification

## Threads and Locks

### Synchronization

The **synchronization** is implemented using **monitors**. Each object in java is associated with a monitor, which a thread can lock or unlock.

The **synchronized** statement computes a reference to an object; it then attempts to perform a lock action on that object’s monitor and does not processed further until the lock action has successfully completed.

A **synchronized** method automatically performs a lock action when it is invoked; its method body is not executed until the lock action has successfully completed. There are two cases for it:

- **instance method** locks the monitor associated with the instance. [this object]
- **static method** locks the monitor associated with the **Class** object that represents the class in which the method is defined.

The java language neither prevents nor requires detection of deadlock conditions.
 
There are some other mechanisms, such as reads and writes of **volatile** variables and the use of  classes in the **java.util.concurrent** package, provide alternative ways of synchronization. 

----
