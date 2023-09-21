

# Go语法

[TOC]


## 基础

### 命名规则
* `大写字母开头`  公有变量、公有函数、公用类型
* `小写字母开头`  私有变量、私有函数、私有类型


### 变量
* `var <name> <type>` 定义变量
* `var <name> <name> ... <type>` 定义多个变量
* `var <name> <type> = <value>`  定义变量并初始化
* `<name> := <value>`  简短声明变量，只用在函数内部
* `var( ... )` 分组定义变量
* `_` 特殊变量名，赋予的值会被丢弃
* `const <name> = <value>`  定义常量
* `const( ... )` 分组定义常量


#### 示例
```go
_, b := 34, 35

var(
    i int
    pi float32
    prefix string
)

const (
    i = 100
    pi = 3.1415
    prefix = "Go_"
)
```


### 创建对象
* `*T = new(T)`  用于各种类型的内存分配
* `T = make(T, args)` 用于map, slice, channel的内存分配


### 错误处理
* `error`接口用于定义错误
* `defer`关键字用于错误时的资源释放，最后的语句先执行
* `panic()`函数用于上报错误，开始错误处理
* `recover()`函数用于终止错误处理流程

#### 示例
```go
type error interface {
    Error() string
}


func Copy(dst string, src string) (w, err) {
    srcFile, err = os.Open(src)
    defer srcFile.Close()

    dstFile, err = os.Open(dst)
    defer dstFile.Close()

    defer func() {
        //other clean
    }

    return io.Copy(dstFile, srcFile)
}


panic(404)
panic("network broken")
panic(Error("..."))

```


### 包
* `package <pkgName>` 定义文件所属的包
    * `main`包会编译成可执行文件
    * 其他包会编译成包文件
* `import <pkgname>`  使用绝对路径导入
    * 目录：`gopath/src/pkgname`
* `import( . <pkgname>)` 调用函数时，可以省略包名
* `import( f <pkgname>)` 重新定义导入的包名


#### 示例
```go
import(
    . "fmt"
)

Println("xxxx")


import(
    f "fmt"
)

f.Println("xxxx")

```


### 流程控制
* `switch`中单个`case`是默认退出的


#### 示例
```go
switch {
    case 0 <= Num && Num <= 3 :
        ...
    case 4 <= Num && Num <=6 :
        ...
}
```



## 类型

### Any类型
* `interface {}` 空接口

### 布尔
* `bool`
* 值：`true`,`false`


### 枚举
* `iota` 默认值为0，每次调用加一

#### 示例
```go
const {
    x = iota  // x == 0
    y = iota  // y == 1
    z = iota  // z == 2
    w         // w == 3
}
```


### 数字
* 无符号：`uint8`(`byte`), `uint16`, `uint32`(`rune`), `uint64`
* 有符号：`int8`, `int16`, `int32`, `int64`
* 浮点：`float32`, `float64`


### 字符串
* 使用utf-8编码
* `"xxxx"`   定义可转义字符串
* 反引号(`)   定义Raw字符串，不可转义
* `<str> := <str> + <str>`  连接字符串
* `<var> := []byte(<str>)`  字符串转换为数组类型


### 函数
* 支持多返回值
* `_` 跳过返回值
* `func <name>(arg <type>, ...) (<type>, ...) { }`  定义函数
* `func <name>(arg <type>, ...) (<name> <type>, ...) { }`  定义函数
* `func <name>(arg ...<type>)`  定义变参
* `type <name> func(<type>, ...) (<type>, ...)`  定义函数类型


#### 示例
```go

func Add(a int, b int) (ret int, err error) {
    ...
    return a + b, nil
}

func ReadWrite() bool {
    f, err := os.Open('xxxx')
    if err != nil {
        log.Fatal(err)
    }
    n, _ = f.Read(buf)
    ....
}
```


### 结构
* `type <name> struct { <name> <type> ... }`  声明结构
* `<val> = <struct>.<name>`  读取属性
* `<struct>.<name> = <val>`  设置属性
* 使用匿名字段进行结构内嵌


#### 示例
```go
type Person struct {
    name string
    age int
}

var P Person
P.name = "xxx"
P.age = 25

P := Person{ age: 24, name: "xxx" }


type Student struct {
    Person
    speciality string
}

mark := Student{ Person{"xxx", 25}, "yyy"}
name = mark.name
```

### 接口
* 非侵入式的：只要实现了接口要求的所有函数，就实现了该接口
* 接口由使用方按需求自定义
* `type <name> interface { ... }` 定义

#### 示例
```go
type IFile interface {
    func Read(buf []byte) (n int, err error)
    func Write(buf []byte) (n int, err error)
}

```
    

## 集合

### 数组(array)
* 长度也是数组类型的一部分
* 数组不能改变长度
* 数组是值类型（作为参数传递时会进行复制）
* `var <name> [<len>]<type>`  定义数组
* `<name> := [<len>]<type>{ ... }`  定义并初始化数组
* `<name> := [...]<type>{ ... }`    定义并初始化数组
* `<val> = <name>[<index>]`  读取值
* `<name>[<index>] = <val>`  赋值

#### 示例
```go
array := [5] {1, 2, 3, 4, 5}
length := len(array) 

for i:=0; i<len(array); i++ {
    ...
}

for i, v := range(array) {
    ...
}
```


### 切片(slice)
* 对数组的一个连续片段的引用
* `var <name> []<type>`  定义切片
* `<name> := []<type> { <val>, ... }` 定义并初始化切片
* `<name> := make([]<type>, <num>, <cap>)`  创建切片
* `<slice> = <slice>[<begin>:<end>]` 获取值，`<end>`不包含
    * `<slice>[:n]` => `<slice>[0:n]`
    * `<slice>[n:]` => `<slice>[n:len(<slice>)]`
    * `<slice>[:]` => `<slice>[0:len(<slice>)]`
* `<int> = len(<slice>)` 获取长度
* `<int> = cap(<slice>)`  获取最大容量
* `append(<slice>, <item>...)` 添加元素
* `copy(<dst-slice>, <src-slice>)`   拷贝元素
* `for <index>, <val> := range <slice>` 遍历切片

### 字典(map)
* `var <name> map[<key-type>]<val-type>`  定义字典
* `<name> := map[<key-type>]<val-type> { <key>:<val>, ... }`  定义并初始化字典
* `<name> = make(map[<key-type>]<val-type>, <cap>)`  创建字典
* `delete(<map>, <key>)`  删除元素
* `<int> = len(<map>)`     获取大小
* `<val>, <ok> = <map>[<key>]`   获取值（值，是否找到）
* `<map>[<key>] = <val>`   设置值
* `for <key>, <val> := range <map>` 遍历字典
* `for <key> := range <map>` 只遍历字典键






## 并发

### 协程
* `go <func-name>(...)` 使用协程运行函数
* `runtime.Gosched()`   让出CPU时间片
* `runtime.Goexit()`    退出当前协程

### 管道
* 接收和发送数据都是阻塞的
* `<name> := make(chan <type>)`  创建非缓存管道
* `<name> := make(chan <type>, <int>)`  创建缓存管道
* `<chan> <- <val>` 发送值到管道
* `<var> := <- <chan>` 从管道中接收值，赋值给变量
* `close(<chan>)`   关闭管道
* `for <val> := range <chan>`  不断的从管道中读取数据
* `_, <bool> := <- <chan>`  判断管道是否关闭
* `select ... case`  读取多个管道
    * 默认是阻塞的，使用`default`后不再阻塞
    * 当多个管道都准备好时，会随机选择一个执行

#### 示例
```go

for {
    select {
        case c <- x:
            ...
        case <- quit:
            ...
    }
}

```