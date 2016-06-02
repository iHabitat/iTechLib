# Maven

## What is Maven
- build tool (构建工具)
- projet management tool (项目管理工具)

> From [apache.org][1]: Apache Maven is a software project management and **comprehension** tool. Based on the concept of a **project object model** (POM), Maven can manage a project's build, reporting and documentation from a central piece of information.

> From others: Maven is a project management tool which encompasses **a project object model, a set of standards, a project lifecycle, a dependency management system, and logic for executing plugin goals at defined phases in a lifecycle**.

## Convention over Configuration
开发人员不需要创建**构建过程**， 也不必接触**配置细节**， maven会主动为你提供项目的默认行为。当你创建一个maven项目后，maven会自动为你创建默认的项目结构，开发人员只需要将相关文件放入对应的目录即可，不需要在pom文件中做任何配置。  
下图为maven默认生成的项目结构：

![default-project-structure][image-1]

## Installation
具体的安装步骤可参考[maven官网][2]。
以下介绍几个实践点，这些都不是必须要做的，但却是比较有用的。

### TIPS
#### 1. M2\_HOME
设置M2\_HOME环境变量指向Maven的安装目录。

\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`
$ echo $M2\_HOME
/Users/ThomsonTang/Apps/apache-maven-3.2.1
\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`

#### 2. MAVEN\_OPTS
**mvn**命令实际上是执行了**java**命令，因此java的命令参数同样可以适用于**mvn**命令。这就需要通过**MAVEN\_OPTS**环境变量来配置相关参数了。
通常情况下，需要设置MAVEN\_OPTS为：

\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`
export MAVEN\_OPTS=“-Xms128m -Xmx512m”
\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`

#### 3. User-specific Configuration and Repository
该目录下放置了maven的本地仓库目录 **repository**。默认情况下，\~/.m2目录下只有repository仓库目录，但是大多数情况下，需要用户将*$M2\_HOME/conf/settings.xml*文件复制到*\~/.m2/settings.xml* 以配置用户范围的settings.xml。

你可以选择配置使用*$M2\_HOME/conf/settings.xml* 文件或者是*\~/.m2/settings.xml*文件，前者是全局范围的，对系统上的所有用户都起作用；后者是用户范围的，只对当前用户起作用。

> 推荐使用用户范围的settings.xml文件，这样不会对其他用户产生影响，而且也有利于maven本身的升级。

#### 4. Do not use built-in maven of IDE
有些IDE在发布新版本时本身会内嵌maven，但是这个内嵌的maven通常都比较新，不够稳定，所以不推荐使用，而是新建一个用户自己安装的maven。另外在使用maven时通常不止在IDE中使用，还会经常在命令行终端中使用，如果使用IDE内嵌的maven，可能会导致同样的构建产生不同的行为结果。
下图展示了eclipse中内嵌的maven。

![eclipse-built-in-maven][image-2]


## Project Object Model
maven项目的核心是pom.xml。POM定义了项目的基本信息， 用来描述项目如何构建，声明项目的依赖等等。POM文件中的内同通常可以分为四部分：

1. 项目通用信息（项目名称、URL、组织信息、开发者信息等）
2. 构建自定义设置（自定义Maven构建过程，源码目录，输出目录、插件配置）
3. 构建环境配置（profiles）
4. pomg关系集配置（父POM、坐标、依赖、modules）

### Super POM
类似java中的**Object**类，包含在Maven的安装包中。

\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`
$ jar xf $M2\_HOME/lib/maven-model-builder-3.2.1.jar
$ vim org/apache/maven/model/pom-4.0.0.xml
\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`

### Coordinate
> 世界上任何一个构件都可以使用Maven坐标唯一标识

如下显示了maven构件的一组坐标定义：

\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`
<groupId>org.springframework</groupId>
<artifactId>spring-core</artifactId>
<version>4.0.3.RELEASE</version>
<packaging>jar</packaging>
\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`

Maven坐标的元素包括：

- groupId：定义当前构件隶属的实际项目。（不应该对应项目隶属的组织或公司）
- artifactId: 定义实际项目中的一个Maven模块，表示构件真正的名字。推荐使用实际项目名称作为artifactId的前缀。
- version：定义项目当前所处的版本。
- packaging：定义maven项目的打包方式，通常与所生成构件的文件扩展名对应。
- classifier：用来帮助定义一些附属构件。不能直接定义项目的classifier，因为附属构件不是由项目直接默认生成的，而是由附加的插件帮助生成的。

以上5个元素中，**groupId**、**artifactId**、**version**是必须定义的，**packaging**是可选的（默认为jar），而**classifier**是不能直接定义的。
项目构件的文件名是与坐标相对应的：

![spring-core-jar][image-3]

### Inheritance and Aggregation
Maven的继承关系：

\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`
<parent>
	    <groupId>com.bj58.qdyw.business</groupId>
	    <artifactId>com.bj58.qdyw.business</artifactId>
	    <version>0.0.1-SNAPSHOT</version>
 </parent>
 <artifactId>com.bj58.qdyw.business.house</artifactId>
\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`

Maven的聚合关系：

\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`
<modules>
	<module>com.bj58.qdyw.business.car</module>
	<module>com.bj58.qdyw.business.house</module>
	<module>com.bj58.qdyw.business.core</module>
	<module>com.bj58.qdyw.business.client</module>
	<module>com.bj58.qdyw.business.contract</module>
	<module>com.bj58.qdyw.business.job</module>
	<module>com.bj58.qdyw.business.sale</module>
	<module>com.bj58.qdyw.business.huangye</module>
	<module>com.bj58.qdyw.business.common</module>
</modules>
\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`

## Dependency
一个简单的依赖配置声明如下：

\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`
<dependencies>
	<dependency>
	    <groupId>com.bj58.qdyw.business</groupId>
	    <artifactId>com.bj58.qdyw.business.core</artifactId>
	    <version>0.0.1-SNAPSHOT</version>
	</dependency>
 <dependencies>
\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`

### 依赖范围
依赖范围是用来控制maven依赖和三种**classpath**（compile classpath、test classpath、runtime classpath）之间的关系。

- **compile**: 编译依赖范围，对三种classpath都有效。（默认的依赖范围）
- **test**: 测试依赖范围，对test classpath有效。例如junit
- **provided**: 已提供依赖范围，对compile classpath、test classpath有效，runtime classpath无效。例如 servlet-api
- **runtime**: 运行时依赖范围，对test classpath和runruntime classpath有效，compile classpath无效。例如 JDBC驱动
- **system**: 系统依赖范围，需要通过systemPath元素显示指定依赖文件的路径。对compile classpath、test classpath有效，runtime classpath无效
- **import**: 导入依赖范围

### 传递性依赖
简单的说就是**dependencies of a dependency**，即**依赖的依赖**

![maven-dependency-transitive][image-4]

> maven会自动解析每个直接依赖的pom文件，然后将必要的间接依赖以传递性依赖的方式引入到当前项目中。

### 依赖调解
既然maven存在传递性依赖，那么就会存在同一个项目通过不同的依赖关系依赖同一个构件的不同版本。例如，某项目A有如下两种依赖关系：

1. A-\>B-\>C-\>E(1.0)
2. A-\>D-\>E(2.0)

这样就导致两条依赖路径上存在两个版本的E，那么到底该依赖哪个版本呢？同样，还可能存在如下情况：

1. A-\>B-\>D(1.0)
2. A-\>C-\>D(2.0)

这同样属于重复依赖。这就需要maven具体可以调解这种冲突的机制，也就是maven **依赖调解** 的原则：

1. **路径最近者优先**。
2. **第一声明者优先**。

第一原则不能解决的问题，通过第二原则来解决。因此，以上实例中，E(1.0)的路径长度为3，E(2.0)的路径长度为2，这样E(2.0)就会被maven解析。第二种情况则需要判断两者就POM文件中依赖声明的顺序，最靠前的依赖将被成功解析。

### TIPS

#### 1. Excluded Dependency （排除依赖）
通过使用**exclusions**元素声明要排除的依赖

``
\<dependency\>
\<groupId\>com.bj58.passport\</groupId\>
\<artifactId\>com.bj58.passport.webclient\</artifactId\>
\<version\>1.0.3-SNAPSHOT\</version\>
\<exclusions\>
\<exclusion\>
\<artifactId\>commons-logging\</artifactId\>
\<groupId\>commons-logging\</groupId\>
\</exclusion\>
\</exclusions\>
\</dependency\>
``

> 声明exclusion的时候，只需要指定groupId和artifactId，而不需要version，因为前两者就可以确定依赖关系图中的某个依赖。

#### 2. Define Constant Variable for Same Kind of Dependencies （为同类依赖“定义常量”）
使用**properties**元素定义Maven属性，然后引入Maven属性。

\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`
<properties>
	    <spring.version>4.0.6.RELEASE</spring.version>
</properties>
<dependencies>
	<dependency>
	        <groupId>org.springframework</groupId>
	        <artifactId>spring-core</artifactId>
	        <version>${spring.version}</version>
	    </dependency>
	    <dependency>
	        <groupId>org.springframework</groupId>
	        <artifactId>spring-context</artifactId>
	        <version>${spring.version}</version>
	    </dependency>
</dependencies>
\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`

#### 3. Use \<dependencyManagement\> Elements
**dependencyManagement**元素可以灵活地实现子模块继承父模块的依赖配置，它对**dependencies**元素下的依赖起到了约束作用，在**dependencyManagement**元素下的依赖声明不会引入实际的依赖。

在父POM中使用**dependencyManagement**声明依赖配置能够统一项目中依赖的版本，这样子模块继承父模块之后，在使用依赖的时候就无须声明依赖的版本了，也就不会发生多个子模块使用依赖版本不一致的情况，从而降低依赖冲突的可能性。

#### 4. Optimize Dependency（优化依赖）
`mvn dependency:list`

`mvn dependency:tree`

`mvn dependency:analyze`

## Repository
Maven提供的依赖管理功能使得我们能够很方便的配置自己的项目所依赖的构件。那么，当配置完这些构件后，Maven从哪里获得这些构件呢？为此，Maven提供了**Repository(仓库)**来下载构件。Maven仓库可以分为两类：

* Local Repository（本地仓库）
* Remote Repository（远程仓库）

用户可以在**settings.xml**配置文件中指定本地仓库的路径。  
最常见的远程仓库是**[中央仓库Central][3]**。中央仓库搜集了基本所有的开源项目构件，可以供开发者浏览、搜索、下载构件。当然了还有一些其他的远程仓库，例如JBoss自己独立维护一套Maven仓库。

有了中央仓库，我们开发项目时所用到的大部分依赖都可以通过中央仓库获得，但是这会导致一个问题，中央仓库只有一个，而全球的开发者人数众多，大家都从中央仓库下载构件，会对中央仓库造成巨大的压力，同时对开发者来说下载速度过慢，费时且费力。
另外，在实际的开发过程中，同一公司或组织内部的不同项目组或开发人员之间存在彼此使用对方的项目的情况，这就需要我们能够将自己的项目通过Maven发布到某个仓库，供他人使用。这就产生了一种特殊的仓库，即**私服**。

> 私服，是一种特殊的远程仓库。通常指公司或组织自己创建的Maven仓库，一般只在公司内部局域网使用，可以为开发人员提供更快的访问和下载速度。同时项目小组或开发人员还可以将自己开发的项目发布到私服上，以供公司其他人员使用。

通常通过**镜像**的方式将私服配置在用户范围的**settings.xml**文件中。

\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`
<mirrors>
	    <mirror>
	        <id>nexus</id>
	        <mirrorOf>*</mirrorOf>
	        <url>http://10.58.120.19:8058/nexus/content/groups/public</url>
	    </mirror>
	    <mirror>
	        <id>nexus-public-snapshots</id>
	        <mirrorOf>public-snapshots</mirrorOf>
	        <url>http://10.58.120.19:8058/nexus/content/repositories/snapshots/</url>
	    </mirror>
	</mirrors>
 \`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`

## Build Lifecycle

> * A build lifecycle is an organized sequence of phases that exist to give order to a set of goals.
> * A Maven lifecycle consists of a sequence of named phases: prepare-resources, compile, package, and install among others.
> * For the person building a project, this means that it is only necessary to learn a small set of commands to build any Maven project, and the POM will ensure they get the results they desired.

Maven包含[三套生命周期][4]：

### Clean Lifecycle

* pre-clean
* clean
* post-clean

### Default Lifecycle
常用的几个default生命周期阶段如下所示：

* validate
* compile
* test
* package
* install
* deploy

### Site Lifecycle
* pre-site
* site
* post-site
* site-deploy

> Maven在执行某个生命周期阶段时，该阶段之前的生命周期阶段也会被执行。

### Maven Snapshot

> SNAPSHOT is a special version that indicates a current development copy. Unlike regular versions, Maven checks for a new SNAPSHOT version in a remote repository for every build.

默认情况下，SNAPSHOT的构件，Maven每天会从私服上自动获取更新。你可以通过如下命令实现每次构建都更新：

\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`
$  maven clean package -U
\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`

也可以直接在**settings.xml**配置文件中设置对应的更新策略：

\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`
<snapshots>
	<enabled>true</enabled>
	<updatePolicy>always</updatePolicy>
</snapshots>
\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`\`

[1]:	http://maven.apache.org/
[2]:	http://maven.apache.org/download.cgi#Installation
[3]:	http://search.maven.org/
[4]:	http://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Lifecycle_Reference

[image-1]:	/Users/ThomsonTang/Pictures/images/dev/mvn-default-project-structure.png
[image-2]:	/Users/ThomsonTang/Pictures/images/dev/eclipse-built-in-maven.jpg
[image-3]:	/Users/ThomsonTang/Pictures/images/dev/maven-coordinate.png
[image-4]:	/Users/ThomsonTang/Pictures/images/dev/maven-dependency-transitive.png