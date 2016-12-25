> 以下条目中 **prefix** 代表组合键 `Ctrl + a`。官方的**prefix**指的是`Ctrl + b`，在配置中已经被复写。

# 会话session
- `tmux new -s <session>`：创建新的会话
- `ctrl + d` 或 `exit`：退出tmux
- `tmux ls`: 列出会话
- `tmux attach -t <target session>`：切换到某个会话
- `prefix + d`：脱离会话
- `prefix + ctrl-z`：挂起当前会话
- `tmux ls` : list sessions.
- `tmux kill-session -t <session name>` : kill session

# 窗口window
- `prefix + c`：新建窗口
- `prefix + p`：前一个窗口
- `prefix + n`：下一个窗口
- `prefix + [0-9]`：跳转到指定的窗口
- `prefix + w`：查看窗口列表
- `prefix + ,`：更改窗口名称
- `prefix + &`：关闭窗口

# 窗格panel
- `prefix + %`：垂直切分
- `prefix + "`：水平切分
- `prefix + o`：切换窗格
- `prefix + <arrow>`：按箭头方向切换