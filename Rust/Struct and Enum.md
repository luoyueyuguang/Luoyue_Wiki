## [结构体的定义和实例化](https://kaisery.github.io/trpl-zh-cn/ch05-01-defining-structs.html#%E7%BB%93%E6%9E%84%E4%BD%93%E7%9A%84%E5%AE%9A%E4%B9%89%E5%92%8C%E5%AE%9E%E4%BE%8B%E5%8C%96)
## [结构体示例程序](https://kaisery.github.io/trpl-zh-cn/ch05-02-example-structs.html#%E7%BB%93%E6%9E%84%E4%BD%93%E7%A4%BA%E4%BE%8B%E7%A8%8B%E5%BA%8F)
## [方法语法](https://kaisery.github.io/trpl-zh-cn/ch05-03-method-syntax.html#%E6%96%B9%E6%B3%95%E8%AF%AD%E6%B3%95)

***
***Struct***
**如何定义**
 - 使用字段定义
``` rust
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}
```

- 初始化
``` rust
fn build_user(email: String, username: String) -> User {
    User {
        active: true,
        username: username,
        email: email,
        sign_in_count: 1,
    }
}
```
- 字段初始化简写
``` rust
fn build_user(email: String, username: String) -> User {
    User {
        active: true,
        username,
        email,
        sign_in_count: 1,
    }
}
```
- 结构体更新语法
``` rust
fn main() {
    // --snip--

    let user2 = User {
        active: user1.active,
        username: user1.username,
        email: String::from("another@example.com"),
        sign_in_count: user1.sign_in_count,
    };
}

fn main() {
    // --snip--

    let user2 = User {
        email: String::from("another@example.com"),
        ..user1
    };
}
```

**没有命名字段的元组结构体**
``` rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

fn main() {
    let black = Color(0, 0, 0);
    let origin = Point(0, 0, 0);
}
```

**类单元结构体**（_unit-like structs_）_没有任何字段的结构体_
``` rust
struct AlwaysEqual;

fn main() {
    let subject = AlwaysEqual;
}
```

- 方法使用impl来实现，impl可以有多个
``` rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!(
        "The area of the rectangle is {} square pixels.",
        rect1.area()
    );
}
```
- 多参数方法
``` rust
impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }

    fn can_hold(&self, other: &Rectangle) -> bool {
        self.width > other.width && self.height > other.height
    }
}

```
- 有在 `impl` 块中定义的函数被称为 **关联函数**（_associated functions_）
- 可以定义不以 self 为第一参数的关联函数（因此不是方法）
``` rust
impl Rectangle {
    fn square(size: u32) -> Self {
        Self {
            width: size,
            height: size,
        }
    }
}

```

***
***Enum***
``` rust
enum IpAddrKind {
    V4,
    V6,
}

```
- 枚举的成员位于其标识符的命名空间中
``` rust
    let four = IpAddrKind::V4;
    let six = IpAddrKind::V6;
```
- 可以这样使用枚举
```rust
fn route(ip_kind: IpAddrKind) {}
```
- 枚举可以有多种多样的类型
``` rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}
```

*Option类型*
``` rust
enum Option<T> {
    None,
    Some(T),
}
```