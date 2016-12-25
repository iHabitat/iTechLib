## Synchronizing a method
every method declared with the **synchronized** keyword is a **critical section** and java only allows the execution of one of the critical section of an object.

**static methods** have a different behavior. Only one execution thread will access one of the static methods declared with the *synchronized* keyword, but another thread can access other non-static method of an object of that class. You have to be very careful with this point, because two threads can access two different *synchronized* methods if one is static and the other one is not. If both methods change the same data, you can have data inconsistency errors.
