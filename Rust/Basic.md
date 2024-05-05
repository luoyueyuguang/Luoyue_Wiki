## 变量绑定与解构
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