# 闭包

本章介绍Groovy的闭包机制。所谓**闭包**，是指一段开放的、匿名的代码块，它可以拥有参数、返回值或者直接被赋值给一个变量使用。与其他常见的闭包不同，Groovy中的`Closure`不但可以引用在其内部作用域中声明的变量，而且也可以包含任何在其作用域之外定义的变量。除此之外，它还提供了一系列其他的优点，本章稍后将会详细介绍。

## 1. 语法

### 1.1. 定义闭包

闭包的定义遵从以下语法格式：

```
{ [closureParameters -> ] statements }
```

其中，`[closureParameters->]`表示可选参数列表，多个参数之间由`,`分隔；`statements`是指0条或多条Groovy语句。闭包的参数列表与方法的参数列表相似，可以给每个参数指定具体的类型，也可以不用指定。

如果闭包的形参已经确定了，那么字符`->`就是必需的，其作用是为了在闭包体中分隔具体的实参。`statements`语句部分可以包含0、1或者多条Groovy语句。

如下示例展示了正确定义闭包的方式：

```
1. { item++ }  
2. { -> item++ }
3. { println it }
4. { it -> println it }
5. { name -> println name }
6. { String x, int y ->
 println "hey ${x} the value is ${y}"
 }
7. { reader -> 
  def line = reader.readLine()
  line.trim()
 }
```

> 1. 引用了一个名叫item的变量的闭包
> 2. 通过增加一个箭头符号`->`可以从闭包代码中显示的区分参数
> 3. 使用了隐式参数 `it` 的闭包
> 4. 使用了显式参数 `it` 的闭包
> 5. 在使用闭包时最好为每个参数显式地命名
> 6. 接受了两个有类型的参数的闭包
> 7. 一个闭包可以包含多条语句

### 1.2. 把闭包当做一个对象

尽管闭包是一段代码块，但它本质上是`groovy.lang.Closure`类的一个实例，因此可以将它像其他变量一样赋给另一个变量或字段。

```groovy
1. def listener = { e -> println "clicked on $e.source" }
 assert listener instance Closure
2. Closure callback = { println 'Done!' }
3. Closure<Boolean> isTextFile = {
  File it -> it.name.endsWith('.txt')
 }
```

> 1. 可以将一个闭包赋给一个变量，因为闭包是`groovy.lang.Closure`类的实例
> 2. 如果不使用`def`的话，可以将一个闭包赋给一个类型为`groovy.lang.Closure`的变量
> 3. 你还可以视情况而定，利用`groovy.lang.Closure`的泛型类型为闭包指定返回值

### 1.3. 调用闭包

闭包作为一段**匿名代码块**，可以像调用其他方法一样被调用。如果你定义了如下没有带任何参数的闭包：

```groovy
def code = { 123 }
```
 
那么当你调用这个闭包的时候，它里面的代码才会被执行。你可以通过类似调用常规的方法的方式调用闭包：

```groovy
assert code() == 123
```

或者可以显式的使用`call`方法：

```groovy
assert code.call() == 123
```

这种方式对于接受了参数的闭包同样适用：

```groovy
1. def isOdd = { int i -> i%2 == 1 }
2. assert isOdd(3) == true
3. assert isOdd.call(2) == false
              
4. def isEven = { it%2 == 0}
5. assert isEven(3) == false
6. assert isEven.call(2) == true
```

> 1. 定义一个闭包，它接受了一个类型为`int`的参数
> 2. 闭包`isOdd`可以直接被调用
> 3. 也可以使用`call`方法来调用
> 4. 同样的规则适用于匿名参数`it`
> 5. 可以直接像正常方法`(args)`一样调用
> 6. 也可以使用`call`

和方法不同的是，闭包被调用后总是会返回一个值。下一章节将讨论如何声明闭包的参数，何时使用它们以及什么是匿名参数`it`。

## 2. 参数

### 2.1. 常规参数

闭包的参数遵从了和常规方法的参数一样的规则：

- 可选的类型
- 命名
- 可选的默认值

参数中间通过逗号`,`分隔：

```groovy
def closureWithOneArg = { str -> str.toUpperCase() }
assert closureWithOneArg('groovy') == 'GROOVY'
   
def closureWithOneArgWithExplicitType = { String str -> str.toUpperCase() }
assert closureWithOneArgWithExplicitType('groovy') == 'GROOVY'

def closureWithTwoArgs = { a, b -> a + b }
assert closureWithTwoArgs(1, 2) == 3
def closureWithTwoArgsAndExplicitTypes = { int a, int b -> a + b }
assert closureWithTwoArgsAndExplicitTypes(1, 2) == 3

def closureWithTwoArgsAndOptionalTypes = { a, int b -> a + b }
assert closureWithTwoArgsAndOptionalTypes(1, 2) == 3

def closureWithTwoArgsAndDefaultValue = { int a, int b = 2 -> a + b }
assert closureWithTwoArgsAndDefaultValue(1) == 3
```

### 2.2. 隐式参数

当闭包没有显式地定义参数列表时(使用`->`)，它总是会指定一个**匿名**的参数，取名叫`it`。这就意味着如下的代码：

```groovy
def greeting = { "hello, $it!" }
assert greeting('Patrick') == 'hello, Patrick!'
```

完全等同于下面的代码：

```groovy
def greeting = { it -> "hello, $it!" }
assert greeting('Patrick') == 'hello, Patrick!'
```

如果你想定义一个**不带任何参数**的闭包，而且必须保证在调用时也没有任何参数，那么你必须要**显式**的声明闭包的**空参数列表**：

```groovy
def magicNumber = { -> 42 }
// 这中调用方式将会失败，因为闭包不接受任何的参数
magicNumber(11)
```

### 2.3. 可变参数

像其他任何**方法**一样，可以为闭包声明可变的参数。**可变参方法**是指那些可以接受若干个参数，且最后的参数长度可变（或者是数组）的方法，就像如下的示例所示：

```groovy
1. def concat1 = { String... args -> args.join('') }
2. assert concat1('abc', 'def') == 'abcdef'
3. def concat2 = { String[] args -> args.join('') }
4. assert concat2('abc', 'def') == 'abcdef'
  def multiConcat = { int n, Strring... args -> 
  args.join('')*n
 }
 assert multiConcat(2, 'abc', 'def') == 'adcdefabcdef'
```

> 1. 该闭包接受多个字符串作为第一个参数
> 2. 可以带上若干个参数来调用这个闭包，而不需要显式的将参数封装在一个数组中
> 3. 和上面具有相同行为的闭包，不过这里使用了数组方式作为参数
> 4. 最后的那个参数是一个等同于数组的、显式的、可变长的参数

## 3. 委托机制

### 3.1. Groovy闭包 VS lambda表达式

如前面所说，Groovy中的闭包是作为[Closure类的实例](http://www.groovy-lang.org/closures.html#closure-as-object)存在的。这使得它和[Java8中的lambda表达式](http://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html)有着很大的区别。**委托(delegate)**是Groovy闭包中很重要的概念，但它和lambda中的delegate不同。Groovy具有修改闭包的委托（亦称**代理**）或委托策略的能力，这使得在Groovy中设计优美的**领域定义语言（DSL）**成为一种可能。

### 3.2. owner, delegate和this

要想理解委托的概念，我们必须先要解释闭包中`this`的含义。一个闭包实际上定义了3个隐含的变量：

- `this` 是指定义了闭包本身的那个**class**
- `owner` 拥有者，是指定义了闭包本身的那个对象, 它有可能是一个类或者一个闭包
- `delegate` 代理者，表示不管何时何地，调用方法或者解析属性时，没有定义消息接收者的一个第三方对象

#### 3.2.1. `this`的含义

在一个闭包中，调用`getThisObject`将会返回定义了这个闭包的类。就相当于显式的用了`this`：

```groovy
class Enclosing {
    void run() {
     def whatIsThisObject = { getThisObject() } //<1>
     assert whatIsThisObject() == this //<2>
     def whatIsThis = { this } //<3>
     assert whatIsThis() == this //<4>
    }
}
class EnclosedInnerClass {
 class Inner {
  Closure cl = { this } //<5>
 }
 void run() {
  def inner = new Inner()
  assert inner.cl() == inner //<6>
 }
}
class NestedClosures {
 void run() {
  def nestedClosures = {
   def cl = { this } //<7>
   cl()
  }
  assert nestedClosures() == this //<8>
  }
}
```

> 1. 在`Enclosing`类内部定义了一个闭包，它返回`getThisObject`
> 2. 调用闭包将返回`Enclosing`类的实例，闭包就是在`Enclosing`中定义的
> 3. 更常见的方式是，直接使用简写的`this`方式
> 4. 准确的返回了同一个对象
> 5. 在内部类中定义了一个闭包
> 6. 闭包返回的自然是内部类的实例，而不是它外层的类实例
> 7. 嵌套闭包，就像此处的`cl`，它是在闭包`nestedClosures`里面定义的闭包
> 8. 那么这个`this`对应的就是最近的外部类，而不是闭包

类似如下方式，可以从闭包类中调用方法：

```groovy
class Person {
 String name
 int age
 String toString() { "$name is $age years old" }
  
 String dump() {
  def cl = {
   String msg = this.toString() //<1>
   println msg
   msg
  }
  cl()
 }
}
def p = new Person(name:'tom', age:28)
assert p.dump() == 'tom is 28 years old'
```

> 1. 闭包`cl`调用了`this`的`toString()`方法，实际上调用的还是闭包所在对象的`toString`方法，也就是`Person`的实例。

#### 3.2.2 闭包的owner

闭包的`owner`和前面所讲的`this`很相似，仅仅有一点不同：它可以返回直接的闭合的对象，可能是一个闭包的，也可能是一个类的。

```groovy
class Enclosing {
 void run() {
  def whatIsOwnerMethod = { getOwner() } //<1>
  assert whatIsOwnerMethod() == this //<2>
  def whatIsOwner = { owner } //<3>
  assert whatIsOwner() == this //<4>
 }
}
class EnclosedInnerClass {
 class Inner {
  Closure cl = { owner } //<5>
 }
 void run() {
  def inner = new Inner()
  assert inner.cl() == inner //<6>
 }
}
class NestedClosures {
 void run() {
  def nestedClosures = {
   def cl = { owner } //<7>
   cl()
  }
  assert nestedClosures() == nestedClosures //<8>
 }
}
```

> 1. 在`Enclosing`类里定义了一个闭包，返回`getOwner`
> 2. 调用这个闭包将返回定义了这个闭包的类`Enclosing`的实例
> 3. 更常用的是，可以直接使用简写方式`owner`
> 4. 返回的还是同一个对象
> 5. 在内部类中定义一个闭包
> 6. 闭包中的`owner`返回的是内部类，而不是最上层的类
> 7. 定义了嵌套闭包`cl`
> 8. 但是此处的`owner`指的是闭合的闭包，而不是外部类，这一点和`this`有所不同

#### 3.2.3 闭包的委托

一个闭包的delegate可以通过`delegate`属性或者`getDelegate`方法访问。在Groovy中，它是一个很强大的概念，尤其是在构建DSL的时候。与`this`和`owner`表示的是闭包的**词法作用域**的含义不同，`delegate`是指一个由用户自己定义的对象，而且闭包可以使用它。默认情况下，`delegate`被设置为`owner`：

```groovy
class Enclosing {
 void run() {
  def cl = { getDelegate() } //<1>
  def cl2 = { delegate } //<2>
  assert cl() == cl2() //<3>
  assert cl() == this //<4>
  def enclosed = {
   { -> delegate }.call() //<5>
  }
  assert enclosed() == enclosed //<6>
 }
}
```

> 1. 可以通过调用`getDelegate`方法获取到闭包的`delegate`
> 2. 或者直接使用`delegate`属性
> 3. 两者返回的都是同一个对象
> 4. 有可能是闭合的类或者闭包
> 5. 在特殊的嵌套闭包的情形中
> 6. `delegate`和`owner`保持一致

一个闭包的`delegate`可以被任意对象更改。我们通过具体的实例来说明。假设我们定义了两个相互独立的类（彼此都不是对方的子类），只是他们都拥有一个相同的属性，名叫`name`：

```groovy
class Person {
 String name
}
class Thing {
 String name
}
def p = new Person(name: 'tom')
def t = new Thing(name: 'tea')
```

然后我们定义一个闭包，它可以获取到委托的`name`属性：

```groovy
def upperCasedName = { delegate.name.toUpperCase() }
```

然后通过更改闭包的委托，可以看到目标对象也改变了：

```groovy
upperCasedName.delegate = p
assert upperCasedName() == 'tom'
upperCasedName.delegate = t
assert upperCasedName() == 'tea'
```

仅从这一点看，`delegate`与闭包词法作用域中定义的变量的作用是相同的：

```groovy
def target = p
def upperCasedNameUsingVar = { target.name.toUpperCase() }
assert upperCasedNameUsingVar == 'tom'
```

但还是存在一些主要的区别：

- 在上个例子中，`target`是一个在闭包内被引用的局部变量
- 委托可以显式的使用。也就是说方法调用时不用带着`delegate.`。下一章节将进行详述。

#### 3.2.4. 委托机制

在一个闭包中，无论何时，属性都可以被访问，而不必显式的为其设置接收者对象。这就引出了**委托机制**：

```groovy
class Person {
 String name
}
def p = new Person(name: 'tom')
def cl = { name.toUpperCase() } //<1>
cl.delegate = p //<2>
assert cl() == 'TOM' //<3>
```

> 1. 在闭包的词法作用域内`name`没有引用任何变量
> 2. 我们可以将闭包的委托更换为`Person`类的实例
> 3. 此时方法调用就会成功

这段代码能够正常执行是因为属性`name`被显式地解析到了`delegate`对象上。这种解析闭包里的属性和方法的方式是非常强大的。使用时也不必显式的设置接收者`delegate.`，闭包默认的委托机制很好的完成了调用。闭包实际上定义了多种委托机制，你可以自由选择：

- `Closure.OWNER_FIRST` 是默认机制。如果闭包的**owner**拥有对应的属性或方法，那么他们将会调用**owner**的，否则，就会调用**delegate**的。
- `Closure.DELEGATE_FIRST` 刚好和上面的逻辑相反，首先调用**delegate**的，然后才是**owner**的。
- `Closure.OWNER_ONLY` 这种机制下，将只会调用**owner**的，**delegate**将会被忽略。
- `Closure.DELEGATE_ONLY` 这种机制则只会查找**delegate**的，而**owner**会被忽略。
- `Closure.TO_SELF` 这种机制是为那些想实现更高级的元编程技术和想实现自定义机制的开发者提供的。它既不会作用于**owner**，也不会作用于**delegate**，而只会作用于闭包类自己。通常，它只会在你实现了自己的`Closure`子类后才会有意义。

让我们用以下代码来说明默认的**“owner first”**机制：

```groovy
class Person {
 String name
 def pretty = { "my name is $name" } //<1>
 String toString() {
  pretty()
 }
}
class Thing {
 String name //<2>
}

def p = new Person(name: 'Tom')
def t = new Thing(name: 'Tea')

assert p.toString() == 'my name is Tom' //<3>
p.pertty.delegate = t //<4>
assert p.toString() == 'my name is Tom' //<5>
```

> 1. 此处定义了一个闭包成员，它引用了“name”
> 2. `Person`类和`Thing`类都定义了一个`name`属性
> 3. 使用默认机制的话，`name`属性会优先被解析为**owner**的
> 4. 如果我们将闭包的`delegate`改为类`Thing`的一个实例`t`
> 5. 那么最终的结果仍不变，因为`name`会被优先解析到闭包的`owner`上

但是，我们可以考虑更改闭包的实现机制：

```groovy
p.pretty.resolveStrategy = Closure.DELEGATE_FIRST
assert p.toString() == 'my name is Tea'
```

通过更改`resolveStrategy`，我们修改了Groovy解析“隐含this”引用的方式。在这种情况下，将会优先查找**delegate**的`name`，如果找不到，才会去查找**owner**的。因为delegate对象，即类`Thing`的实例，中也定义了`name`，所以用到的就是它的值。

为了说明**delegate first**、**delegate only**、**owner first**以及**owner only**之间的区别，我们假设其中一种委托（比如owner）并没有对应的方法或者属性：

```groovy
class Person {
 String name
 int age
 def fetchAge = { age }
}
class Thing {
 String name
}

def p = new Person(name: 'Jeff', age: 30)
def t = new Thing(name: 'ball')
def cl = p.fetchAge
cl.delegate = p
assert cl() == 30
cl.delegate = t
assert cl() == 30
cl.resolveStrategy = Closure.DELEGATE_ONLY
cl.delegate = p
assert cl() == 30
cl.delegate = t
try {
 cl()
 assert false
} catch (MissingPropertyException ex) {
 // "age" is not defined on the delegate
}
```

在这个例子中，我分别定义了两个类，`Person`和`Thing`。其中二者都有一个`name`属性，不同的是`Person`还有一个`age`属性。`Person`类还声明了一个引用的`age`属性的闭包。我们可以将默认的**owner first**委托机制更改为**delegate only**委托机制。因为闭包的拥有者是`Person`类，所以我们可以通过成功调用闭包，验证出闭包的委托就是`Person`类的一个实例。但是如果我们用类`Thing`的一个实例作为委托来调用闭包，发现失败了，程序抛出了`groovy.lang.MissingPropertyException`异常。尽管闭包是在`Person`类中定义的，但是拥有者却没被使用。
