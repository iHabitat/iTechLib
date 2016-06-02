# IDEA无法安装findbug插件问题解决

在IDEA的插件库中下载findbug-ide插件，并选择enable，此时idea提示重启。重启后，出现了如下错误：

```
Plugin 'FindBugs-IDEA' failed to initialize and will be disabled.  Please restart IntelliJ IDEA.

java.lang.ExceptionInInitializerError
    at java.lang.Class.forName0(Native Method)
    at java.lang.Class.forName(Class.java:249)
    at com.intellij.openapi.components.impl.ComponentManagerImpl$ComponentsRegistry.a(ComponentManagerImpl.java:409)
    at com.intellij.openapi.components.impl.ComponentManagerImpl$ComponentsRegistry.a(ComponentManagerImpl.java:398)
    at com.intellij.openapi.components.impl.ComponentManagerImpl$ComponentsRegistry.access$000(ComponentManagerImpl.java:384)
    at com.intellij.openapi.components.impl.ComponentManagerImpl.a(ComponentManagerImpl.java:107)
    at com.intellij.openapi.components.impl.ComponentManagerImpl.init(ComponentManagerImpl.java:89)
    at com.intellij.openapi.project.impl.ProjectImpl.init(ProjectImpl.java:296)
    at com.intellij.openapi.project.impl.ProjectManagerImpl.a(ProjectManagerImpl.java:280)
    at com.intellij.openapi.project.impl.ProjectManagerImpl.access$400(ProjectManagerImpl.java:83)
    at com.intellij.openapi.project.impl.ProjectManagerImpl$10.compute(ProjectManagerImpl.java:580)
    at com.intellij.openapi.project.impl.ProjectManagerImpl$10.compute(ProjectManagerImpl.java:576)
    at com.intellij.openapi.progress.impl.ProgressManagerImpl$4.run(ProgressManagerImpl.java:240)
    at com.intellij.openapi.progress.impl.ProgressManagerImpl$TaskRunnable.run(ProgressManagerImpl.java:471)
    at com.intellij.openapi.progress.impl.ProgressManagerImpl$6.run(ProgressManagerImpl.java:281)
    at com.intellij.openapi.progress.impl.ProgressManagerImpl$2.run(ProgressManagerImpl.java:178)
    at com.intellij.openapi.progress.ProgressManager.executeProcessUnderProgress(ProgressManager.java:209)
    at com.intellij.openapi.progress.impl.ProgressManagerImpl.executeProcessUnderProgress(ProgressManagerImpl.java:212)
    at com.intellij.openapi.progress.impl.ProgressManagerImpl.runProcess(ProgressManagerImpl.java:171)
    at com.intellij.openapi.application.impl.ApplicationImpl$10$1.run(ApplicationImpl.java:645)
    at com.intellij.openapi.application.impl.ApplicationImpl$8.run(ApplicationImpl.java:419)
    at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:439)
    at java.util.concurrent.FutureTask$Sync.innerRun(FutureTask.java:303)
    at java.util.concurrent.FutureTask.run(FutureTask.java:138)
    at java.util.concurrent.ThreadPoolExecutor$Worker.runTask(ThreadPoolExecutor.java:895)
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:918)
    at java.lang.Thread.run(Thread.java:695)
    at com.intellij.openapi.application.impl.ApplicationImpl$1$1.run(ApplicationImpl.java:149)
Caused by: com.intellij.diagnostic.PluginException: edu/umd/cs/findbugs/FindBugs : Unsupported major.minor version 51.0 [Plugin: FindBugs-IDEA]
    at com.intellij.ide.plugins.cl.PluginClassLoader.b(PluginClassLoader.java:130)
    at com.intellij.ide.plugins.cl.PluginClassLoader.a(PluginClassLoader.java:77)
    at com.intellij.ide.plugins.cl.PluginClassLoader.loadClass(PluginClassLoader.java:66)
    at java.lang.ClassLoader.loadClass(ClassLoader.java:247)
    at org.twodividedbyzero.idea.findbugs.core.FindBugsPluginImpl.<clinit>(FindBugsPluginImpl.java:128)
    ... 28 more
Caused by: java.lang.UnsupportedClassVersionError: edu/umd/cs/findbugs/FindBugs : Unsupported major.minor version 51.0
    at java.lang.ClassLoader.defineClass1(Native Method)
    at java.lang.ClassLoader.defineClassCond(ClassLoader.java:637)
    at java.lang.ClassLoader.defineClass(ClassLoader.java:621)
    at java.lang.ClassLoader.defineClass(ClassLoader.java:471)
    at com.intellij.util.lang.UrlClassLoader._defineClass(UrlClassLoader.java:195)
    at com.intellij.util.lang.UrlClassLoader.defineClass(UrlClassLoader.java:191)
    at com.intellij.util.lang.UrlClassLoader._findClass(UrlClassLoader.java:167)
    at com.intellij.ide.plugins.cl.PluginClassLoader.b(PluginClassLoader.java:124)
    ... 32 more
```

看异常信息大概是版本不兼容导致的。google一下，找到了如下解决方案：
[Issue 76: UNSupportedClassVersionError](https://code.google.com/p/findbugs-idea/issues/detail?id=76)

的确是jvm版本太低导致，需要将idea的**JVMVersion**更改为**1.7**