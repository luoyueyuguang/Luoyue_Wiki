# 什么是Linux
## Linux Kernel
**Linux内核**（英语：Linux kernel）是一种开源的[类Unix](https://zh.wikipedia.org/wiki/%E7%B1%BBUnix%E7%B3%BB%E7%BB%9F "类Unix系统")[操作系统](https://zh.wikipedia.org/wiki/%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F "操作系统")[宏内核](https://zh.wikipedia.org/wiki/%E5%AE%8F%E5%86%85%E6%A0%B8)。由Linus Torvalds开发。
*from:WikiPedia*
[Red Hat 对Linux的介绍](https://www.redhat.com/zh/topics/linux/what-is-linux)
![[Pasted image 20240426205937.png]]
## GNU/Linux
 GNU/Linux 系统中，Linux 就是内核组件。而该系统的其余部分主要是由 GNU 工程编写和提供的程序组成。因为单独的 Linux 内核并不能成为一个可以正常工作的操作系统
## 关于Linux的其他
Android也是Linux，但Android是Linux内核加Android套件。人们口中的Linux普遍指的还是和GNU套件结合的Linux

## 一切皆文件
“一切皆是文件”是 Unix/Linux 的基本哲学之一，它是指 Linux 系统中的所有的一切都可以通过文件的方式访问、管理，即使不是文件，也以文件的形式来管理。例如硬件设备、进程、套接字等都抽象成文件，使用统一的用户接口，虽然文件类型各不相同，但是对其提供的却是同一套操作。

## Linux发行版
Linux 发行版是一个由 Linux 内核、[GNU 工具](https://www.gnu.org/manual/blurbs.html)、附加软件和软件包管理器组成的操作系统，它也可能包括[显示服务器](https://linux.cn/article-12589-1.html)和[桌面环境](https://itsfoss.com/what-is-desktop-environment/)，以用作常规的桌面操作系统。
![[Pasted image 20240426142117.png]]

## 镜像站

国内比较著名的有清华，科大等
https://mirrors.ustc.edu.cn/
https://mirrors.tuna.tsinghua.edu.cn/


# Linux常用命令
## `man` `-h` `--help` `tldr` and `GPT like`
`man`
`-h / -help`
`tldr`

## ***常用Tab***
## 文件与文件夹相关
### touch
`touch`生成文件
`mkdir`开文件夹
### cd
`cd /usr/bin`绝对路径的用法
`cd ..`回到上级目录
`cd ~`回到家目录
`cd -`回到之前的目录
`cd`　回到家目录

`pwd`给出当前路径
### ls
`ls`查看当前目录
`ls -a`查看隐藏文件
### cp
`cp src dst`复制文件
`cp -R src/ dst`递归用来复制目录
### mv
`mv src dst`移动文件
### rm
`rm something`删除文件
`rm -f`强制删除
`rm -i`删除前询问一下
### file
`file something`查看文件类型

### cat
`cat` 查看文件

### type
`type`查看命令类型
### *ln*链接
`ln src dst`硬链
`ln -s src dst`软链(symbolic link)
### rsync
## glob文件匹配模式
| 模式                | 说明                                              |
| ----------------- | ----------------------------------------------- |
| `*`               | 匹配除了斜杠(/)之外的所有字符。 Windows上是斜杠(/)和反斜杠(\)         |
| `**`              | 匹配零个或多个目录及子目录。不包含 `.` 以及 `..` 开头的。              |
| `?`               | 匹配任意单个字符。                                       |
| `[seq]`           | 匹配 seq 中的其中一个字符。                                |
| `[!seq]`          | 匹配不在 seq 中的任意一个字符。                              |
| `\`               | 转义符。                                            |
| `!`               | 排除符。                                            |
| `?(pattern_list)` | 匹配零个或一个在 `pattern_list` 中的字符串。                  |
| `*(pattern_list)` | 匹配零个或多个在 `pattern_list` 中的字符串。                  |
| `+(pattern_list)` | 匹配一个或多个在 `pattern_list` 中的字符串。                  |
| `@(pattern_list)` | 匹配至少一个在 `pattern_list` 中的字符串。                   |
| `!(pattern_list)` | 匹配不在 `pattern_list` 中的字符串.                      |
| `[...]`           | POSIX style character classes inside sequences. |

## 其他命令
`ps`查看进程
`top`监控
`htop`
`btop`
`kill`

`mount`挂盘

`grep`搜索
`grep something `

`history`查看历史

## 多个命令混合
`&&`
`;`
`|`*管道符*
`echo $BASH_SUBSHELL`来查看`(`的进程列表
## alias
`~/.*shrc`
## 环境变量
`$PATH`
`$LD_LIBRARY_PATH`
`$CPATH`
`env`

## 运行shell脚本
- *shebang*
- 不同方式运行脚本的区别
	`.　`与`source`
	`./与bash`
# Linux上的编译
`gcc/g++ src -o bin`
`clang`也是一样

## Makefile
`make`命令

## CMake xmake autoconf
都是用来生成Makefile的

# VIM

# Git

 Reference
- https://archlinuxstudio.github.io/ShellTutorial/#/commandLine/linux_env?id=%e7%99%bb%e5%bd%95-shell
- https://101.lug.ustc.edu.cn/



