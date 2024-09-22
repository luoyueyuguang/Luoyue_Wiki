# shell语法
## shell变量
-  `$0 - 脚本名 `
- `$1 到 $9 - 脚本的参数。 $1 是第一个参数，依此类推。`
- `$@ - 所有参数`
- `$# - 参数个数`
- `$? - 前一个命令的返回值`
- `$$ - 当前脚本的进程识别码`
* `!! - 完整的上一条命令，包括参数。常见应用：当你因为权限不足执行命令失败时，可以使用 sudo !!再尝试一次。`
- `$_ - 上一条命令的最后一个参数。如果你正在使用的是交互式 shell，你可以通过按下 Esc 之后键入 . 来获取这个值。`

## shell语法

# awk 
### awk的执行方式
- `pattern { action }`awk程序是由`模式-动作`组成的序列
- `awk 'program'`会交互式执行
- `awk -f progfile optional-files`progfile是由program组成的
### 打印
- `{ print }`或`{ print $0 }`表示一整行
- `$1`或`$3`等可以打印具体的条目,例`{print $1, $3}`
- `NF`表示打印字段的数量
- 字段也可以计算,`{$2 * $3}`
- `NR`表示行号
- 可以放入单词到字段中,`{ print "total pay for", $1, "is", $2 * $3 }`
- `printf(format, value1, value2 , ... , valuen )`可以使用`printf`来进行格式化,例:`{ printf("total pay for %s is $%.2f\n", $1, $2 * $3) }``printf`不会自动产生空格符和换行符
- 