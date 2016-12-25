# 在本机生成ssh key
# copy公钥到远程server
可以使用如下命令，将生成的key拷贝到远程server上：

```shell
cat ~/.ssh/id_rsa.pub | ssh user@123.45.56.78 "mkdir -p ~/.ssh && cat >>  ~/.ssh/authorized_keys"
```
