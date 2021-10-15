
# Kotlin语法

[TOC]

## 基础

### 概述
* 一切都是对象
* `var` 定义可变的变量
* `val` 定义不可变的变量
* `===` 判断引用相等
* `==`  判断内容相等，使用`equals()`检测
* `if (<condition>) <result> else <result>` 三元表达式


### 函数

#### 参数
* 使用`name:type`格式定义
* 可以定义默认值
* 调用传参时，可以指定参数名
* 若最后一个参数是lambda，则可以在括号外传入
* 可变数量参数，用`vararg`标记

##### 示例
```kotlin
fun foo(bar: Int, baz:Int = 1, qux: () -> Unit) { ... }

foo(1) { println("hello") }
foot(bar = 1, baz = 2, qux = { println("hello") })
```

#### 类型
```kotlin
//高阶函数：以其他函数作为参数、返回值的函数
fun foo(x: Int) {
	//局部函数：在一个函数内部定义的函数
	fun double(y: Int): Int {
		return y * 2
	}
	println(double(x))
}

//扩展函数：在不修改已有类的前提下，给其增加新方法
fun View.invisible() {
	this.visibility = View.INVISIBLE
}

//单表达式函数，在=符号后定义代码体
fun double(x: Int): Int = x * 2

//中缀表达函数
infix fun Int.shl(x: Int): Int { ... }

1 shl 2 
1.shl(2)


//内嵌函数
inline fun idouble(s: Int): Int { ... }
```



## 类型


### 数字
* 整型：`Byte`（8位）, `Short`（16位）, `Int`（32位）, `Long`（64位）
* 浮点：`Float`, `Double`
* 无符号整型：`UByte`, `UShort`, `UInt`, `ULong`

#### 定义
* `123`         十进制
* `0x0F`        十六进制
* `0b00001011`  二进制
* `123L`        Long型
* `125.5F`      Float型
* `123u`        无符号类型
* `123ul`       无符号长整型


### 字符串
* `"xxxx"`    定义可转义字符串
* `"""xxx"""` 定义原生字符串，不会进行转义处理
* `${xxx}`    模板求值

#### 示例
```kotlin
println("Hello, $name")
println("Hello, ${if (args.size > 0) args[0] else "someone"}!")

val html = """
	<html>
		<body>
			<p>Hello Word</p>
		</body>
	</html>
"""
```

### 数组
* `arrayOf(<val_1>, <val_2>,...)` 创建Array类型数组
* `arrayOfNulls(<size>)`  创建Array类型的空数组
* `intArrayOf(<val_1>, <val_2>, ...)` 创建IntArray原生类型数组



## 集合

### 概述
* 分成 可变集合(Mutable) 和 不可变集合(Immutable)
* 类型有：list, set, map
* 可以使用序列进行惰性求值


### 类层次
```dot
digraph G {
	Iterable -> MutableIterable
	Iterable -> Collection
	MutableIterable -> MutableCollection
	Collection -> MutableCollection

	Collection -> List
	Collection -> Set

	List -> MutableList
	MutableCollection -> MutableList

	Set -> MutableSet
	MutableCollection -> MutableSet

	Map -> MutableMap
}
```


### List

#### 创建
* 创建不可变空列表 `<list> = emptyList()`
* 创建不可变列表 `<list> = listOf('a', 'b', 'c')`
* 创建可变空列表 `<list> = mutableListOf<Type>()`
* 创建可变列表 `<list> = mutableListOf(1, 2, 3)`

#### 获取元素
* 获取指定位置元素 `<elm> = <list>.get(index)` or `<elm> = <list>[index]`
* 获取第一个元素 `<elm> = <list>.first()`
* 获取最后一个元素 `<elm> = <list>.last()`
* 获取首个满足条件的元素 `<elm> = <list>.first { it -> <cond> }`
* 获取最后一个满足条件的元素 `<elm> = <list>.last { it -> <cond> }`

* 使用区间获取元素 `<list> = <list>.slice(<range>)`
* 从头获取指定数量的元素 `<list> = <list>.take(num)`
* 从头去除指定数据的元素 `<list> = <list>.drop(num)`
* 从头获取匹配条件的元素 `<list> = <list>.takeWhile { it -> <cond> }`
* 从尾获取指定数量的元素 `<list> = <list>.takeLast(num)`
* 从尾去除指定数据的元素 `<list> = <list>.dropLast(num)`
* 从尾获取匹配条件的元素 `<list> = <list>.takeLastWhile { it -> <cond> }`

#### 更新元素
* 向尾部插入元素 `<bool> = <list>.add(<elm>)`
* 向指定位置插入元素 `<bool> = <list>.add(index, <elm>)`
* 向尾部插入列表 `<bool> = <list>.add(<new-list>)`
* 向指定位置插入列表 `<bool> = <list>.add(index, <new-list>)`
* 向指定位置替换元素 `<list>.set(index, <elm>)` or `<list>[index] = <elm>`


#### 查找
* 从头线性查找，返回指定元素的索引，无返回 -1 `index = <list>.indexOf(<elm>)`
* 从尾线性查找，返回指定元素的索引，无返回 -1 `index = <list>.lastIndexOf(<elm>)`
* 从头线性查找，返回匹配元素的索引，无返回 -1 `index = <list>.indexOfFirst { elm -> <cond> }`
* 从尾线性查找，返回匹配元素的索引，无返回 -1 `index = <list>.indexOfLast { elm -> <cond> }`

#### 转换、过滤
* 映射 `<list> = <list>.map { elm -> <oper> }`
* 带索引的映射 `<list> = <list>.mapIndexed { idx, value -> <oper> }`
* 带null值过滤的映射 `<list> = <list>.mapNotNull { elm -> <oper> }`

* 肯定条件过滤 `<list> = <list>.filter { elm -> <cond> }`
* 否定条件过滤 `<list> = <list>.filterNot { elm -> <cond> }`
* 过滤给定类型 `<list> = <list>.filterIsInstance<Type>()`

#### 排序-返回新列表
* 自然升序排序 `<list> = <list>.sorted()`
* 自然降序排序 `<list> = <list>.sortedDescending()`
* 获取倒序 `<list> = <list>.reversed()`
* 随机排序 `<list> = <list>.shuffled()`
* 自定义排序 `<list> = <list>.sortedBy { elm -> <oper> }`

#### 排序-修改旧列表
* 自然升序排序 `<list>.sort()`
* 自然降序排序 `<list>.sortDescending()`
* 获取倒序 `<list>.reverse()`
* 随机排序 `<list>.shuffle()`
* 自定义排序 `<list>.sortBy { elm -> <oper> }`

#### 分组、分块
* 将列表分块 `<nest-list> = <list>.chunked(size)`
* 将列表分块并转换 `<list> = <list>.chunked(size) { elm -> <oper> }`
* 将元素按条件分组 `<map:key-list> = <list>.groupBy { elm -> <cond> }`

#### 扁平化
* 将嵌套列表合并成一个列表 `<list> = <nest-list>.flatten()`
* 先映射元素后进行合并 `<list> = <nest-list>.flatMap { elm -> <oper> }`
	* 等同于 `<list> = <nest-list>.map().flatten()`

#### 聚合
* 将操作应用于元素并累积 `result = <list>.fold(init, { acc, elm -> <oper> })`
	* 可以提供一个初始值
* 将操作应用于元素并累积 `result = <list>.reduct { acc, elm -> <oper> }`
	* 将第一个元素作为初始值，该元素不会被应用操作

#### 转换为字符串
* 将列表元素合并成一个字符串 `<str> = <list>.joinToString([separator], [prefix], [postfix])`
* 将列表元素合并加入`Appendable`对象中 `<list>.joinTo(<appendable>, [separator], [prefix], [postfix])`


##### 示例
```kotlin
val list = mutableListOf(1, 2, 3)
var result = list.map { it * 3 }
result = list.mapIndexed { idx, value -> value * idx }

result = list.mapNotNull { if ( it == 2) null else it * 3 }
result = list.mapIndexedNotNull { idx, value -> 
				if (idx == 0) null else value * idx 
			}


val numbers = (0..13).toList()
numbers.chunked(3)
//[[0, 1, 2], [3, 4, 5], [6, 7, 8], [9, 10, 11], [12, 13]]

numbers.chunked(3) { it.sum() }
//[3, 12, 21, 30, 25]

val numbers = listOf("one", "two", "three", "four", "five", "six")
numbers.first { it.length > 3 }

//three
numbers.last { it.startsWith("f") }
//five


val numbers = listOf("one", "two", "three", "four", "five")
numbers.groupBy { it.first().toUpperCase() }
//{O=[one], T=[two, three], F=[four, five]}

numbers.groupBy(keySelector = { it.first() }, valueTransform = { it.toUpperCase() })
//{o=[ONE], t=[TWO, THREE], f=[FOUR, FIVE]}


val numbers = listOf(5, 2, 10, 4)
numbers.fold(0) { sum, element -> sum + element * 2 }
//42


val numberSets = listOf(setOf(1, 2, 3), setOf(4, 5, 6), setOf(1, 2))
numberSets.flatten()
//[1, 2, 3, 4, 5, 6, 1, 2]


val numbers = listOf("one", "two", "three", "four")
numbers.joinToString()
//one, two, three, four

val numbers = listOf("one", "two", "three", "four")    
numbers.joinToString(separator = " | ", prefix = "start: ", postfix = ": end")
//start: one | two | three | four: end
```


### Map
* 


## 类型系统


## 面向对象

### 概述
* 类成员默认是全局可见的
* 类默认是final的，不可继承
* 构建对象时，不需要new关键字
* 一个文件中可以创建多个类


### 构造函数
* 主构造函数，如果没有定义任何构造函数，系统会生成一个空构造函数
	```kotlin
	class Person constructor(firstName: String) { /*...*/ }
	
	//若无任何标注和可见性修改器，则可以省略constructor
	class Person(firstName: String) { /*...*/ }

	//定义属性
	class Person(
		val firstName: String,
		val lastName: String,
		var age: Int, // trailing comma
	) { /*...*/ }

	//屏蔽掉公开的构造函数
	class DontCreateMe private constructor () { /*...*/ }
	```
* `init`关键字用于定义初始化代码
	```kotlin
	class InitOrderDemo(name: String) {
		val firstProperty = "First property: $name".also(::println)
		
		init {
			println("First initializer block that prints ${name}")
		}
		
		val secondProperty = "Second property: ${name.length}".also(::println)
		
		init {
			println("Second initializer block that prints ${name.length}")
		}
	}
	```
* 次构造函数
	```kotlin

	//需要constructor做前缀
	class Pet {
    	constructor(owner: Person) { }
	}

	//次构造函数需要委托给主构造函数
	class Person(val name: String) {
		constructor(name: String, parent: Person) : this(name) { }
	}
	```

### 继承
* 构造函数
```kotlin
open class Base(p: Int)
class Derived(p: Int) : Base(p)

//次构造函数，需要使用super
class MyView : View {
    constructor(ctx: Context) : super(ctx)
    constructor(ctx: Context, attrs: AttributeSet) : super(ctx, attrs)
}
```
* 覆盖方法
```kotlin
open class Shape {
    open fun draw() { /*……*/ }
    fun fill() { /*……*/ }
}

class Circle() : Shape() {
    override fun draw() { /*……*/ }
}
```
* 调用超类实现
```kotlin
open class Rectangle {
    open fun draw() { println("Drawing a rectangle") }
    val borderColor: String get() = "black"
}

class FilledRectangle : Rectangle() {
    override fun draw() {
        super.draw()
        println("Filling the rectangle")
    }

    val fillColor: String get() = super.borderColor
}
```

### 属性
* 声明语法
```
var <propertyName>[: <PropertyType>] [= <property_initializer>]
    [<getter>]
    [<setter>]
```
* 只读属性
```kotlin
val isEmpty: Boolean
    get() = this.size == 0
```
* 读写属性
```kotlin
var stringRepresentation: String
    get() = this.toString()
    set(value) {
        setDataFromString(value)
    }
```
* 延时初始化 `by lazy`
```kotlin
class Bird(val weight: Double = 0.00,
		   val age: Int = 0, val color: String = "blue") { 
    val sex: String by lazy { 
		if (color == "yellow") "male" else "female"
	}
}
```



## 协程

### 概述
* 协程总是运行在一些协程上下文中(CoroutineContext)
* 上下文包括：协程的Job对象，协程调度器(CoroutineDispatcher)
* 协程调度器用于确定协程可以运行的线程

### 基本操作
* `<Job> = launch { ... }` 启动一个顺序协程
* `<Job>.join()`           等待协程完成
* `<Job>.cancel()`         取消协程
* `<Job>.cancelAndJoin()`  取消协程并等待结束
* `<Deferred> = async { ... }`  启动一个并发协程
* `<Deferred>.await()`     等待并获取协程结果
* `delay(<int>)`           挂起一个协程
* `GlobalScope.launch { ... }` 启动一个全局协程


#### 示例
```kotlin
fun main() = runBlocking { // this: CoroutineScope 
    launch { // 启动一个新的协程，并继续运行
        delay(1000L) // 非阻塞的延时1秒
        println("World!") // 延时后打印 
    }
    println("Hello") // 当前一个协程被延迟时，主协程会继续运行
}

>>> Hello
>>> World!
```

### 作用域
* `runBlocking { ... }`     创建一个作用域，阻塞当前线程
* `coroutineScope { ... }`  创建一个作用域，其中的协程是并行运行

#### 示例
```kotlin
// 串行执行
fun main() = runBlocking {
    doWorld()
    println("Done")
}

// 并行执行
suspend fun doWorld() = coroutineScope { // this: CoroutineScope
    launch {
        delay(2000L)
        println("World 2")
    }
    launch {
        delay(1000L)
        println("World 1")
    }
    println("Hello")
}
```

### 异步流(Flow)