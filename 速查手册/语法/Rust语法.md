

# Rust语法


[TOC]


## 基础

* 强类型语言，可以自动判断变量类型
* 表达式语言：控制流和块都是表达式，会产生值
    * 块的最后一行无`;`，会产生一个值


### 设计原则
* 开销应该对程序员`显而易见`，基本操作必须保持简单


### 注释
* `// ...`  行注释
* `/* ... */` 块注释
* `/// ...`   外部文档注释
* `//! ...`   内部文档注释

### 变量
* `let <name>: <type> = <value>` 定义变量
* `let mut <name>: <type> = <value>` 定义可变变量
* `const <name> = <value>`  定义常量


#### 示例
```rust
let a = 123;
let mut b = 10;
const c: i32 = 123;
```


## 所有权

### 规则
* 每个值都有一个所有者
* 同一个时间只能有一个所有者
* 当所有者离开作用域时，值被丢弃

#### 含义
* 可以通过`树`来描述所有权关系


### 转移
* 原来的变量让渡值的所有权给目标变量，并变成未初始化状态
* 若目标变量已初始化，则该值会被清除
* 转移的数据只是内容的`特征值`
* `Copy`类型的赋值会复制值，而不是转移值
    * 数值类型，`bool`，元组，数组

### 共享
* `let <var> = Rc::new(...)` 带引用计数功能的指针
* `let <var> = Arc::new(...)` 带引用计数功能的指针，线程安全

#### 示例
```rust
let s: Rc<String> = Rc::new("test".to_string());
let t: Rc<String> = s.clone();   //该操作不会复制值
```

### 引用
* 一种非所有权的指针，对所引用资源的生命周期没有影响
* `借用`：创建对某个值的引用
* `&` 创建引用
* `*` 解引用
* `&T` 共享引用，只读型访问
* `&mut T` 可修改引用，排他型访问
* `.`操作符会对左操作数进行隐式解引用
* 给引用赋值会使其指向新值
* 引用可以嵌套

#### 示例
```rust
let mut y = 32;
let m = &mut y;
*m += 32;     //显示解引用

let x = 10;
let y = 20;

let mut r = &x;
r = &y;    //r会指向y

let point = Point { x: 100, y: 200};
let r: &Point = &point;
let rr: &&Point = &r;
let rrr: &&&Point = &rr;
assert_eq!(rrr.y, 200);  //rust会找到原始值
```



## 表达式


## 类型

### 布尔
* `bool`
* 值：`true`，`false`

### 数字
* 无符号：`u8`，`u16`, `u32`, `u64`
* 有符号：`i8`，`i16`, `i32`, `i64`
* 浮点：`f32`，`f64`


### 字符串
* `let <name>: &str = <value>`   定义字面量
* `let <name> = String::new();`  创建空字符串
* `let <name> = String::from(<value>)` 从字面量创建
* `let <name> = <value>.to_string()` 从字面量创建
* `let <name> = format!("<expr>")`   格式化字符串


#### 示例
```rust
let mut s = String::new();
let s = "hello".to_string();
let s = format!("{} {}", "hello", "world");

let s = 42.to_string(); 
```



### 函数
* `fn <name>(arg: <type>) -> <type> { }`  定义函数


### 闭包
* `|<name>:<type>| -> <type> ...;`      创建闭包，借用`捕获`的变量
* `|<name>:<type>| -> <type> { ... };`  创建，借用`捕获`的变量

* `move |<name>:<type>| -> <type> ...;` 创建闭包，转移`捕获`的变量



### 结构体


### Box
* 是一个指针，指向堆中的值
* `let <var> = Box::new(<val>)` 创建并转移值 

### 指针大小
* 有符号：`isize`
* 无符号：`usize`



### 枚举
* `enum <name> { ... }` 定义枚举
* `enum <name><T> { ... }` 定义泛型枚举
* `impl <name> { ... }` 定义方法



#### 示例
```rust
enum Ordering {
    Less,
    Equal,
    Greater
}

enum HttpStatus {
    Ok = 200,
    NotModified = 304,
    NotFound = 404,
}

enum TimeUnit {
    Secondes, Minutes, Hours,
}

impl TimeUnit {

    fu singular(self) -> &`static str {
        self.plural().trim_right_matches('s')
    }
}

enum Option<T> {
    None,
    Some(T)
}

enum Result<T, E> {
    Ok(T),
    Err(E),
}
```


## 集合


### 元组
* 每个元素可以是不同的类型
* 用于函数返回多个值

* `let <tuple> = (<value>,<value>,...)` 创建
* `let <name> = <tuple>.<index>`  访问元素

#### 示例
```rust
let tuple = (1, 'A', "Cool", 78)
```

### 数组
数组大小是不变的，是类型的一部分
每个元素都是相同类型

* `let <array>: [<type>;<len>] = [ ... ]`  定义数组


#### 示例
```rust
let array: [i64; 6] = [1,2,3,4,5,6];

let mut array: [i32: 3] = [2,4,6];
array[1] = 4;
```

### 向量
大小是可变的

* `let <vec> = Vec::new();`         创建空向量
* `let <vec>: Vec<T> = vec![];`     创建空向量
* `let <vec> = Vec::with_capacity(<size>);`  创建指定容量的空向量
* `let <vec>: Vec<T> = vec![...];`  从数组创建向量

* `let <ref> = &<vec>[<index>];`          获取元素的引用
* `let <var> = <vec>[<index>];`           获取元素的副本
* `let <slice> = &<vec>[<begin>..<end>];` 获取切片

* `<vec>.push(<val>);`                尾部插入元素
* `let <opt> = <vec>.pop();`          移除并返回尾部的元素
* `<vec>.insert(<index>, <val>);`      指定位置插入元素
* `let <val> = <vec>.remove(<index>);`   指定位置删除元素

* `<vec>.clear();`                    清空向量
* `<vec>.extend(<iter>);`             尾部插入所有项

### 切片
只能按引用传递
类型为：`&[T]`，`&mut [T]`
切片上的方法可以直接用到数组、向量上

* `let <opt> = <slice>.first()`   获取第一个元素
* `let <opt> = <slice>.last()`    获取最后一个元素
* `let <opt> = <slice>.get(<index>);`  获取指定元素

* `let mut <opt> = <slice>.first_mut()`   获取第一个元素，可修改
* `let mut <opt> = <slice>.last_mut()`    获取最后一个元素，可修改
* `let mut <opt> = <slice>.get_mut(<index>);`  获取指定元素，可修改

* `let <vec> = <slice>.to_vec()`   克隆切片，返回新向量

* `let <usize> = <slice>.len();`          获取长度
* `let <bool> = <slice>.is_empty();`    判断是否为空

* 
