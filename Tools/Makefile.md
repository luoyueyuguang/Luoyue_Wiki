***reference:《跟我一起写Makefile》陈皓***
***
`make -n`可以dry-run,模拟运行命令
`make -d`可以用来debug
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
- 另一个设置文件搜索路径的方法是使用`make`的`“vpath”`关键字(注意,它是全小写的),这不是变量,这是一个 make 的关键字,且使用方法比`VPATH`变量灵活。
``` shell
vpath <pattern> <directories>
	为符合模式 <pattern> 的文件指定搜索目录 <directories>。
vpath <pattern>
	清除符合模式 <pattern> 的文件的搜索目录。
vpath
	清除所有已被设置好了的文件搜索目录。
```
- `vpath`使用方法中的`<pattern>`需要包含`%`字符.`%`的意思是匹配零或若干字符,例如`vpath %.h ../headers`
***
``` shell
clean:
	rm *.o temp
***
.PHONY : clean
clean :
	rm *.o temp
```
- `“clean”`这个文件并不生成, “伪目标”并不是一个文件,只是一个标签,由于“伪目标”
不是文件,所以`make`无法生成它的依赖关系和决定它是否要执行。我们只有通过显式地指明这个“目标”才能让其生效,“伪目标”的取名不能和文件名重名,不然其就失去了“伪目标”的意义了.
- 可以使用`“.PHONY”`来显式地指明一个目标是“伪目标” ,向 make 说明,不管是否有这个文件,这个目标就是“伪目标”
- 伪目标一般没有依赖的文件。但是,我们也可以为伪目标指定所依赖的文件。伪目标同样可以作为“默认目标”,只要将其放在第一个。
``` shell
all : prog1 prog2 prog3
.PHONY : all
```
- `make`也可以通过隐式规则推导出伪目标
- 目标也可以成为依赖。所以,伪目标同样也可成为依赖。
``` shell
.PHONY : cleanall cleanobj cleandiff
cleanall : cleanobj cleandiff
	rm program
```
***
``` shell
<targets ...> : <target-pattern> : <prereq-patterns ...>
	<commands>
	...
```
- `targets`定义了一系列的目标文件,可以有通配符。是目标的一个集合。
- `target-pattern`是指明了`targets`的模式,也就是的目标集模式。
- `prereq-patterns`是目标的依赖模式,它对`target-pattern`形成的模式再进行一次依赖目标的定义。
``` shell
objects = foo.o bar.o
all: $(objects)
$(objects): %.o: %.c
	$(CC) -c $(CFLAGS) $< -o $@
# it is equivalant to 
foo.o : foo.c
	$(CC) -c $(CFLAGS) foo.c -o foo.o
bar.o : bar.c
	$(CC) -c $(CFLAGS) bar.c -o bar.o
```
***
- C/C++编译器有`-M`或`-MM`选项来生成依赖关系
- 如果使用 GNU 的 C/C++ 编译器,得用`-MM`参数,不然,`-M`参数会把一些标准库的头文件也包含进来
``` shell
#GNU 组织建议把编译器为每一个源文件的自动生成的依赖关系放到一个文件中,为每一个 name.c 的文件都生成一个 name.d 的 Makefile 文件,.d 文件中就存放对应 .c 文件的依赖关系。
#感谢左耳朵耗子的模板
%.d: %.c
	@set -e; rm -f $@; \
	$(CC) -M $(CPPFLAGS) $< > $@.$$$$; \
	sed 's,\($*\)\.o[ :]*,\1.o $@ : ,g' < $@.$$$$ > $@; \
	rm -f $@.$$$$
```
***
- 通常, make 会把其要执行的命令行在命令执行前输出到屏幕上。用`@`字符在命令行前,那么,这个命令将不被 make 显示出来,ps`@echo 正在编译XXX模块......`
- 如果 make 执行时,带入`make`参数`-n`或`--just-print`,那么其只是显示命令,但不会执行命令
- make 参数`-s`或`--silent`或`--quiet`则是全面禁止命令的显示
***
- 当依赖目标新于目标时,也就是当规则的目标需要被更新时,make 会一条一条的执行其后的命令。
- 如果要让上一条命令的结果应用在下一条命令时,应该使用分号分隔这两条命令。
``` shell
exec:
	cd /home/hchen
	pwd
# or
exec:
	cd /home/hchen; pwd #use ; to spilit
```
- make 一般是使用环境变量 SHELL 中所定义的系统 Shell 来执行命令,默认情况下使用 UNIX 的标准 Shell——/bin/sh 来执行命令。
*MS-DOS 下没有 SHELL 环境变量,当然你也可以指定。如果你指定了 UNIX 风格的目录形式,首先,make 会在 SHELL 所指定的路径中找寻命令解释器,如果找不到,其会在当前盘符中的当前目录中寻找,如果再找不到,其会在 PATH 环境变量中所定义的所有路径中寻找。MS-DOS 中,如果你定义的命令解释器没有找到,其会给你的命令解释器加上诸如 .exe 、.com 、.bat 、.sh 等后缀.*
***
*每当命令运行完后,make 会检测每个命令的返回码,如果命令返回成功,那么 make 会执行下一条命令,当规则中所有的命令成功返回后,这个规则就算是成功完成了。如果一个规则中的某个命令出错了(命令退出码非零) ,那么 make 就会终止执行当前规则,这将有可能终止所有规则的执行*
- 忽略命令的出错,我们可以在 Makefile 的命令行前加一个减号`-`
``` shell
clean:
	-rm -f *.o
```
- 给 make 加上 `-i`或是`--ignore-errors`参数,那么,Makefile 中所有命令都会忽略错误。
- 而如果一个规则是以`.IGNORE`作为目标的,那么这个规则中的所有命令将会忽略错误。
-  `make`的`-k`或是`--keep-going`参数表示:如果某规则中的命令出错了,那么就终止该规则的执行,但继续执行其它规则
***
- 可以在不同目录里写Makefile,来进行make的控制
``` shell
subsystem:
	cd subdir && $(MAKE)
#it is equivalent to
subsystem:
	$(MAKE) -C subdir
```
- 可以通过`export/unexport`来对每个Makefile设置不同的环境变量
- 有两个变量,一个是`SHELL`,一个是`MAKEFLAGS`,不管是否`export`,其总是要传递到下层 Makefile中
``` shell
# 如果不想往下层传递参数
subsystem:
	cd subdir && $(MAKE) MAKEFLAGS=
```
***
- 如果 Makefile 中出现一些相同命令序列,那么我们可以为这些相同的命令序列定义一个变量。定义这种命令序列的语法以`define`开始,以`endef`结束.
``` shell
define run-yacc
yacc $(firstword $^)
mv y.tab.c $@
endef
```
使用
``` shell
foo.c : foo.y
	$(run-yacc)
```
***
- 变量的命名字可以包含字符、数字,下划线(可以是数字开头),但不应该含有 : 、# 、= 或是空字符(空格、回车等)。变量是大小写敏感的,“foo”、“Foo”和“FOO”是三个不同的变量名。
- 变量会在使用它的地方精确地展开,就像 C/C++ 中的宏一样,
- 变量在声明时需要给予初值,而在使用时,需要给在变量名前加上 $ 符号,但最好用小括号 `()`或是大括号`{}`把变量给包括起来。
***
*变量的定义方法*
- `=`  左侧是变量,右侧是变量的值,,右侧变量的值可以定义在文件的任何一处
``` shell
foo = $(bar)
bar = $(ugh)
ugh = Huh?
all:
	echo $(foo)
```
- `:=`前面的变量不能使用后面的变量,只能使用前面已定义好了的变量
``` shell
x := foo
y := $(x) bar
x := later

#it is equivalent to 

y := foo bar
x := later
```
- `?=`如果没有定义过就定义
``` shell
FOO ?= bar
# 其含义是,如果 FOO 没有被定义过,那么变量 FOO 的值就是“bar”
```
*some little tips*
- “#”注释符可以来表示变量定义的终止
``` shell
dir := /foo/bar    #dir是/foo/bar加四个空格
```
- MAKELEVEL变量会表示make的调用层数,总控的Makefile是0,子目录下的Makefile为1.
***
- 可以替换变量中的共有的部分,其格式是`$(var:a=b)`或是`${var:a=b}`,其意思是`“var”`中所有以“a”字串“结尾”的“a”替换成“b”字串。这里的“结尾”意思是“空格”或是“结束符”.
- 另外一种变量替换的技术是以“静态模式”来定义
``` shell
foo := a.o b.o c.o
bar := $(foo:%.o=%.c)
```
- 可以把变量的值再当成变量
``` shell
x = y
y = z
a := $($(x))
```
- 可以使用 += 操作符给变量追加值,如果变量之前没有定义过,那么,+= 会自动变成 = ,如果前面有变量定义,那么 += 会继承于前次操作的赋值符。如果前一次的是 := ,那么 += 会以 := 作为其赋值符 
``` shell
objects = main.o foo.o bar.o utils.o
objects += another.o
```
***
- 如果变量是`make`的命令行参数设置的,那么`Makefile`中对这个变量的赋值会被忽略。如果想在 Makefile 中设置这类参数的值,那么,可以使用`“override”`指令.
``` shell
override <variable>; = <value>;
override <variable>; := <value>;
override <variable>; += <more text>;

override define foo
bar
endef
```
***
- 使用 define 关键字设置变量的值可以有换行,这有利于定义一系列的命令
- define 指令后面跟的是变量的名字,而重起一行定义变量的值,定义是以 endef 关键字结束.其工作方式和“=”操作符一样。变量的值可以包含函数、命令、文字,或是其它变量.
``` shell
define two-lines
echo foo
echo $(bar)
endef
```
***
- make 运行时的系统环境变量可以在 make 开始运行时被载入到 Makefile 文件中,但是如果 Makefile中已定义了这个变量,或是这个变量由 make 命令行带入,那么系统的环境变量的值将被覆盖。(如果make 指定了“-e”参数,那么,系统环境变量将覆盖 Makefile 中定义的变量)
- 当 make 嵌套调用时,上层 Makefile 中定义的变量会以系统环境变量的方式传递到下层的 Makefile 中。当然,默认情况下,只有通过命令行设置的变量会被传递。而定义在文件中的变量,如果要向下层 Makefile 传递,则需要使用 export 关键字来声明。
***
- 前面我们所讲的在 Makefile 中定义的变量都是“全局变量”,在整个文件,我们都可以访问这些变量。当然,“自动化变量”除外,如`$<`等这种类量的自动化变量就属于“规则型变量”,
- 可以为某个目标设置局部变量,这种变量被称为`“Target-specific Variable”`,它可以和“全局变量”同名,因为它的作用范围只在这条规则以及连带规则中,所以其值也只在作用范围内有效。而不会影响规则链以外的全局变量的值。
- 在 GNU 的 make 中,还支持模式变量(Pattern-specific Variable),通过上面的目标变量中,我们知道,变量可以定义在某个目标上。
``` shell
<target ...> : <variable-assignment>;
<target ...> : override <variable-assignment>
```
example:
``` shell
prog : CFLAGS = -g
prog : prog.o foo.o bar.o
	$(CC) $(CFLAGS) prog.o foo.o bar.o
	
prog.o : prog.c
	$(CC) $(CFLAGS) prog.c
	
foo.o : foo.c
	$(CC) $(CFLAGS) foo.c
bar.o : bar.c
	$(CC) $(CFLAGS) bar.c

#GNU make的模式变量
%.o : CFLAGS = -O
```
***
- make中有条件表达式
- syntax如下
``` shell
<conditional-directive>
<text-if-true>
endif
#or
<conditional-directive>
<text-if-true>
else
<text-if-false>
endif
```
- `<conditional-directive>`表示条件关键字,这个关键字有四个
1. `ifeq`比较`arg1`和`arg2`的值是否相同
2. `ifneq`比较参数`arg1`和`arg2`的值是否不同
``` shell
ifeq/ifneq (<arg1>, <arg2>)
ifeq/ifneq '<arg1>' '<arg2>'
ifeq/ifneq "<arg1>" "<arg2>"
ifeq/ifneq "<arg1>" '<arg2>'
ifeq/ifneq '<arg1>' "<arg2>"
```
- `ifdef`,如果变量`<variable-name>`的值非空,那么表达式为真,`ifdef`只是测试一个变量是否有值,其并不会把变量扩展到当前位置.
- `ifndef`,与`ifdef`相反
``` shell
ifndef <variable-name>
 
#example
foo =
ifdef foo
	frobozz = yes
else
	frobozz = no
endif
```
- 在`<conditional-directive>`这一行上,多余的空格是被允许的,但是不能以 Tab 键作为开始,注释符 # 同样也是安全的,`else`和`endif`同样.
- make 是在读取 Makefile 时就计算条件表达式的值,并根据条件表达式的值来选择语句,所以,最好不要把自动化变量(如`$@`等)放入条件表达式中,因为自动化变量是在运行时的。
- 为了避免混乱,make 不允许把整个条件语句分成两部分放在不同的文件中
***
- 函数调用,很像变量的使用,也是以`$`来标识的
``` shell
$(<function> <arguments>)
${<function> <arguments>}
```
- `<function>`就是函数名,make 支持的函数不多.`<arguments>`为函数的参数,参数间以逗号分割
``` shell
comma:= ,
empty:=
space:= $(empty) $(empty)
foo:= a b c
bar:= $(subst $(space),$(comma),$(foo))
```
***
- `$(subst <from>,<to>,<text>)`:把字串`<text>`中的`<from>`字符串替换成`<to>`