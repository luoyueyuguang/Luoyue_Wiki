***reference:《跟我一起写Makefile》陈皓***
***
``` shell
target ... : prerequisites ...
	recipe
	...
	...
```
- target
可以是一个 object file(目标文件),也可以是一个可执行文件,还可以是一个标签(label)。+
- prerequisites
生成该 target 所依赖的文件和/或 target。
- recipe
该 target 要执行的命令(任意的 shell 命令)。
*example*
``` shell
edit : main.o kbd.o command.o display.o \
		insert.o search.o files.o utils.o
	cc -o edit main.o kbd.o command.o display.o \
		insert.o search.o files.o utils.o
main.o : main.c defs.h
	cc -c main.c
kbd.o : kbd.c defs.h command.h
	cc -c kbd.c
command.o : command.c defs.h command.h
	cc -c command.c
display.o : display.c defs.h buffer.h
	cc -c display.c
insert.o : insert.c defs.h buffer.h
	cc -c insert.c
search.o : search.c defs.h buffer.h
	cc -c search.c
files.o : files.c defs.h buffer.h command.h
	cc -c files.c
utils.o : utils.c defs.h
	cc -c utils.c
clean :
	rm edit main.o kbd.o command.o display.o \
		insert.o search.o files.o utils.o
```
**prerequisites中如果有一个以上的文件比target文件要新的话,recipe所定义的命令就会被执行。**
***
- make 会一层又一层地去找文件的依赖关系,直到最终编译出第一个目标文件
***
- make中可以使用变量
``` shell
objects = main.o kbd.o command.o display.o \
			insert.o search.o files.o utils.o
edit : $(objects)
	cc -o edit $(objects)
```
***
- make可以自动推导,make 看到一个 .o 文件,它就会自动的把 .c 文件加在依赖关系中。
``` shell
main.o : defs.h
```
- 自动推导的存在使得另一种风格的Makefile可以存在
``` shell
objects = main.o kbd.o command.o display.o \
	insert.o search.o files.o utils.o
edit : $(objects)
	cc -o edit $(objects)
	
$(objects) : defs.h
kbd.o command.o files.o : command.h
display.o insert.o search.o files.o : buffer.h

.PHONY : clean
clean :
	rm edit $(objects)

```
***
*about clean*
``` shell
clean:
	rm edit $(objects)

.PHONY : clean
clean :
	-rm edit $(objects)

```
***
Makefile里有什么
1. 显式规则。显式规则说明了如何生成一个或多个目标文件。这是由 Makefile 的书写者明显指出要生成的文件、文件的依赖文件和生成的命令。
2. 隐式规则。由于我们的 make 有自动推导的功能,所以隐式规则可以让我们比较简略地书写 Makefile,这是由 make 所支持的。
3. 变量的定义。在 Makefile 中我们要定义一系列的变量,变量一般都是字符串,这个有点像你 C 语言中的宏,当 Makefile 被执行时,其中的变量都会被扩展到相应的引用位置上。
4. 指令。其包括了三个部分,一个是在一个 Makefile 中引用另一个 Makefile,就像 C 语言中的 include一样;另一个是指根据某些情况指定 Makefile 中的有效部分,就像 C 语言中的预编译 `#if` 一样;还有就是定义一个多行的命令。有关这一部分的内容,我会在后续的部分中讲述。
5. 注释。 Makefile 中只有行注释,和 UNIX 的 Shell 脚本一样,其注释是用 # 字符,这个就像 C/C++的 `//` 一样。如果你要在你的 Makefile 中使用 # 字符,可以用反斜杠进行转义,如:`\# `。
***
- 默认的情况下,`make`命令会在当前目录下按顺序寻找文件名为 `GNUmakefile`,`makefile` 和`Makefile`的文件
- 最好使用`Makefile` 这个文件名,因为这个文件名在排序上靠近其它比较重要的文件,如`README`最好不要用`GNUmakefile`,因为这个文件名只能由 `GNU make`识别
- 可以使用`make`的`-f`或` --file`参数来指定Makefile文件,使用多条`-f`或`--file`参数,你可以指定多个`makefile`。
***
``` shell
include <filenames>...
```
- `include`类似于c/c++的`#include`
- 可以包含其他Makefile,`<filenames>`可以是当前操作系统 Shell 的文件模式(可以包含路径和通配符)。
- 在 include 前面可以有一些空字符,但是绝不能是 Tab 键开始
-  如果 make 执行时,有 `-I` 或 `--include-dir `参数,那么`make`就会在这个参数所指定的目录下去寻找。
- 接下来按顺序寻找目录 `<prefix>/include (一般是 /usr/local/bin )`、`/usr/gnu/include `、`/usr/local/include` 、`/usr/include`.
- 环境变量 .INCLUDE_DIRS 包含当前 make 会寻找的目录列表。
- `-include/sinclude <filenames>...`无论 include 过程中出现什么错误,都不要报错继续执行,`sinclude`兼容性更好
- 环境变量 MAKEFILES ,那么 make 会把这个变量中的值做一个类似于include 的动作,这个变量中的值是其它的 Makefile,用空格分隔。只是,它和 include 不同的是,从这个环境变量中引入的 Makefile 的“目标”不会起作用,如果环境变量中定义的文件发现错误,make 也会不理。
***
- GNU 的 make 工作时的执行步骤
1. 读入所有的 Makefile。
2. 读入被 include 的其它 Makefile。
3. 初始化文件中的变量。
4. 推导隐式规则,并分析所有规则。
5. 为所有的目标文件创建依赖关系链。
6. 根据依赖关系,决定哪些目标要重新生成。
7. 执行生成命令。
***
- 一般来说,make 会以 UNIX 的标准 Shell,也就是 /bin/sh 来执行命令。
``` shell
targets : prerequisites ; command
	command
	...
```
- `targets`是文件名,以空格分开,可以使用通配符。
- `command`是命令行,如果其不与`“target:prerequisites”`在一行,那么,必须以 Tab 键开头,如果和`prerequisites`在一行,那么可以用分号做为分隔。
- prerequisites 也就是目标所依赖的文件(或依赖目标)
- 如果命令太长,你可以使用反斜杠`(\ )`作为换行符。make 对一行上有多少个字符没有限制。
- `make`中可以使用通配符,但变量中并不会自动展开,展开需要依靠`make`关键字(wildcard, patsubst)等
***
- `VPATH`让`make`自动去寻找文件的依赖关系,如果没有指明这个变量,make 只会在当
前的目录中去找寻依赖文件和目标文件。如果定义了这个变量,那么,make 就会在当前目录找不到的情况下,到所指定的目录中去找寻文件了。
- 另一个设置文件搜索路径的方法是使用`make`的`“vpath”`关键字(注意,它是全小写的),这不是变量,这是一个 make 的关键字,且使用方法比`VPATH`b
- 
***
