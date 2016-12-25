# Error Handing with Exceptions

> The basic philosophy of Java is that “badly formed code will not be run.”

## Basic exceptions

An **exceptional condition** is a problem that prevents the continuation of the current method or scope. With an exceptional condition, you cannot continue processing because you don’t have the information necessary to deal with the problem in the current context. All you can do is jump out of the current context and  relegate that problem to a higher context.

When you throw an exception, several things happen:

- First, the exception object is created in the same way that any Java object is created: on the heap with **new**.
- Then the current path of execution (the one you could’t continue) is stopped and the reference for the exception object is ejected from the current context.
- At this point the exception-handling mechanism takes over and begins to look for an appropriate place to continue executing the program.

Throw the exception, which allows you -in the current context- to abdicate responsibility for thinking about the issue further.

Exception allow you to think of everything that you do as a **transaction**.

One of the most important aspects of exceptions is that if something bad happens, they don’t allow a program to continue along its ordinary path.

## Catching an exception

### the **try** block
If you don’t want a **throw** to exit the method, you can set up a special block within that method to capture the exception. This is called the **try** block.

### Exception handler
Exception handlers immediately follow the **try** block and  are denoted by the keyword **catch**. Each **catch** clause (exception handler) is like a little method that takes one and only one argument of a particular type.