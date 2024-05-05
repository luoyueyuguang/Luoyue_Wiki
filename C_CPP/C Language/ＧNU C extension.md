# attribute
```
__attribute__ ((attribute-list))
```

| attribute-list  | meaning                             |
| --------------- | ----------------------------------- |
| aligned(n)      | 指定变量的对齐方式，n表示对齐字节数                  |
| packed          | 指定结构体或联合体的成员按照1字节对齐。                |
| section(“name”) | 指定变量或函数所在的段名。                       |
| unused          | 告诉编译器该变量或函数未被使用，避免编译器产生警告。          |
| noreturn        | 告诉编译器该函数不会返回，避免编译器产生警告。             |
| format          | 指定函数的参数格式，用于检查printf和scanf等函数的参数类型。 |
| constructor     | 指定函数为构造函数，在程序启动时自动执行。               |
| destructor      | 指定函数为析构函数，在程序结束时自动执行                |


