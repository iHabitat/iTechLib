Github是如今最流行的代码托管平台。普通用户在Github上创建的仓库都是公共的，任何人都可以看的到。虽然Github也为用户提供了私有仓库，但是那得需要花银子购买才行。那么问题来了？我想不花银子就拥有类似Github的私有仓库，该何如实现呢？*Google*了一下，大家都推荐使用**Dropbox**。刚好我最爱的云端存储工具就是Dropbox。参考网友提供的方法，我也开始创建自己的私有仓库了。

首先，在Dropbox的根目录下，创建一个用来存放你的仓库的目录。例如，我的就叫**Git**：

![](https://www.evernote.com/l/ANyqsKJIQdVAraFYP9g2c1znw1l110g7vu8B/image.png)
	
打开终端，进入刚才创建的那个目录，并在其中创建一个 **bare repository**:

```shell
$ cd ~/Dropbox/Git
$ git init --bare testrepo.git
Initialized empty Git repository in /Users/ThomsonTang/Dropbox/Git/testrepo.git/
```

如下图：

![](https://www.evernote.com/l/ANwP_Hkt9jlAerIU-J45el7ikKC_FF3elroB/image.png)

此时，可以在 Dropbox 目录中看到创建的这个仓库的目录：

![](https://www.evernote.com/l/ANztXQyGnHRGg7sbJ6_5yhnv3JL8jNPD560B/image.png)

打开这个目录，可以看到git的相关默认资源：

![](https://www.evernote.com/l/ANzukNhx9XJHM5sxzTzZS89CXgabGan0OtYB/image.png)
  
至此，bare repository 就创建完成了。接下来，就该创建项目了。  

创建项目的仓库。选择相应的目录，创建git仓库，并在其中添加或修改文件，并提交。(本例假定要在**~/Projects**目录下创建一个名为**mytestrepo**的仓库)

```shell
$ cd ~/Projects
$ mkdir mytestrepo
$ git init
$ echo "test building github repos using dropbox" > testfile.txt
$ git add testfile.txt
$ git commit -m "add a test file"
$ echo "add more text" >> testfile.txt
$ git commit testfile.txt -m "append more text"
```

仓库创建完毕，并新增了一个`testfile.txt`文件。我们可以看看这个文件的内容以及git的日志：

```shell
$ cat testfile.txt
$ git log
```

之后，我们需要做的就是将`mytestrepo`这个本地仓库与前面在**Dropbox**目录下创建的那个仓库（我们可以称其为**远程仓库**）相关联。

```shell
$ git remote add origin ~/Dropbox/Git/testrepo.git 
```

然后执行push操作，

```shell
$ git push -u origin master
```

这时可以从状态栏中看到 Dropbox 正在同步文件。但是如果打开Dropbox中创建的仓库，即**"远程仓库"**，是看不到提交的 **testfile.txt** 文件的，因为那里面都是git的**元信息**。到此创建远程仓库的整个过程都已完毕，接下来我们可以clone远程仓库到本地，开始正式使用自己的私有仓库。

最后，我们尝试将仓库`clone`到本地，查看我们提交的文件。

```shell
$ git clone ~/Dropbox/Git/testrepo.git testrepo
Cloning into 'testrepo'...
done.
```

之后打开 `testrepo` 目录，可以看到我之前提交的 `testfile.txt` 文件。

```shell
$ cd testrepo
$ ls
testfile.txt
$ cat testfile.txt
test building github repos using dropbox
add more text
```

当然，也可以查看提交的日志：

```shell
$ git log
Mon Jun 8 22:52:19 2015 +0800 1a1e6bd (HEAD, origin/master, origin/HEAD, master) append more text  [ThomsonTang]
Mon Jun 8 22:50:38 2015 +0800 9b67d73 add a test file  [ThomsonTang]
```

OK，整个过程就是这么简单，不用花银子，同样拥有了自己的私有仓库。赶紧动手创建吧！
