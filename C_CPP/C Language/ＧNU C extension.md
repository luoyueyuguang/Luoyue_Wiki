- 在编译时加入`-pedantic`选项会对拓展做出警告
# attribute
```
__attribute__ ((attribute-list))
```

| attribute-list                                           | meaning                             |
| -------------------------------------------------------- | ----------------------------------- |
| aligned(n)                                               | 指定变量的对齐方式，n表示对齐字节数                  |
| packed                                                   | 指定结构体或联合体的成员按照1字节对齐。                |
| section(“name”)                                          | 指定变量或函数所在的段名。                       |
| unused                                                   | 告诉编译器该变量或函数未被使用，避免编译器产生警告。          |
| noreturn                                                 | 告诉编译器该函数不会返回，避免编译器产生警告。             |
| format                                                   | 指定函数的参数格式，用于检查printf和scanf等函数的参数类型。 |
| constructor                                              | 指定函数为构造函数，在程序启动时自动执行。               |
| destructor                                               | 指定函数为析构函数，在程序结束时自动执行                |
| noinline                                                 |                                     |
| always_inline                                            |                                     |
| pure                                                     |                                     |
| `format (`archetype`,` string-index`,` first-to-check`)` |                                     |
## aligned(n)
指定变量n字节对齐,n表示字节对齐树,不止适用于结构
## packed
__attribute__((packed)): 取消结构在编译过程中的优化对齐，也可以认为是1字节对齐。
## section
用__attribute__ 来声明一个 section 属性，主要用途是在程序编译时，将一个函数或变量放到指定的段
## unused
告诉编译器该变量或函数未被使用，避免编译器产生警告。

## constructor
当在一个函数声明或定义前加上__attribute__((constructor))属性时，就会告诉编译器，在程序加载时（在main函数执行之前），需要自动调用这个函数。这个特性通常用于在程序启动时执行一些全局的初始化工作。
constructor可以有优先级，指定优先级时，先执行优先级小的，再执行优先级大的，最后执行没有指定优先级。

## destructor
若函数被设定为destructor属性，则该函数会在main（）函数执行之后或者exit（）被调用后被自动的执行。

