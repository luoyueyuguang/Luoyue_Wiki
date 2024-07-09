*reference:(https://gist.github.com/ryerh/14b7c24dfd623ef8edc7 )*

## tmux命令
- `tmux [new -s 会话名 -n 窗口名]`启动新会话
- `tmux a(attach) [-t 会话名]`恢复会话
- `tmux ls`列出所有会话
- `tmux kill-session -t 会话名`关闭会话
- `tmux ls | grep : | cut -d. -f1 | awk '{print substr($1, 0, length($1)-1)}' | xargs kill`关闭所有会话
***
## ctrl+b之后
```
:new<回车>  启动新会话
s           列出所有会话
$           重命名当前会话
```

```
c  创建新窗口
w  列出所有窗口
n  后一个窗口
p  前一个窗口
f  查找窗口
,  重命名当前窗口
&  关闭当前窗口
```

```
swap-window -s 3 -t 1  交换 3 号和 1 号窗口
swap-window -t 1       交换当前和 1 号窗口
move-window -t 1       移动当前窗口到 1 号
```

```
%  垂直分割
"  水平分割
o  交换窗格
x  关闭窗格
⍽  左边这个符号代表空格键 - 切换布局
q 显示每个窗格是第几个，当数字出现的时候按数字几就选中第几个窗格
{ 与上一个窗格交换位置
} 与下一个窗格交换位置
z 切换窗格最大化/最小化
```

```
PREFIX : resize-pane -D          当前窗格向下扩大 1 格
PREFIX : resize-pane -U          当前窗格向上扩大 1 格
PREFIX : resize-pane -L          当前窗格向左扩大 1 格
PREFIX : resize-pane -R          当前窗格向右扩大 1 格
PREFIX : resize-pane -D 20       当前窗格向下扩大 20 格
PREFIX : resize-pane -t 2 -L 20  编号为 2 的窗格向左扩大 20 格
```

```
d  退出 tmux（tmux 仍在后台运行）
t  窗口中央显示一个数字时钟
?  列出所有快捷键
:  命令提示符
```

