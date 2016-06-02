# upgrade git 升级git时遇到的一些问题

## Problem

今天看到Git2.0发布了，于是想更新一下。查阅相关资料得知，先需要uninstall以前的版本。首先我需要确定当前的git版本以及安装目录：

```shell
localhost:~ ThomsonTang$ git version
git version 1.8.5.2 (Apple Git-48)
```
 
查看其所在目录：

```shell
localhost:~ ThomsonTang$ which git
/usr/bin/git
```

此路径并不在`/usr/local`下，于是想查看下`/usr/local`下是否有别的git：

```shell
localhost:bin ThomsonTang$ ls /usr/local/bin/ | grep git
git
git-credential-osxkeychain
git-cvsserver
git-receive-pack
git-shell
git-upload-archive
git-upload-pack
github

localhost:bin ThomsonTang$ /usr/local/bin/git --version
git version 1.8.4
```

果然还有另外一个版本的git。想了想，才想起起初安装git是通过Xcdoe安装的，后来又安装了github App，这样就产生了两个版本的git：

1. Xcode将git安装在了`/usr/bin/`下
2. Github for Mac将git安装在了`/usr/local/bin/`下

这样就有了两个版本的git，之后检查一下PATH环境变量：

```shell
/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/usr/local/git/bin:/usr/local/go/bin
```

系统首先找到的git版本便是`/usr/bin`下的。现在要升级git为2.0，因为`/usr/bin/`下的git是Xcode安装的，我担心卸载掉会引起别的错误，所以还是决定将`/usr/local/bin`下的卸载，之后重新安装2.0版本的git。

在stackoverflower上查看了相关帖子，都建议卸载Github for Mac，然后使用HomeBrew安装最新的git。考虑到自己平时也很少使用Github for Mac，更多的是在命令行操作，所以就决定采用这种方法。另外一个原因是，在mac上使用HomeBrew安装应用是一个不错的选择，它默认便会将应用在`/usr/local`下，并且统一归类到一个名叫`Cellar`的目录中。

## Solution

### 卸载Github for Mac.app

推荐使用[AppCleaner](http://www.freemacsoft.net/appcleaner/)卸载相关APP。这里我只需卸载Github.app即可。

### 使用HomeBrew安装git

直接在终端执行：

```shell
localhost:~ ThomsonTang$ brew install git
==> Downloading https://downloads.sf.net/project/machomebrew/Bottles/git-2.1.0.mavericks.bottle.tar.gz
Already downloaded: /Library/Caches/Homebrew/git-2.1.0.mavericks.bottle.tar.gz
==> Pouring git-2.1.0.mavericks.bottle.tar.gz
==> Caveats
The OS X keychain credential helper has been installed to:
  /usr/local/bin/git-credential-osxkeychain

The 'contrib' directory has been installed to:
  /usr/local/share/git-core/contrib

Bash completion has been installed to:
  /usr/local/etc/bash_completion.d

zsh completion has been installed to:
  /usr/local/share/zsh/site-functions
==> Summary
🍺  /usr/local/Cellar/git/2.1.0: 1339 files, 31M
```

OK，安装成功，可以看到在`/usr/local/bin`下已经有了与git相关的命令：

![brew-install-git](/Users/ThomsonTang/Pictures/images/dev/brew-install-git.png)

然后执行`git —version`，发现依旧显示的是以前的git版本：

```shell
localhost:~ ThomsonTang$ git --version
git version 1.8.5.2 (Apple Git-48)
```

可以想到这是由于PATH变量找到的是旧的版本，需要修改PATH变量。

### 更改PATH变量

在终端中执行：

```shell
localhost:bin ThomsonTang$ cat /etc/paths
/usr/bin
/bin
/usr/sbin
/sbin
/usr/local/bin
```

看的到`/usr/local/bin`在`/usr/bin/`之后，需要将`/usr/local/bin`放在`/usr/bin/`之前，这样PATH才会先找到`/usr/local/bin`下的git。修改`/etc/paths`后，得到正确的结果，升级成功。

```shell
localhost:~ ThomsonTang$ git --version
git version 2.1.0
```