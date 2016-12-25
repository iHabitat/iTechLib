# dotfiles

> 什么是**dotfiles**呢？根据其字表含义，就是**点(dot)文件**，即**带点的文件**。

熟悉Linux、Unix的都知道，在Linux系统下，以`.(dot)`为prefix的文件默认是被隐藏的。比如用户的**home**目录下就有一堆的以 `.` 开头的文件，这些文件都是隐藏的。当然这不是最重要的，关键在于这些隐藏的文件通常都是用做**配置文件**的，主要用来存储一些**个性化的设置**或**自定的扩展**功能。例如，Mac OS X 上，用户的`home`目录下的`.bash_profile`文件，就是用来设置用户的shell环境变量、命令别名等一些用户习惯性的操作方式。作为一个RD，拥有一套符合自己习惯的个性化的配置是很有价值的，可以最大程度的提高自己的工作效率。**dotfiles**就是这类文件的一个统称。

# 为什么要创建自己的dotfiles
既然dotfiles是一些个性化配置文件，那么这些文件一般是很少改动的，基本上都是用户自己长期积累的。因此，当用户更换了工作设备比如电脑，就没有必要重新再创建自己的dotfiles了，直接使用以前的即可。因此创建并保存好自己的dotfiles就很重要了。借助**Github**或者其他的云端存储工具可以很好的保存自己的dotfiles，这样就再也不用担心更换电脑后还得重新配置。

# 如何创建自己的dotfiles
除了前面提到的`.bash_profile`，常见的dotfile还有`.vimrc`、`.bashrc`、`.tmux.config`等，这些文件都与其代表的软件工具息息相关，而这些工具都是我们工作和编码中常用的利器。  
人们常说“站在巨人的肩膀上”，因此没有必要从零开始构建自己的dotfiles，我们可以参考牛人已经创建好的，然后增加或修改自己的配置就可使用了。从哪里找牛人呢？当然是**Github**了。我们可以利用 [dotfiles.github.io](http://dotfiles.github.io/) 上推荐的一些dotfiles project作为基础，在使用的同事慢慢完善自己的dotfiles。我自己就是使用 [skwp/dotfiles](https://github.com/skwp/dotfiles) 来创建自己的dotfiles。