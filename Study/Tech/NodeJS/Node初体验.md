## Node初体验

### 1. 安装Node
可以通过编译构建源码安装，也可以直接安装对应各平台的二进制包安装。具体可参考官方文档。

download node-v0.10.26.pkg and installed

Node was installed at `/usr/local/bin/node`
npm was installed at `/usr/local/bin/npm`
Make sure that `/usr/local/bin` is in your `$PATH`.  
安装后可以在命令行中执行以下命令验证：

	$ node -v

有两种方式可以执行node：

1. CLI (command-line-interface)，直接键入 `node` 命令便可进入一个JavaScript命令提示行，然后就可以开始畅写node并执行了。  
2. File：通过node命令直接运行一个js文件  
	`$ node file.js`   
或者给该js文件分配执行权限：  
	`$ chmod o+x file.js`  
然后在文件开头插入一行：  
	`#!/usr/bin/env node`  
最后直接执行该文件即可：   
	`$ ./file.js`  

### 2. NPM - Node Package Manager
NPM已成为node管理包的标准，可用以下命令安装：  

	$ curl http://npmjs.org/install.sh sh    
NPM命令：

### 3. 为什么
