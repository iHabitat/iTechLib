## 如何将本地仓库上传到github

### 1. 创建本地仓库
直接在指定位置创建仓库，然后进行初始化。

```shell
$ mkdir app-test-repo
$ cd app-test-repo/
$ git init
Initialized empty Git repository in /Users/ThomsonTang/Github/app-test-repo/.git/
```

当然了，按照约定习惯，应该在仓库根目录下添加 *README*文件：

```
$ touch README.md
$ echo "Hello, app test repo" > README.md
```

此时，可以看看当前仓库中文件的状态：

![git status][git-status]

由上可知，README文件还未提交到本地仓库，可以先将其提交：

```
$ git add README.md
$ git commit -m "add README file"
[master (root-commit) fac0868] add README file
 1 file changed, 1 insertion(+)
 create mode 100644 README.md
```

这时再看文件状态：

```
$ git status
On branch master
nothing to commit, working directory clean
```

一切ok，本地仓库的基本操作已完成，接下来创建远程仓库了。

### 2. 在github上创建远程仓库
要想将本地仓库上传或者提交到github上，就得需要在github上创建一个repository（仓库名称随意指定，当然最好是和本地仓库名保持一致），这部分可以参考github官网上的说明步骤进行操作。本例中，我创建的repository为：**app-test-repo**, 其对应的仓库地址为：**git@github.com:ThomsonTang/app-test-repo.git**。仓库创建完毕后，接下来就需要将本地仓库和远程仓库进行关联。

### 3. 关联本地仓库和远程仓库
要将本地仓库和github上的远程仓库相关联，就要使用到git的命令`git remote`了。如下：

```
$ git remote add origin git@github.com:ThomsonTang/app-test-repo.git
```

其中，`origin`表示是对远程仓库的一个引用别名，这个名称可以自定义，默认的是`origin`；后面的url便是`origin`所引用的远程仓库的地址。
关联完成后可以通过命令`git remote`或`git remote -v`（显示详情）查看一下关联的结果：

```
$ git remote
origin
$ git remote -v
origin	git@github.com:ThomsonTang/app-test-repo.git (fetch)
origin	git@github.com:ThomsonTang/app-test-repo.git (push)
```

以上结果表示关联成功。接下来便是提交啦！

### 4. 提交本地仓库至github上
关联成功后，便可使用`git push`命令将本地仓库中的资源提交至远程：

```
$ git push origin master
Counting objects: 3, done.
Writing objects: 100% (3/3), 242 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To git@github.com:ThomsonTang/app-test-repo.git
 * [new branch]      master -> master
```

其中，`origin`便是之前关联的远程仓库的别名，`master`则是指主分支。
最后，就可以在github上看到提交的内容了。剩下的就是随心的在本地仓库操作，新增、修改文件等，然后使用`git push`命令提交至github即可。

[git-status]: http://img2.ph.126.net/MgS4o8-_4pwg_Gv1dua_QQ==/6608701500166528826.png
