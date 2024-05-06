## 概念
### 变量与可变性
- 变量绑定
使用`let v = 5`来进行绑定
- rust变量默认不可变， 可变性需要添加mut
```
let x = 5 //不可变
let mut x = 5 //可变
```
- 使用下划线开头忽略未使用的变量
`let _x = 5 //可以被忽略，编译器不会爆出警告`
- 变量解构
`let (a, mut b): (bool,bool) = (true, false);`
- 解构式赋值
```
struct Struct { e: i32 }

fn main() {
	Struct { e, .. } = Struct { e: 5 };
	[c, .., d, _] = [1, 2, 3, 4, 5]; //_代表匹配一个值
}
```
`let` 会重新绑定，而这里仅仅是对之前绑定的变量进行再赋值。
- 常量
```
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```
用const来进行声明,rust 对常量的命名约定是在单词之间使用全大写加下划线
- rust可以定义一个与之前变量同名的新变量,当再次使用 `let` 时，实际上创建了一个新变量,
### 数据类型
#### 整型
| 长度      | 有符号     | 无符号     |
| ------- | ------- | ------- |
| 8-bit   | `i8`    | `u8`    |
| 16-bit  | `i16`   | `u16`   |
| 32-bit  | `i32`   | `u32`   |
| 64-bit  | `i64`   | `u64`   |
| 128-bit | `i128`  | `u128`  |
| arch    | `isize` | `usize` |

| 数字字面值                 | 例子           |     |
| --------------------- | ------------ | --- |
| Decimal (十进制)         | `98_222`     |     |
| Hex (十六进制)            | `0xff`       |     |
| Octal (八进制)`0o77`     |              |     |
| Binary (二进制)`         | 0b1111_0000` |     |
| Byte (单字节字符)(仅限于`u8`) | `b'A'`       |     |
数字字面值允许使用类型后缀，例如 `57u8` 来指定类型，同时也允许使用 `_` 做为分隔符以方便读数，例如`1_000`
#### 浮点型

| 精度  | 类型  |
| --- | --- |
| 单精度 | f32 |
| 双精度 | f64 |

- 默认为双精度
#### 数值运算
``` rust
fn main() {
    // addition
    let sum = 5 + 10;

    // subtraction
    let difference = 95.5 - 4.3;

    // multiplication
    let product = 4 * 30;

    // division
    let quotient = 56.7 / 32.2;
    let truncated = -5 / 3; // 结果为 -1

    // remainder
    let remainder = 43 % 5;
}
```
#### 布尔型
`true` or `false`
#### 字符类型
- Rust 的 `char` 类型是语言中最原生的字母类型。
``` rust
fn main() {
    let c = 'z';
    let z: char = 'ℤ'; // with explicit type annotation
    let heart_eyed_cat = '😻';
}
```
- 用单引号声明 `char` 字面量，而与之相反的是，使用双引号声明字符串字面量。Rust 的 `char` 类型的大小为四个字节 (four bytes)，并代表了一个 Unicode 标量值（Unicode Scalar Value），这意味着它可以比 ASCII 表示更多内容。
#### 复合类型
| 类型        | 样式             |
| --------- | -------------- |
| 元组(tuple) | `(v1, v2, v3)` |
| 数组(array) | `[v1, v2, v3]` |
- 元组是一个将多个其他类型的值组合进一个复合类型的主要方式。元组长度固定：一旦声明，其长度不会增大或缩小。
``` rust
fn main() {
    let tup: (i32, f64, u8) = (500, 6.4, 1);
}

fn main() {
    let tup = (500, 6.4, 1);

    let (x, y, z) = tup;

    println!("The value of y is: {y}");
}

fn main() {
    let x: (i32, f64, u8) = (500, 6.4, 1);

    let five_hundred = x.0;

    let six_point_four = x.1;

    let one = x.2;
}
```
- 程序首先创建了一个元组并绑定到 `tup` 变量上。接着使用了 `let` 和一个模式将 `tup` 分成了三个不同的变量，`x`、`y` 和 `z`。这叫做 **解构**（_destructuring_）
- 不带任何值的元组有个特殊的名称，叫做 **单元（unit）** 元组。这种值以及对应的类型都写作 `()`，表示空值或空的返回类型。如果表达式不返回任何其他值，则会隐式返回单元值。
***
- 数组中的每个元素的类型必须相同。Rust 中的数组与一些其他语言中的数组不同，Rust 中的数组长度是固定的。
- 可以通过数组索引来访问元素,访问越界rust会报错 

### 函数与注释
- `fn` 关键字用来声明新函数
- Rust 代码中的函数和变量名使用 _snake case_ 规范风格。所有字母都是小写并使用下划线分隔单词。
``` rust
fn main() {
    println!("Hello, world!");

    another_function();
}

fn another_function() {
    println!("Another function.");
}
```
***
- 我们可以定义为拥有 **参数**（_parameters_）的函数，参数是特殊变量，是函数签名的一部分。当函数拥有参数（形参）时，可以为这些参数提供具体的值（实参）。
``` rust
fn main() {
    another_function(5);
}

fn another_function(x: i32) {
    println!("The value of x is: {x}");
}
```
***
- rust 是一门基于表达式（expression-based）的语言
-  **语句**（_Statements_）是执行一些操作但不返回值的指令。**表达式**（_Expressions_）计算并产生一个值
``` rust
//这是语句
let y = 6
//这也是语句，整个函数都是语句
fn main() {
    let y = 6;
}
```
- 表达式会计算出一个值,函数调用是一个表达式。宏调用是一个表达式。用大括号创建的一个新的块作用域也是一个表达式
```rust
fn main() {
    let y = {
        let x = 3;
        x + 1
    };

    println!("The value of y is: {y}");
}

//是一个表达式
{
    let x = 3;
    x + 1
}
```

***
- 函数可以向调用它的代码返回值,rust不对返回值命名，但要在箭头（`->`）后声明它的类型
- 函数的返回值等同于函数体最后一个表达式的值。使用 `return` 关键字和指定值，可从函数中提前返回；但大部分函数隐式的返回最后的表达式。
``` rust
fn five() -> i32 {
    5
}

fn main() {
    let x = five();

    println!("The value of x is: {x}");
}
```
***
- 在 Rust 中，惯用的注释样式是以两个斜杠开始注释，并持续到本行的结尾。对于超过一行的注释，需要在每一行前都加上 `//`
``` rust
#![allow(unused)]
fn main() {
// So we’re doing something complicated here, long enough that we need
// multiple lines of comments to do it! Whew! Hopefully, this comment will
// explain what’s going on.
}
```
- Rust 还有另一种注释，称为文档注释
### 控制流
- `if` 表达式允许根据条件执行不同的代码分支。你提供一个条件并表示 “如果条件满足，运行这段代码；如果条件不满足，不运行这段代码。”
``` rust
fn main() {
    let number = 3;

    if number < 5 {
        println!("condition was true");
    } else {
        println!("condition was false");
    }
}
```
- 代码中的条件 **必须** 是 `bool` 值。条件不是 `bool` 值，将得到一个错误,**rust并不会转换数据类型**
- 可以将 `else if` 表达式与 `if` 和 `else` 组合来实现多重条件。
``` rust
fn main() {
    let number = 6;

    if number % 4 == 0 {
        println!("number is divisible by 4");
    } else if number % 3 == 0 {
        println!("number is divisible by 3");
    } else if number % 2 == 0 {
        println!("number is divisible by 2");
    } else {
        println!("number is not divisible by 4, 3, or 2");
    }
}
```
- 因为 `if` 是一个表达式，我们可以在 `let` 语句的右侧使用它
``` rust
fn main() {
    let condition = true;
    let number = if condition { 5 } else { 6 };

    println!("The value of number is: {number}");
}
```
***
- rust 提供了多种 **循环**（_loops_,Rust 有三种循环：`loop`、`while` 和 `for`

- `loop` 关键字告诉 Rust 一遍又一遍地执行一段代码直到你明确要求停止
``` rust
fn main() {
    loop {
        println!("again!");
    }
}
```
- 可以使用 `break` 关键字来告诉程序何时停止循环
- 循环中的 `continue` 关键字告诉程序跳过这个循环迭代中的任何剩余代码，并转到下一个迭代
- 可以从循环中返回值
``` rust
fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            break counter * 2;
            //返回counter * 2
        }
    };

    println!("The result is {result}");
}
```
- 如果存在嵌套循环，`break` 和 `continue` 应用于此时最内层的循环。你可以选择在一个循环上指定一个 **循环标签**（_loop label_），然后将标签与 `break` 或 `continue` 一起使用
``` rust
fn main() {
    let mut count = 0;
    'counting_up: loop {
        println!("count = {count}");
        let mut remaining = 10;

        loop {
            println!("remaining = {remaining}");
            if remaining == 9 {
                break;
            }
            if count == 2 {
                break 'counting_up;
            }
            remaining -= 1;
        }

        count += 1;
    }
    println!("End count = {count}");
}
```
- rust也支持while循环
``` rust
fn main() {
    let mut number = 3;

    while number != 0 {
        println!("{number}!");

        number -= 1;
    }

    println!("LIFTOFF!!!");
}
```
- 可以使用 `for` 循环来对一个集合的每个元素执行一些代码
``` rust
fn main() {
    let a = [10, 20, 30, 40, 50];

    for element in a {
        println!("the value is: {element}");
    }
}
```	