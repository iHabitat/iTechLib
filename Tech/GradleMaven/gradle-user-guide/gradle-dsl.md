
## Basics
 Fist, gradle scripts are *configuration scripts*. For example, as a build script executes, it configures an object of type Project. This object is called the delegate object of the script.

```
build script    ====>>(delegate to instance of)  Project
init script     ====>>(delegate to instance of)  Gradle
setting script  ====>>(delegate to instance of)  Settings  
```
 The properties and methods of the delegate object are available for you to use in the script.    Second, each Gradle script implements the *Script* interface. This interface defines a number of properties and methods which you can use in the script.

## Build script structure
 A build script is made up of zero or more statements and script blocks. *Statements* can include method calls, properties assignments, and local variable definitions. *Script block* is a method call which takes a closure as a parameter. The *closure* is treated as _configuration closure_. 
## Core Types
### Project
 This interface is the main API you use to interact with Gradle from your build file.

#### Lifecycle
 There is a one-to-one relationship between a Project and a *build.gradle* file.  

- Create a *Settings* instance for the build.
- Evaluate the *settings.gradle* script, if present, against the *settings* object to configure it.
- Use the configured *settings* object to create the hierarchy of Project instances.
- Finally, evaluate each Project by executing its *build.gradle* file, if present, against the project. The project are evaluated in breadth-wise order, such that a project is evaluated before its child projects. 
