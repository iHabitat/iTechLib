# Linux服务器监控工具汇总

## `uptime` - 查看系统负载

1. 当前时间
2. 系统已运行时间
3. 当前在线用户
4. 系统平均负载

系统平均负载被定义为在特定时间间隔内运行队列中的平均进程数。一般来说，每个CPU内核当前活动进程数不大于3，则系统运行表现良好！当然这里说的是每个cpu内核，也就是如果你的主机是四核cpu的话，那么只要uptime最后输出的一串字符数值小于12即表示系统负载不是很严重.
当然如果达到20，那就表示当前系统负载非常严重，估计打开执行web脚本非常缓慢.

## `top` - 系统各进程资源情况

### 统计信息区

前五行是系统整体的统计信息。

- 第一行是**任务队列**信息，执行结果同 `uptime` 命令相同。结果有：*当前时间、系统运行时间、当前登录用户数、系统负载(任务队列的平均长度)*
- 第二、三行是进程和CPU的信息。
- 最后两行为内存信息。

### 进程信息区

## `htop` - 交互式进程查看器
