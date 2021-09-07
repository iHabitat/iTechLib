## 1. 使用`ctrl+R`搜索历史

## 2. 快速重复执行上一条命令

- 直接上下方向键切换，并回车
- 输入`!!`，然后回车

## 3. 使用`!+id`的方式执行指定的命令（id为history中命令的标号）

## 4. 使用`!+key`的方式执行指定关键字的命令

## 5. 使用*HISTSIZE*控制历史命令记录的总行数

```shell
#vim ~/.bash_profile
HISTSIZE=500
HISTFILESIZE=500
```

## 6. 使用*HISTCONTROL*删除连续重复的命令

```shell
export HISTCONTROL=ignoredups
```

## 7. 使用`-c`清除所有的命令历史

```shell
history -c
```

## 8. 命令替换

```shell
ls testfile.txt
#等价于 vim testfile.txt
vim !$
```

同理，`!^`获取前条命令的第一项参数

## 9. 替换指定命令的指定参数

```shell
cp ~/docker/testfile.txt /github/domeos/src/testfile.txt
ls -l !cp:2
#等价于： ls -l /github/domeos/src/testfile.txt
```

## 10. 指定显示最近命令的条数

```shell
history [n]
```

*n*是一个数字