- 所有程序都必须管理其运行时使用计算机内存的方式。一些语言中具有垃圾回收机制，在程序运行时有规律地寻找不再使用的内存；在另一些语言中，程序员必须亲自分配和释放内存。Rust 则选择了第三种方式：通过所有权系统管理内存，编译器在编译时会根据一系列的规则进行检查。如果违反了任何这些规则，程序都不能编译。在运行时，所有权系统的任何功能都不会减慢程序。
- 跟踪哪部分代码正在使用堆上的哪些数据，最大限度的减少堆上的重复数据的数量，以及清理堆上不再使用的数据确保不会耗尽空间，这些问题正是所有权系统要处理的。
## rules
>1. Rust 中的每一个值都有一个 **所有者**（_owner_）。
>2. 值在任一时刻有且只有一个所有者。
>3. 当所有者（变量）离开作用域，这个值将被丢弃。

***
- Rust 采取了一个不同的策略：内存在拥有它的变量离开作用域后就被自动释放。
``` rust
fn main() {
    {
        let s = String::from("hello"); // 从此处起，s 是有效的

        // 使用 s
    }               // 此作用域已结束 +
				    // s 不再有效
}
```
- 当 `s` 离开作用域的时候。当变量离开作用域，Rust 为我们调用一个特殊的函数。这个函数叫做 [`drop`](https://doc.rust-lang.org/std/ops/trait.Drop.html#tymethod.drop)，在这里 `String` 的作者可以放置释放内存的代码。Rust 在结尾的 `}` 处自动调用 `drop`
### 移动
``` rust
    let s1 = String::from("hello");
    let s2 = s1;// s1成为了无效引用

    println!("{}, world!", s1);
```

Rust 永远也不会自动创建数据的 “深拷贝”。因此，任何 **自动** 的复制都可以被认为是对运行时性能影响较小的。

![[Pasted image 20240508155330.png]]
***
### 克隆
``` rust
    let s1 = String::from("hello");
    let s2 = s1.clone();

    println!("s1 = {}, s2 = {}", s1, s2);
```
- 深度复制 `String` 中堆上的数据，而不仅仅是栈上的数据，可以使用一个叫做 `clone` 的通用函数。
*** 
### [只在栈上的数据：拷贝](https://kaisery.github.io/trpl-zh-cn/ch04-01-what-is-ownership.html#%E5%8F%AA%E5%9C%A8%E6%A0%88%E4%B8%8A%E7%9A%84%E6%95%B0%E6%8D%AE%E6%8B%B7%E8%B4%9D)
``` rust
    let x = 5;
    let y = x;

    println!("x = {}, y = {}", x, y);

```
- 像整型这样的在编译时已知大小的类型被整个存储在栈上，所以拷贝其实际的值是快速的
- Rust 有一个叫做 `Copy` trait 的特殊注解，可以用在类似整型这样的存储在栈上的类型上（[第十章](https://kaisery.github.io/trpl-zh-cn/ch10-00-generics.html)将会详细讲解 trait）。如果一个类型实现了 `Copy` trait，那么一个旧的变量在将其赋值给其他变量后仍然可用。
- Rust 不允许自身或其任何部分实现了 `Drop` trait 的类型使用 `Copy` trait。
可copy的类型
- 所有整数类型，比如 `u32`。
- 布尔类型，`bool`，它的值是 `true` 和 `false`。
- 所有浮点数类型，比如 `f64`。
- 字符类型，`char`。
- 元组，当且仅当其包含的类型也都实现 `Copy` 的时候。比如，`(i32, i32)` 实现了 `Copy`，但 `(i32, String)` 就没有。
***
### [所有权与函数](https://kaisery.github.io/trpl-zh-cn/ch04-01-what-is-ownership.html#%E6%89%80%E6%9C%89%E6%9D%83%E4%B8%8E%E5%87%BD%E6%95%B0)
- 将值传递给函数与给变量赋值的原理相似。向函数传递值可能会移动或者复制，就像赋值语句一样。
``` rust
fn main() {
    let s = String::from("hello");  // s 进入作用域

    takes_ownership(s);             // s 的值移动到函数里 ...
                                    // ... 所以到这里不再有效

    let x = 5;                      // x 进入作用域

    makes_copy(x);                  // x 应该移动函数里，
                                    // 但 i32 是 Copy 的，
                                    // 所以在后面可继续使用 x

} // 这里，x 先移出了作用域，然后是 s。但因为 s 的值已被移走，
  // 没有特殊之处

fn takes_ownership(some_string: String) { // some_string 进入作用域
    println!("{}", some_string);
} // 这里，some_string 移出作用域并调用 `drop` 方法。
  // 占用的内存被释放

fn makes_copy(some_integer: i32) { // some_integer 进入作用域
    println!("{}", some_integer);
} // 这里，some_integer 移出作用域。没有特殊之处
```
***
### [返回值与作用域](https://kaisery.github.io/trpl-zh-cn/ch04-01-what-is-ownership.html#%E8%BF%94%E5%9B%9E%E5%80%BC%E4%B8%8E%E4%BD%9C%E7%94%A8%E5%9F%9F)
- 返回值也可以转移所有权。
``` rust
fn main() {
    let s1 = gives_ownership();         // gives_ownership 将返回值
                                        // 转移给 s1

    let s2 = String::from("hello");     // s2 进入作用域

    let s3 = takes_and_gives_back(s2);  // s2 被移动到
                                        // takes_and_gives_back 中，
                                        // 它也将返回值移给 s3
} // 这里，s3 移出作用域并被丢弃。s2 也移出作用域，但已被移走，
  // 所以什么也不会发生。s1 离开作用域并被丢弃

fn gives_ownership() -> String {             // gives_ownership 会将
                                             // 返回值移动给
                                             // 调用它的函数

    let some_string = String::from("yours"); // some_string 进入作用域。

    some_string                              // 返回 some_string 
                                             // 并移出给调用的函数
                                             // 
}

// takes_and_gives_back 将传入字符串并返回该值
fn takes_and_gives_back(a_string: String) -> String { // a_string 进入作用域
                                                      // 

    a_string  // 返回 a_string 并移出给调用的函数
}
```
