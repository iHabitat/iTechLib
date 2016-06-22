在mac上无法直接使用`homebrew`安装`sshpass`，但是可以变相的使用`brew install`来实现。实现方法如下：

```shell
$ brew install https://raw.githubusercontent.com/kadwanev/bigboybrew/master/Library/Formula/sshpass.rb
```

这样，`sshpass`就安装在`/usr/local/Cellar/`目录下了，然后只需要**link**对应的shell command即可。

```shell
$ ln -s /usr/local/Cellar/sshpass/1.05/bin/sshpass /usr/local/bin/sshpass
```

可以用以下命令验证是否安装成功。

```shell
$ sshpass -V
sshpass 1.05 (C) 2006-2011 Lingnu Open Source Consulting Ltd.
This program is free software, and can be distributed under the terms of the GPL
See the COPYING file for more information.
```