# Item 38: Check parameters for validity

Most methods and constructors have some restrictions on what values may be passed into their parameters. You should clearly document all such restrictions and enforce them with checks at the beginning of the method body.

For public methods, use the Javadoc *@throws* tag to document the exception that will be thrown if a restriction on parameter value is violated.
Nonpublic methods should generally check their parameters using *assertions*.

It is particularly important to check the validity of parameters that are not used by a method but are stored away for later use.
It is critical to check the validity of constructor parameters to prevent the construction of an object that violates its class invariants.

The fewer restrictions that you place on parameters, the better, assuming the method can do something reasonable with all of the parameter values that it accepts.