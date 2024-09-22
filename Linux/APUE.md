*reference:APUE*
*Unix 架构*
![[Pasted image 20240909195347.png]]
***
*看看login in*
```
cat /etc/passwd
git:x:965:965:git daemon user:/:/usr/bin/git-shell
登录名字:加密密码(在其他文件):用户id:组id:注释域:家目录:shell
```
***
`sysconf,pathconf,fpathconf`查看配置
`sysconf`用来查看系统特定配置
`pathconf`查看与路径有关的
`fpathconf`获取与特定文件描述符相关的

一些系统限制清单

| Limit | FreeBSD 8.0 | Linux 3.2.0 | Mac OS X 10.6.8 | Solaris 10 (UFS file system) | Solaris 10 (PCFS file system) |
| --- | --- | --- | --- | --- | --- |
| ARG_MAX | 262,144 | 2,097,152 | 262,144 | 2,096,640 | 2,096,640 |
| ATEXIT_MAX | 32 | 2,147,483,647 | 2,147,483,647 | no limit | no limit |
| CHARCLASS_NAME_MAX | no symbol | 2,048 | 14 | 14 | 14 |
| CHILD_MAX | 1,760 | 47,211 | 266 | 8,021 | 8,021 |
| clock ticks/second | 128 | 100 | 100 | 100 | 100 |
| COLL_WEIGHTS_MAX | 0 | 255 | 2 | 10 | 10 |
| FILESIZEBITS | 64 | 64 | 64 | 41 | unsupported |
| HOST_NAME_MAX | 255 | 64 | 255 | 255 | 255 |
| IOV_MAX | 1,024 | 1,024 | 1024 | 16 | 16 |
| LINE_MAX | 2,048 | 2,048 | 2,048 | 2,048 | 2,048 |
| LINK_MAX | 32,767 | 65,000 | 32,767 | 32,767 | 1 |
| LOGIN_NAME_MAX | 17 | 256 | 255 | 9 | 9 |
| MAX_CANON | 255 | 255 | 1,024 | 256 | 256 |
| MAX_INPUT | 255 | 255 | 1,024 | 512 | 512 |
| NAME_MAX | 255 | 255 | 255 | 255 | 8 |
| NGROUPS_MAX | 1,023 | 65,536 | 16 | 16 | 16 |
| OPEN_MAX | 3,520 | 1,024 | 256 | 256 | 256 |
| PAGESIZE | 4,096 | 4,096 | 4,096 | 8,192 | 8,192 |
| PAGE_SIZE | 4,096 | 4,096 | 4,096 | 8,192 | 8,192 |
| PATH_MAX | 1,024 | 4,096 | 1,024 | 1,024 | 1,024 |
| PIPE_BUF | 512 | 4,096 | 512 | 5,120 | 5,120 |
| RE_DUP_MAX | 255 | 32,767 | 255 | 255 | 255 |
| STREAM_MAX | 3,520 | 16 | 20 | 256 | 256 |
| SYMLINK_MAX | 1,024 | no limit | 255 | 1,024 | 1,024 |
| SYMLOOP_MAX | 32 | no limit | 32 | 20 | 20 |
| TTY_NAME_MAX | 255 | 32 | 255 | 128 | 128 |
| TZNAME_MAX | 255 | 6 | 255 | no limit | no limit |
***
## File I/O
- 所有打开的文件都被描述为一个非零整数的文件描述符
- Unix系统下,0作为标准输入,1
作为标准输出,2为标准错误
- 一般被定义为`STDIN_FILENO, STDOUT_FILENO, and STDERR_FILENO`常数

**open and openat**
``` c
#include <fcntl.h>

int open(const char *pathname, int flags, ...
                  /* mode_t mode */ );

int openat(int dirfd, const char *pathname, int flags, ...
                  /* mode_t mode */ );
       /* Documented separately, in openat2(2): */
int openat2(int dirfd, const char *pathname,
                  const struct open_how *how, size_t size);

Feature Test Macro Requirements for glibc (see feature_test_macros(7)):

openat():
    Since glibc 2.10:
        _POSIX_C_SOURCE >= 200809L
    Before glibc 2.10:
        _ATFILE_SOURCE

```
*flag*

| flag              | meaning                                                                |
| ----------------- | ---------------------------------------------------------------------- |
| O_RDONLY          | 打开只读                                                                   |
| O_WRONLY          | 打开只写                                                                   |
| O_RDWR            | 打开后读和写                                                                 |
| O_EXEC            | 打开仅执行                                                                  |
| O_SEARCH          | 打开仅搜索(应用于文件)                                                           |
| O_APPEND          | 追加写                                                                    |
| O_CLOEXEC         | 设置文件描述符为 FD_CLOEXEC                                                    |
| O_CREAT           | 创造文件如果不存在                                                              |
| O_DIRECTORY       | 创造文件夹如果文件夹不存在                                                          |
| O_EXCL            | 如果O_CREAT存在且文件存在，产生一个错误                                                |
| O_NOCTTY          | 如果path为终端设备，不将它作为这个进程的控制终端                                             |
| O_NOFOLLOW        | 如果path是符号链接，报错                                                         |
| O_NONBLOCK        | 如果 "path" 指向一个FIFO（先进先出队列）、块特殊文件或字符特殊文件，此选项<br>将为文件的打开以及后续I/O设置非阻塞性模式。 |
| O_SYNC            | 让每个写操作等待物理I/O完成，包括那些由于写操作而导致文件属性更新所必需的<br>I/O操作                        |
| O_TRUNC           | 如果文件存在，并且成功以只写或读写模式打开，将其长度截断至0                                         |
| O_TTY_INIT        | 当打开一个尚未开启的终端设备时，设置非标准termios参数为与Single UNIX Specification兼容的行为所对应的值。   |
| O_DSYNC(optional) | 让每个操作等待物理I/O完成，但如果不影响刚刚写入的数据的可读性，则<br>不要等待文件属性更新。                      |
| O_RSYNC(optional) | 让每个针对文件描述符的读操作等待，直到同一部分文件中任何待处理的写<br>操作都完成                             |

**create**
``` c
int creat(const char *pathname, mode_t mode);
```
相当于
``` c
open(path, O_WRONLY | O_CREAT | O_TRUNC, mode);
```
create仅写，若想写入稍后读取，可以使用
``` c
open(path, O_RDWR | O_CREAT | O_TRUNC, mode);
```

**close**
``` c
int close(int fd);
```
关闭一个文件，当进程退出时，所有文件自动关闭

**lseek**
``` c
off_t lseek(int fd, off_t offset, int whence);
```
每个文件都有oifset,默认为,除非`O_APPEND`被设定
- `whence`为`SEEK_SET`，文件的位置将被设置为开始后的`offset`字节
- `whence`为`SEEK_CUR`，文件的位置将被设置为当前位置加上`offset`字节
- `whence`为`SEEK_END`，文件的位置将被设置为文件大小加上`offset`字节
成功的`lseek`调用会返回文件偏移量，可用以下代码来找到当前偏移量
``` c
off_t currpos;
currpos = lseek(fd, 0, SEEK_CUR);
```
如果文件不可`seeking`,为pipe, FIFO, or socket，`lseek`会返回`-1`
