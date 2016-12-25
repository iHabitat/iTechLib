# Golang

## 1、Download the Go distribution
download the Mac OSX package installer:  
**go1.3.1.darwin-amd64-osx10.8.pkg**

## 2、Install the Go tools
> WARN: If you are upgrading from an older version of Go you must first remove the existing version.

the Mac OS X package installer install the Go distribution to `/usr/local/go` and put he `/usr/local/go/bin` directory in the PATH environment variables.

## 3、Test Installation

```
$ vim hello.go
```

then write the code below:

```go
package main
import “fmt”
func main() {
	fmt.Printf(“hello/n”)
}
```

and run the program:

```
$ go run hello.go
```

it print: `hello`

## 4、Code Organization

### workspace
Go code must be kept in a *workspace*. A workspace is a directory hierarchy with three directories as its root:
- **src**: contains source files organized into packages(one package per directory)
- **pkg**: contains package objects
- **bin**: contains executable commands

The *go* tool builds source packages and installs the resulting binaries to the *pkg* and *bin* directories.

### The **GOPATH** environment variable
The **GOPATH** environment variable specifies the location of your workspace.  To get started, create a workspace directory and set **GOPATH** accordingly. The workspace can be located wherever you like, I use **$HOME/go** here.
> Note: the **GOPATH** must not be the same path as your Go installation.

```shell
$ mkdir $HOME/go
$ export GOPATH=$HOME/go
```
For convenience, add the workspace’s *bin* subdirectory to the **PATH**:

```shell
$ export PATH=$PATH:$GOPATH/bin
```

### Package paths
The packages from the standard library are given short paths such as "fmt" and "net/http". For your own packages, you must choose a base path that is unlikely to collide with future additions to the standard library or other external libraries.

If you keep your code in a source repository somewhere, then you should use the root of that source repository as your base path. For instance, if you have a GitHub account at github.com/user, that should be your base path.

> Note that you don't need to publish your code to a remote repository before you can build it. It's just a good habit to organize your code as if you will publish it someday. In practice you can choose any arbitrary path name, as long as it is unique to the standard library and greater Go ecosystem.

Here use *github.com/thomsontang* as base path. Create a directory inside your workspace in which to keep source code:

```shell
$ mkdir -p $GOPATH/src/github.com/thomsontang
```

### The first program
first choose a package path (we’ll use *github.com/thomsontang/hello*) and create a corresponding package directory inside workspace:

```
$ mkdir $GOPATH/src/github.com/thomsontang/hello
```
next, create a file named *hello.go* inside the directory, containing the following Go code:

```go
package main
import "fmt"
func main() {
    fmt.Printf("hello world!\n")
}
```
Now, we can build and install that program with the go tool:

```shell
$ go install github.com/thomsontang/hello
```

Note that you can run this command from anywhere on your system. The go tool finds the source code by looking for the *github.com/user/hello* package inside the workspace specified by **GOPATH**. You can also omit the package path if you run **go install** from the package directory:

```shell
$ cd $GOPATH/src/github.com/thomsontang/hello
$ go install
```

This command builds the hello command, producing an executable binary. It then installs that binary to the workspace's bin directory as hello (or, under Windows, hello.exe). In our example, that will be **$GOPATH/bin/hello**, which is **$HOME/go/bin/hello**.

The go tool will only print output when an error occurs, so if these commands produce no output they have executed successfully.

You can now run the program by typing its full path at the command line:

```
$ $GOPATH/bin/hello
Hello, world.
```

Or, as you have added **$GOPATH/bin** to your PATH, just type the binary name:

```
$ hello
Hello, world.
```

### The first library
Let’s write a library and use it from the *hello* program. Again, the first step is to choose a package path (here we use *github.com/thomsontang/newmath*) and create the package directory:

```
$ mkdir $GOPATH/src/github.com/thomsontang/newmath
```

next, create a file named *sqrt.go* in that directory with following code:

