## 更好地使用Git

### 1. 开启git命令自动补全

在Unix系统下，运行以下指令来获取脚本：（windows的？咱已不用windows多年！）

```shell
$ cd ~
$ curl https://raw.githubusercontent.com/git/git/master/contrib/completion/git-completion.bash -o ~/.git-completion.bash
```

然后修改~/.bash_profile文件，增加一下代码：

```shell
if [ -f ~/.git-completion.bash ];
then . ~/.git-completion.bash
fi
```

### 2. git忽略文件
可以设置让某些无用的或讨厌的文件或目录不参与git管理，也就是让git忽略这些文件。做法很简单，只需在项目目录下创建一个**.gitignore**文件，然后在其中列出要忽略的文件或目录即可，当然也可以用**”!”**表示取反，即例外的情况。

```
#idea config files
*.iml
*.ipr
*.iws

#Class files
*.class

.gradle/

!build.gradle

#all target folder
*/target/
*/build/
*/out/
```

### 3. git