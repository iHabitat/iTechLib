# 设计模式之接口隔离原则ISP（Interface Segregation Principle）

## 定义

> 1. Clients should not be forced to depend upon interfaces that they don’t use. （客户端不应该被迫依赖它不需要的接口）
> 2. The dependency of one class to another one should depend on the smallest possible interface.（类之间的依赖关系应该建立在尽可能小的接口上）

## 问题
假设类A通过接口I依赖类B，类C也通过接口I依赖类D。如果接口I对于类A和类C来说不是最小接口，那么类B和类D就必须实现他们不需要的方法。
如下代码描述了上述问题：

1. 接口I：

```java
	public interface I {
		public void method1();
		public void method2();
		public void method3();
		public void method4();
		public void method5();
	}
```

2. 类A：

```java
	public class A {
		    public void depend1(I i) {
		        i.method1();
		    }
		    public void depend2(I i) {
		        i.method2();
		    }
		    public void depend3(I i) {
		        i.method3();
		}
	}
```

3. 类B：

```java
public class B implements I {
	@Override public void method1() {
	    System.out.println("类B实现方法1");
	}
	@Override public void method2() {
	    System.out.println("类B实现方法2");
	}
	@Override public void method3() {
	    System.out.println("类B实现方法3");
	}
	@Override public void method4() {
	}
	@Override public void method5() {
	}
```

4. 类C:
  
```java
public class C {
	public void depend1(I i) {
	    i.method1();
	}
	public void depend2(I i) {
	    i.method4();
	}
	public void depend3(I i) {
	    i.method5();
	}
}
```

5. 类D：

```java
public class D implements I {
	@Override public void method1() {
	    System.out.println("类D实现方法1");
	}
	@Override public void method2() {
	}
	@Override public void method3() {
	}
	@Override public void method4() {
	    System.out.println("类D实现方法4");
	}
	@Override public void method5() {
	    System.out.println("类D实现方法5");
	}
}
```

6. 场景类Client：

```java
public class Client {
	public static void main(String[] args) {
	    // 类A通过接口I依赖类B
	    A a = new A();
	    a.depend1(new B());
	    a.depend2(new B());
	    a.depend3(new B());
	    // 类C通过接口I依赖类D
	    C c = new C();
	    c.depend1(new D());
	    c.depend2(new D());
	    c.depend3(new D());
	}
```

如上程序所示，我们可以很明显的看到类B和类D在实现接口I时，不得不实现一些空方法。这显然不是好的设计。

## 解决方案
建立**单一接口**，不要建立臃肿庞大的接口。接口尽量细化，同时接口中的方法尽量少。将臃肿的接口I拆分为**独立的几个接口**，类A和类C分别与他们需要的接口建立依赖关系。也就是要遵循**接口隔离原则**。修改后的代码如下：

```java
public interface I1 {
	public void method1();
}

public interface I2 {
	public void method2();
	public void method3();
}

public interface I3 {
	public void method4();
	public void method5();
}

public class A {
	public void depend1(I1 i) {
	    i.method1();
	}
	
	public void depend2(I2 i) {
	    i.method2();
	}
	
	public void depend3(I2 i) {
	    i.method3();
	}
}

public class B implements I1, I2 {
	@Override public void method1() {
	    System.out.println("类B实现方法1");
	}
	
	@Override public void method2() {
	    System.out.println("类B实现方法2");
	}
	
	@Override public void method3() {
	    System.out.println("类B实现方法3");
	}
}

public class C {
	public void depend1(I1 i) {
	    i.method1();
	}
	public void depend2(I3 i) {
	    i.method4();
	}
	public void depend3(I3 i) {
	    i.method5();
	}
}

public class D implements I1, I3 {
	@Override public void method1() {
	    System.out.println("类D实现方法1");
	}
	
	@Override public void method4() {
	    System.out.println("类D实现方法4");
	}
	
	@Override public void method5() {
	    System.out.println("类D实现方法5");
	}
}
```

## 实践

接口隔离原则是对接口进行规范约束，其包含以下几层含义：

- 接口要尽量小  
	这是隔离原则的核心定义，但是也要注意“小”是有限度的。首先要保证不能违反**单一职责原则**，如果过小，会造成接口数量过多，使得设计更复杂了。
	  
- 接口要高内聚  
	**高内聚**就是要提高接口、类、模块的处理能力，减少对外的交互，使接口用最少的方法完成最多的事情。那么具体到接口隔离原则就是，要求在接口中尽量少公布public的方法，接口是对外的   承诺，承诺越少，对开发越有利，变更的风险也就越少。
	  
- 为依赖接口的类定制服务  
	**定制服务**就是要单独为一个个体提供优良的服务。只提供访问者需要的方法，它不需要得方法则隐藏起来。只有专注的为一个模块提供定制服务，才能建   立最小的依赖关系

接口隔离原则是对接口的定义，同时也是对类的定义，接口和类尽量使用原子接口和原子类来组装。但是，这个原子该怎么划分却是一大难题，可以根据以下几个规则来衡量：

- 一个接口只服务于一个子模块或业务逻辑
- 通过业务逻辑压缩接口中的public方法，接口时常去回顾，尽量让接口达到短小精悍
- 已经被污染了的接口，尽量去修改，若变更的风险较大，则采用**适配器模式**进行转换处理
- 了解环境，拒绝盲从
