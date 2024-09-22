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
- `$(patsubst <pattern>,<replacement>,<text>)`查找`<text>`中的单词(单词以`“空格”`,`“Tab”`或`“回车”``“换行”`分隔)是否符合模式`<pattern>`,如果匹配的话,则以`<replacement>`替换。这里,`<pattern>` 可以包括通配符`%`,表示任意长度的字串。如果`<replacement>`中也包含`%`,那么,`<replacement>`中的这个`%`将是`<pattern>`中的那个 `%`所代表的字串。
- `$(strip <string>)`去掉`<string>`字串中开头和结尾的空字符。
- `$(findstring <find>,<in>)`在字串`<in>`中查找 `<find>`字串,如果找到,那么返回 `<find>`,否则返回空字符串。
- `$(filter <pattern...>,<text>)`,以`<pattern>`模式过滤`<text>`字符串中的单词,保留符合模式`<pattern>`的单词。可以有多个模式。
- `$(filter-out <pattern...>,<text>)`, 以`<pattern>`模式过滤`<text>`字符串中的单词,去除符合模式`<pattern>`的单词。可以有多个模式
- `$(sort <list>)`给字符串`<list>`中的单词排序(升序)
- `$(word <n>,<text>)`取字符串`<text>`中第`<n>`个单词。(从一开始)
- `$(wordlist <ss>,<e>,<text>)`,从字符串`<text>`中取从`<ss>`开始到`<e>`的单词串。`<ss>`和`<e>`都是数字
- `$(words <text>)`统计`<text>`中字符串中的单词个数。
- `$(firstword <text>)`取字符串`<text>`中的第一个单词。
***
- `$(dir <names...>)`从文件名序列`<names>`中取出目录部分。目录部分是指最后一个反斜杠(/ )之前的部分。如果没有反斜杠,那么返回` ./`
- `$(notdir <names...>)`,:从文件名序列`<names>`中取出非目录部分。非目录部分是指最後一个反斜杠(/ )之后的部分。
- `$(suffix <names...>)`从文件名序列`<names>`中取出各个文件名的后缀。
- `$(basename <names...>)`从文件名序列`<names>`中取出各个文件名的前缀部分。
- `$(addsuffix <suffix>,<names...>)`把后缀`<suffix>`加到`<names>`中的每个单词后面。
- `$(addprefix <prefix>,<names...>)` 把前缀`<prefix>`加到`<names>`中的每个单词前面。
- `$(join <list1>,<list2>)`把`<list2>`的单词对应地加到`<list1>`的单词后面。如果 `<list1>`的单词个数要比`<list2>`的多,那么,`<list1>`中的多出来的单词将保持原样。如果`<list2>`的单词个数要比`<list1>`多,那么,`<list2>`多出来的单词将被复制到`<list1>`中。
***
- `$(foreach <var>,<list>,<text>)`foreach函数,用来做循环,表示把参数`<list>`中的单词逐一取出放到参数`<var>`所指定的变量中,然后再执行`<text>`所包含的表达式
- 每一次`<text>`会返回一个字符串,循环过程中,`<text>`的所返回的每个字符串会以空格分隔,最后当整个循环结束时,`<text>`所返回的每个字符串所组成的整个字符串(以空格分隔)将会是`foreach`函数的返回值。
``` shell
names := a b c d
files := $(foreach n,$(names),$(n).o).

# a.o b.o c.o d.o
```
- foreach 中的`<var>`参数是一个临时的局部变量,`foreach`函数执行完后,参数`<var>`的变量将不在作用,其作用域只在`foreach`函数当中
***
- if函数,`$(if <condition>,<then-part>)`或`$(if <condition>,<then-part>,<else-part>)`,很像`ifeq`.
-  if 函数的参数可以是两个,也可以是三个。`<condition>`参数是`if`的表达式,如果其返回的为非空字符串,那么这个表达式就相当于返回真,于是,`<then-part>`会被计算,否则 `<else-part>`会被计算
- if 函数的返回值是,如果`<condition>`为真(非空字符串),那个`<then-part>`会是整个函数的返回值,如果`<condition>`为假(空字符串),那么`<else-part>`会是整个函数的返回值,此时如果`<else-part>`没有被定义,那么,整个函数返回空字串。
***
- `$(call <expression>,<parm1>,<parm2>,...,<parmn>)` make 执行这个函数时,`<expression>`参数中的变量,如`$(1)、$(2)`等,会被参数`<parm1>,<parm2>,<parm3>`依次取代。而`<expression` 的返回值就是 call 函数的返回值
``` shell
reverse = $(1) $(2)
foo = $(call reverse,a,b)
#a b
```
***
- `$(origin <variable>)`origin函数不操作变量的值,只是告诉这个变量是哪里来的
- `origin`函数会以其返回值来告诉你这个变量的“出生情况”,下面,是 origin 函数的返回值:
``` shell
undefined
	如果 <variable> 从来没有定义过,origin 函数返回这个值 undefined
default
	如果 <variable> 是一个默认的定义,比如“CC”这个变量,这种变量我们将在后面讲述。
environment
	如果 <variable> 是一个环境变量,并且当 Makefile 被执行时,-e 参数没有被打开。
file
	如果 <variable> 这个变量被定义在 Makefile 中。
command line
	如果 <variable> 这个变量是被命令行定义的。
override
	如果 <variable> 是被 override 指示符重新定义的。
automatic
	如果 <variable> 是一个命令运行中的自动化变量。
```
***
- `shell`函数也不像其它的函数。顾名思义,它的参数应该就是操作系统 Shell 的命令。它和反引号\`是相同的功能.
```　shell
CURRENT_DIR = `pwd`
CURRENT_DIR = $(shell pwd)
```
- `$(error <text ...>)`产生一个致命的错误,`<text ...>`是错误信息。
- error 函数不会在一被使用就会产生错误信息,可以把其定义在某个变量中,并在后续的脚本中使用这个变量
``` shell
ERR = $(error found an error!)

.PHONY: err

err: $(ERR)
```
- `$(warning <text ...>)`输出一段警告信息,make继续执行
***
make 命令执行后有三个退出码:
```
0
表示成功执行。
1
如果 make 运行时出现任何错误,其返回 1。
2
如果你使用了 make 的“-q”选项,并且 make 使得一些目标不需要更新,那么返回 2。
```
- `-f/--file/--makefile`指定makefile文件
- 一般来说,make 的最终目标是 makefile 中的第一个目标,而其它目标一般是由这个目标连带出来的
- 任何在 makefile 中的目标都可以被指定成终极目标,但是除了以 - 打头,或是包含了 = 的目标,因为有这些字符的目标,会被解析成命令行参数或是变量。 
- 甚至没有被我们明确写出来的目标也可以成为make 的终极目标,也就是说,只要 make 可以找到其隐含规则推导规则,那么这个隐含目标同样可以被指定成终极目标。
- 有一个 make 的环境变量叫`MAKECMDGOALS`,这个变量中会存放你所指定的终极目标的列表,如果在命令行上,你没有指定目标,那么,这个变量是空值。
***
*在 Unix 世界中,软件发布时,特别是 GNU这种开源软件的发布时,其 makefile 都包含了编译、安装、打包等功能。我们可以参照这种规则来书写我们的 makefile 中的目标。*
-  all: 这个伪目标是所有目标的目标,其功能一般是编译所有的目标。
- clean: 这个伪目标功能是删除所有被 make 创建的文件。
- install: 这个伪目标功能是安装已编译好的程序,其实就是把目标执行文件拷贝到指定的目标中去。
- print: 这个伪目标的功能是例出改变过的源文件。
- tar: 这个伪目标功能是把源程序打包备份。也就是一个 tar 文件。
- dist: 这个伪目标功能是创建一个压缩文件,一般是把 tar 文件压成 Z 文件。或是 gz 文件。
- TAGS: 这个伪目标功能是更新所有的目标,以备完整地重编译使用。
- check 和 test: 这两个伪目标一般用来测试 makefile 的流程。
***
- `-n, --just-print, --dry-run, --recon`不执行参数,这些参数只是打印命令,不管目标是否更新,把规则和连带规则下的命令打印出来,但不执行,这些参数对于我们调试 makefile 很有用处。
- `-t, --touch`这个参数的意思就是把目标文件的时间更新,但不更改目标文件。也就是说,make 假装编译目标,但不是真正的编译目标,只是把目标变成已编译过的状态。
- `q, --question`这个参数的行为是找目标的意思,也就是说,如果目标存在,那么其什么也不会输出,当然也不会执行编译,如果目标不存在,其会打印出一条出错信息。
- `-W <file>, --what-if=<file>, --assume-new=<file>, --new-file=<file>`这个参数需要指定一个文件。一般是是源文件(或依赖文件),Make 会根据规则推导来运行依赖于这个文件的命令,一般来说,可以和`-n`参数一同使用,来查看这个依赖文件所发生的规则命令。
***
关于make的具体参数,可以通过`man make`查看
***
- make有一些隐含规则
- make 会在自己的“隐含规则”库中寻找可以用的规则,如果找到,那么就会使用。如果找不到,那么就会报错。
- 如果我们不明确地写下规则,那么,make 就会在这些规则中寻找所需要规则和命令。当然,我们也可以使用 make 的参数`-r`或`--no-builtin-rules`选项来取消所有的预设置的隐含规则。
- 即使是我们指定了 -r 参数,某些隐含规则还是会生效,因为有许多的隐含规则都是使用了*\“后缀规则\” 来定义的,所以,只要隐含规则中有“后缀列表”(也就一系统定义在目标 .SUFFIXES 的依赖目标),那么隐含规则就会生效。
- 默认的后缀列表是:`.out, .a, .ln, .o, .c, .cc, .C, .p, .f, .F, .r, .y, .l, .s,.S, .mod, .sym, .def, .h, .info, .dvi, .tex, .texinfo, .texi, .txinfo, .w, .ch .web, .sh, .elc, .el`。
***
隐含规则一览：
1. 编译 C 程序的隐含规则。`<n>.o`的目标的依赖目标会自动推导为`<n>.c`,并且其生成命令是`$(CC) –c $(CPPFLAGS) $(CFLAGS)`
2. 编译 C++ 程序的隐含规则。`<n>.o`的目标的依赖目标会自动推导为`<n>.cc`或是`<n>.C`,并且其生成命令是`$(CXX) –c $(CPPFLAGS) $(CXXFLAGS)`。(建议使用`.cc`作为 C++ 源文件的后缀,而不是 .C )
3. 编译 Pascal 程序的隐含规则。`<n>.o`的目标的依赖目标会自动推导为`<n>.p`,并且其生成命令是`$(PC) –c $(PFLAGS)`。
4. 编译 Fortran/Ratfor 程序的隐含规则.`<n>.o`的目标的依赖目标会自动推导为`<n>.r`或 `<n>.F`或`<n>.f`,并且其生成命令是:
``` shell
 .f $(FC) –c $(FFLAGS)
 .F $(FC) –c $(FFLAGS) $(CPPFLAGS)
 .f $(FC) –c $(FFLAGS) $(RFLAGS)
```
5. 预处理 Fortran/Ratfor 程序的隐含规则。`<n>.f`的目标的依赖目标会自动推导为`<n>.r`或`<n>.F`。这个规则只是转换 Ratfor 或有预处理的Fortran 程序到一个标准的 Fortran 程序。其使用的命令是:
```
 .F $(FC) –F $(CPPFLAGS) $(FFLAGS)
 .r $(FC) –F $(FFLAGS) $(RFLAGS)
```
6. 编译 Modula-2 程序的隐含规则。`<n>.sym`的目标的依赖目标会自动推导为`<n>.def`,并且其生成命令是:`$(M2C) $(M2FLAGS)$(DEFFLAGS)`.`<n>.o`的目标的依赖目标会自动推导为`<n>.mod`,并且其生成命令是:`$(M2C) $(M2FLAGS) $(MODFLAGS)`。
7. 汇编和汇编预处理的隐含规则`<n>.o`的目标的依赖目标会自动推导为`<n>.s`,默认使用编译器`as`,并且其生成命令是:`$ (AS) $(ASFLAGS)`.`<n>.s 的目标的依赖目标会自动推导为 <n>.S ,默认使用 C 预编译器 cpp ,并且其生成命令是:$(AS) $(ASFLAGS) 。
8. 链接 Object 文件的隐含规则。`<n>`目标依赖于`<n>.o`,通过运行 C 的编译器来运行链接程序生成(一般是 ld ),其生成命令是:`$(CC) $(LDFLAGS) <n>.o $(LOADLIBES) $(LDLIBS)`。这个规则对于只有一个源文件的工程有效,同时也对多个 Object 文件(由不同的源文件生成)的也有效。
9. Yacc C 程序时的隐含规则,`<n>.c`的依赖文件被自动推导为`n.y`(Yacc 生成的文件),其生成命令是:`$(YACC) $(YFALGS)`
10.  Lex C 程序时的隐含规则。`<n>.c`的依赖文件被自动推导为`n.l`(Lex 生成的文件),其生成命令是:`$(LEX) $(LFALGS)`
11.  Lex Ratfor 程序时的隐含规则。`<n>.r`的依赖文件被自动推导为`n.` (Lex 生成的文件),其生成命令是:`$(LEX) $(LFALGS)`
12.  从 C 程序、Yacc 文件或 Lex 文件创建 Lint 库的隐含规则.`<n>.ln`(lint 生成的文件)的依赖文件被自动推导为 n.c ,其生成命令是:`$(LINT) $(LINTFALGS) $(CPPFLAGS) -i `。对于`<n>.y`和`<n>.l`也是同样的规则。
***
*make中关于命令的变量*
AR : 函数库打包程序。默认命令是 ar
- AS : 汇编语言编译程序。默认命令是 as
- CC : C 语言编译程序。默认命令是 cc
- CXX : C++ 语言编译程序。默认命令是 g++
- CO : 从 RCS 文件中扩展文件程序。默认命令是 co
- CPP : C 程序的预处理器(输出是标准输出设备)。默认命令是 $(CC) –E
- FC : Fortran 和 Ratfor 的编译器和预处理程序。默认命令是 f77
- GET : 从 SCCS 文件中扩展文件的程序。默认命令是 get
- LEX : Lex 方法分析器程序(针对于 C 或 Ratfor)。默认命令是 lex
- PC : Pascal 语言编译程序。默认命令是 pc
- YACC : Yacc 文法分析器(针对于 C 程序)。默认命令是 yacc
- YACCR : Yacc 文法分析器(针对于 Ratfor 程序)。默认命令是 yacc –r
- MAKEINFO : 转换 Texinfo 源文件(.texi)到 Info 文件程序。默认命令是 makeinfo
- TEX : 从 TeX 源文件创建 TeX DVI 文件的程序。默认命令是 tex
- TEXI2DVI : 从 Texinfo 源文件创建军 TeX DVI 文件的程序。默认命令是 texi2dvi
- WEAVE : 转换 Web 到 TeX 的程序。默认命令是 weave
- CWEAVE : 转换 C Web 到 TeX 的程序。默认命令是 cweave
- TANGLE : 转换 Web 到 Pascal 语言的程序。默认命令是 tangle
- CTANGLE : 转换 C Web 到 C。默认命令是 ctangle
- RM : 删除文件命令。默认命令是 rm –f
***
*关于命令参数的变量*
- ARFLAGS : 函数库打包程序 AR 命令的参数。默认值是 rv
- ASFLAGS : 汇编语言编译器参数。(当明显地调用 .s 或 .S 文件时)
- CFLAGS : C 语言编译器参数。
- CXXFLAGS : C++ 语言编译器参数。
- COFLAGS : RCS 命令参数。
- CPPFLAGS : C 预处理器参数。(C 和 Fortran 编译器也会用到)。
- FFLAGS : Fortran 语言编译器参数。
- GFLAGS : SCCS “get”程序参数。
- LDFLAGS : 链接器参数。(如:ld )
- LFLAGS : Lex 文法分析器参数。
- PFLAGS : Pascal 语言编译器参数。
- RFLAGS : Ratfor 程序的 Fortran 编译器参数。
- YFLAGS : Yacc 文法分析器参数。
***
- 有些时候,一个目标可能被一系列的隐含规则所作用。例如,一个 .o 的文件生成,可能会是先被Yacc 的 `[.y]`文件先成 .c ,然后再被 C 的编译器生成。这一系列的隐含规则叫做“隐含规则链”.
- 这种 .c 的文件(或是目标) ,叫做中间目标。不管怎么样,make 会努力自动推导生成目标的一切方法,不管中间目标有多少,其都会执着地把所有的隐含规则和你书写的规则全部合起来分析
- 在默认情况下,对于中间目标,它和一般的目标有两个地方所不同:第一个不同是除非中间的目标不存在,才会引发中间规则。第二个不同的是,只要目标成功产生,那么,产生最终目标过程中,所产生的中间目标文件会被以`rm -f`删除。
- 使用伪目标` .INTERMEDIATE`来强制声明一个文件或是目标是中介目标
- 使用伪目标`.SECONDARY`来强制阻止`make`自动删除中间目标(如:`.SECONDARY : sec`)。还可以把你的目标,以模式的方式来指定(如:%.o )成伪目标`.PRECIOUS`的依赖目标,以保存被隐含规则所生成的中间文件。
- 在“隐含规则链”中,禁止同一个目标出现两次或两次以上,这样一来,就可防止在 make 自动推导时出现无限递归的情况
- 可以使用模式规则来定义一个隐含规则,一个模式规则就好像一个一般的规则,只是在规则中,目标的定义需要有`%`字符.`%`的意思是表示一个或多个任意字符。在依赖目标中同样可以使用`%`,只是依赖目标中的`%`的取值,取决于其目标.****
- % 的展开发生在变量和函数的展开之后,变量和函数的展开发生在 make 载入 Makefile 时,而模式规则中的 % 则发生在运行时
***
``` shell
# 模式规则example
%.o : %.c ; <command ......>;
```
***
*自动化变量*
- `$@`: 表示规则中的目标文件集。在模式规则中,如果有多个目标,那么,`$@`就是匹配于目标中模式定义的集合。
- `$%`: 仅当目标是函数库文件中,表示规则中的目标成员名。例如,如果一个目标是`foo.a(bar.o)`,那么,`$%`就是`bar.o`,`$@`就是`foo.a`。如果目标不是函数库文件(Unix 下是 `.a`,Windows下是`.lib`),那么,其值为空。
- `$<`: 依赖目标中的第一个目标名字。如果依赖目标是以模式(即`%`)定义的,那么`$<`将是符合模式的一系列的文件集。注意,其是一个一个取出来的。
- `$?`: 所有比目标新的依赖目标的集合。以空格分隔。
- `$^`: 所有的依赖目标的集合。以空格分隔。如果在依赖目标中有多个重复的,那么这个变量会去除重复的依赖目标,只保留一份。
- `$+`: 这个变量很像 $^ ,也是所有依赖目标的集合。只是它不去除重复的依赖目标。
- `$*`: 这个变量表示目标模式中`%`及其之前的部分。如果目标是`dir/a.foo.b`,并且目标的模式是`a.%.b`,那么,`$*`的值就是`dir/foo`。这个变量对于构造有关联的文件名是比较有效。如果目标中没有模式的定义,那么`$*`也就不能被推导出,但是,如果目标文件的后缀是`make`所识别的,那么`$*`就是除了后缀的那一部分.例如:如果目标是`foo.c`,因为`.c`是`make`所能识别的后缀名,所以,`$*`的值就是`foo`。这个特性是 GNU make 的,很有可能不兼容于其它版本的make,所以,你应该尽量避免使用`$*`,除非是在隐含规则或是静态模式中。如果目标中的后缀是 make 所不能识别的,那么`$*`就是空值。
- 这七个自动化变量还可以取得文件的目录名或是在当前目录下的符合模式的文件名,只需要搭配上`D`或`F`字样
- `$(@D)`表示`$@`的目录部分(不以斜杠作为结尾),如果`$@`值是`dir/foo.o`,那么`$(@D)`就是`dir`,而如果`$@`中没有包含斜杠的话,其值就是 . (当前目录)。
- `$(@F)`表示`$@`的文件部分,如果`$@`值是`dir/foo.o`,那么`$(@F)`就是`foo.o`,`$(@F)`相当于函数`$(notdir $@) 。
- `$(*D), $(*F)`和上面所述的同理,也是取文件的目录部分和文件部分。对于上面的那个例子,$(*D) 返回`dir`,而 $(*F) 返回 foo
- ` $(%D),$(%F)`表示函数包文件成员的目录部分和文件部分这对于形同`archive(member` 形式的目标中的 member 中包含了不同的目录很有用。
- `$(<D), $(<F)`分别表示依赖文件的目录部分和文件部分。
- `$(^D), $(^F)`分别表示所有依赖文件的目录部分和文件部分。(无相同的)`$(+D),$(+F)`
分别表示所有依赖文件的目录部分和文件部分。(可以有相同的)
- `$(?D), $(?F)`分别表示被更新的依赖文件的目录部分和文件部分。
***
- 在定义好了的模式中,我们把 % 所匹配的内容叫做“茎”,例如 %.c 所匹配的文件`test.c`中`test`就是“茎(stem)”.因为在目标和依赖目标中同时有 % 时,依赖目标的“茎”会传给目标,当做目标中的“茎”.
- 后缀规则是一个比较老式的定义隐含规则的方法。后缀规则会被模式规则逐步地取代。因为模式规则更强更清晰。
- 让 make 知道一些特定的后缀,我们可以使用伪目标 .SUFFIXES 来定义或是删除
``` shell
.SUFFIXES: .hack .win
###
.SUFFIXES:
 # 删除默认的后缀
.SUFFIXES: .c .o .h
 # 定义自己的后缀
```
***
1. 把`T`的目录部分分离出来。叫`D`而剩余部分叫`N`(如:如果`T`是`src/foo.o`,那么,`D`就是`src/`,`N`就是`foo.o`)
2. 创建所有匹配于`T`或是`N`的模式规则列表。
3. 如果在模式规则列表中有匹配所有文件的模式,如`%`,那么从列表中移除其它的模式。
4. 移除列表中没有命令的规则。
5. 对于第一个在列表中的模式规则:
	1. 推导其“茎”`S`,`S`应该是`T`或是`N`匹配于模式中`%`非空的部分。
	2. 计算依赖文件。把依赖文件中的`%`都替换成“茎”S。如果目标模式中没有包含斜框字符,而把`D`加在第一个依赖文件的开头。
	3. 测试是否所有的依赖文件都存在或是理当存在。(如果有一个文件被定义成另外一个规则的目标文件,或者是一个显式规则的依赖文件,那么这个文件就叫“理当存在”)
	 4. 如果所有的依赖文件存在或是理当存在,或是就没有依赖文件。那么这条规则将被采用,退出该算法。
6. 如果经过第 5 步,没有模式规则被找到,那么就做更进一步的搜索。对于存在于列表中的第一个模式规则:
	1. 如果规则是终止规则,那就忽略它,继续下一条模式规则。
	2. 计算依赖文件。(同第 5 步)
	3. 测试所有的依赖文件是否存在或是理当存在。
	4. 对于不存在的依赖文件,递归调用这个算法查找他是否可以被隐含规则找到。
	5. 如果所有的依赖文件存在或是理当存在,或是就根本没有依赖文件。那么这条规则被采用,退出该算法。
	6. 如果没有隐含规则可以使用,查` .DEFAUL` 规则,如果有,采用,`.DEFAULT`的命令给 T使用。
一旦规则被找到,就会执行其相当的命令,而此时,自动化变量的值才会生成。
***
- 函数库文件及其组成
``` shell
archive(member)

#example1
foolib(hack.o) : hack.o
	ar cr foolib hack.o

#example2
foolib(hack.o kludge.o)
```
- 当 make 搜索一个目标的隐含规则时,一个特殊的特性是,如果这个目标是`a(m)`形式的,其会把目标变成`(m)`
***
使用`-j`可以并行编译
