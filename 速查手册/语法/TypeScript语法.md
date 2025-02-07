

# TypeScript语法

[TOC]

## 基础

* ECMAScript是 JavaScript 的规范
	* 包括 ES5（ES2009），ES6（ES2015）多个版本
* TypeScript实现了ECMAScript，并加入了类型系统


### 要点
* 只使用`===`和`!==`来进行比较
* 只使用`let`和`const`来定义变量和常量
* 变量后使用`!`，表示类型推断排除null, undefined
* 属性，参数中使用`?`表示该项可选
* `...`用于将数组转换成逗号参数形式
* `??` 用于当表达式为null、undefined时，设置一个默认值
* `?.`  可选链，当左值存在时才访问属性，不然返回一个undefined
* 条件运算符`<cond> ? <val1> : <val2>`


#### 示例
```ts

const firstName = null
const useName = firstName ?? "Guest"
//output: Guest
```

### 解构
* `[var_1, var_2] = [ ... ]`                数组解构
* `[var_1, var_2, ...var_other] = [ ... ]`  数组解构剩余
* `[var_1, , ...var_other] = [ ... ]`       数组解构忽略

* `{var_1, var_2} = { ... }`           对象解构
* `{var_1, var_2, {var_3}} = { ... }`  嵌套对象解构

#### 示例
```ts
const [x, y] = [1, 2]
//output: 1, 2
const [x, y, ...others] = [1, 2, 3, 4]
//outputs: 1, 2, [3, 4]
const [x, , ...others] = [1, 2, 3, 4]
//outputs: 1, [3, 4]

const {x, y, width, height} = { x: 0, y: 10, width: 15, height: 20}
const {bar: {bas}} = { bar: { bas: 123} }
```


### 箭头函数
* `=>` 又称为 胖箭头函数、lambda函数
* 功能：不需要使用function, 可以捕获this, 可以捕获 arguments

#### 示例
```ts
const inc = (x) => x + 1

class Person {
    constructor(public age:number) {}
    growOld = () => {
        this.age++
    }
}
//使用
const person = new Person(1)
person.growOld()
```


### 类型注解
* `num: number`         数字类型
* `str: string`         字符串类型
* `isSucc: boolean`     布尔类型
* `array: boolean[]`    数组类型
* `nameNumber: [string, number]`   元组类型
* `object: { [key:number] : string }`  对象类型
* `name: { first: string, second: string }` 内联方式
* `function reverse<T>(items: T[]): T[] { ... }`   泛型方式
* `command: string[] | string`   联合类型
* `type <name> = <type_1> | <type_2>`   类型别名

#### 示例
```ts
type StrOrNum = string | number
type Text = string | { text: string }
type Callback = (data: string) => void
```


### 语句

#### `for .. of`
* 用于遍历可迭代对象(Iterator)，包括：array, set, map, string
* 数组 `for (const item of <array>) { ... }`
* 映射 `for (const [key, value] of <map>) { ... }`

#### `for .. in`
* 用于遍历对象


### 浅拷贝

* `Object.assign( ... )`
* `{ ... }`

#### 示例
```ts
const obj = { foo: "foo", bar: "bar" }

const cp1 = Object.assign({}, obj)
const cp2 = { ...obj }
```

## 集合

### 概述
* Array: 有序，可重复，类型需要一样
* Tuple: 有序，可重复，类型可以不一样
* Set:   无序，无重复，类型需要一样


### Array
* `<array> = new Array(<len>)` 定义空数组
* `<array> = [<el_1>, <el_2>, <el_3>]` 定义数组
* `<array> = new Array(<el_1>, <el_2>, <el_3>)` 定义数组
* `<array> = Array.from(<iterable>)` 转成数组

* `<len> = <array>.length`  获取数组长度
* `<array>.length = 0`      清空数组
* `<len> = <array>.push(<el_1>, <el_2>)` 向尾部加入多个元素，并返回新的长度
* `<elm> = <array>.pop()`  从尾部移除元素，并返回该元素
* `<index = <array>.indexOf(elm)` 返回给定元素的首个索引，无返回-1
* `<index = <array>.lastIndexOf(elm)` 返回给定元素的最后一个索引，无返回-1
* `<bool> = <array>.includes(elm)`  判断给定元素是否在数组中

* `<array> = <array>.concat(<array>, <array>)` 合并数组，返回一个新数组
* `<del-array> = <array>.splice(startIndex, count)` 从给定索引开始删除元素，直接修改原数组

* `<elm> = <array>.find(elm => <cond>)` 查找元素，返回第一个匹配的元素
* `<index> = <array>.findIndex(elm => <cond>)` 查找元素，返回第一个匹配元素的索引值，无返回-1

* `<array>.forEach(elm => <oper>)`          遍历元素
* `<array>.forEach((elm, index) => <oper>)` 遍历元素
* `for(const val of <array>) { ... }`       遍历元素

* `<bool> = <array>.every(elm => <cond>)` 测试是否所有元素都符合条件
* `<bool> = <array>.some(elm => <cond>)` 测试是否有元素符合条件
* `<array> = <array>.filter(elm => <cond>)` 过滤元素，并返回一个符合条件的新数组
* `<array> = <array>.map(elm => <oper>)` 处理每个元素，并返回一个新数组
* `<array> = <array>.map((elm, index) => <oper>)` 处理每个元素，并返回一个新数组
* `<array> = <array>.flatMap(elm => <oper>)` 处理每个元素，将oper返回的嵌套数组扁平化
* `<val> = <array>.reduce((result, elm) => <oper>, <init>)` 将数组合并为一个值

* `<str> = <array>.join()` 使用逗号连接元素组成一个字符串
* `<str> = <array>.join(sep)` 使用分隔符连接元素组成一个字符串

* `<array>.sort()`  按字母顺序进行排序
* `<array>.sort((a, b) => <oper>)`  按比值函数的值进行排序，返回值越小排在越前面
* `<array>.reverse()` 反转数组中的元素

#### 示例
```ts 
const words = ['spray', 'limit', 'elite', 'exuberant', 'destruction']

words.splice(0, 2)
//output: ['spray', 'limit']
//origin: ['elite', 'exuberant', 'destruction']

word.every(word => word.startsWith('s'))
//output: false

words.some(word => word.startsWith('s'))
//output: true

words.filter(word => word.length > 6)
//output: ["exuberant", "destruction"]

words.map(word => word + "_ext")
//output: ['spray_ext', 'limit_ext', 'elite_ext', 'exuberant_ext', 'destruction_ext']

words.flatMap(word => [work, word + "_ext"])
//output: ['spray', 'limit', 'elite', 'exuberant', 'destruction', 'spray_ext', 'limit_ext', 'elite_ext', 'exuberant_ext', 'destruction_ext']

words.reduce((result, word) => result + "_" + word, "")
//output: 'spray_limit_elite_exuberant_destruction'

for(const word of words) {
  console.log(word)
}
//outputs: spray \n limit \n elite \n exuberant \n destruction


[40, 100, 1, 5, 25, 10].sort((a, b) => a -b)
//output: [1,5,10,25,40,100]
```


### Map
* `<map> = new Map()` 或 `<map> = {}`创建一个空映射
* `<map> = new Map([[key_1, val_1], [key_2, val_2]])` 创建一个映射

* `<map> = new Map(Object.entries(<obj>))` 从狭义对象中生成映射
* `<obj> = Object.fromEntries(<map>)`      映射转换成对象

* `size = <map>.size` 获取映射大小，键值对的个数
* `<map> = <map>.set(<ey, val)` 增加、更新元素
* `val = <map>.get(<key>)` 获取元素
* `<bool> = <map>.has(key)` 判断元素是否存在
* `<bool> = <map>.deletee(key)`  删除元素
* `<map>.clear()` 清除所有元素

* `<iter> = <map>.keys()` 返回一个包含键的遍历器
* `<iter> = <map>.values()` 返回一个包含值的遍历器
* `<iter> = <map>.entries()` 返回一个包含键值对的遍历器
* `<map>.forEach(val => <oper>)` 遍历元素
* `<map>.forEach((val, key) => <oper>)` 遍历元素，使用键值对

* `Array.from(<map>).map(([key, val] => { ... })` 按列表进行处理

#### 示例
```ts
for (const key of myMap.keys()) {
  console.log(key)
}

for (const[key, value] of myMap.entries()) {
  console.log(key + ' = ' + value)
}

myMap.forEach((value, key) => {
  console.log(key + ' = ' + value)
})
```


### Set
* `<set> = new Set()`            创建一个空集合
* `<set> = new Set([1,2,3,4])`   创建一个集合

* `<size> = <set>.size`   获取集合大小
* `<set> = <set>.add(<elm>)`  加入一个元素
* `<bool> = <set>.has(<elm>)` 判断元素是否存在
* `<bool> = <set>.delete(<elm>)` 移除指定元素
* `<set>.clear()` 清除所有元素

* `<set>.forEach(<elm> => { ... })` 遍历集合
* `<set>.forEach((<elm>,<index>) => { ... })` 遍历集合


### Tuple
* `var: [<type_1>, <type_2>] = [val_1, val_2]` 创建元组

#### 示例
```ts
let x: [string, number] = ['Hello', 10]
```




## 数据类型

* 字符串(string)，数字(number)，布尔(boolean)，符号(symbol)
* 对空(null)，未定义(undefined), 任意值(any)，空值(void)
* 对象(object)，数组(array)，元组(tuple), 枚举(enum)，函数(function)

### 数字
* 都是小数，内部使用64位浮点数

### 字符串 
* 内部用Unicode存储
* 使用单引号(')、双引号(")、反引号(`)进行定义
* `${<var>}` 模板格式化
* `num = <str>.length` 获取长度
* `<str> = <str>.toLowerCase()` 小写化
* `<str> = <str>.toUpperCase()` 大写化
* `<str> = <str>.trim()`        去除两边的空格
* `<sub_str> = <str>.substring(start_index, end_index)` 返回一个子字符串，不包含end_index处的字符
* `<sub_str> = <str>.substring(start_index)` 返回一个子字符串

* `index = <str>.indexOf(<sub_str>, [from_index])`  搜索字符串首次出现的地方,（-1表示找不到）
* `index = <str>.lastIndexOf(<sub_str>, [from_index])`  搜索字符串最后出现的地方,（-1表示找不到）

* `<array> = <str>.split()`  使用空格拆分字符串
* `<array> = <str>.split(sep)` 使用指定字符拆分字符串

* `<bool> = <str>.startsWith(<sub_str>)` 字符串头部是否包含指定字符
* `<bool> = <str>.endsWith(<sub_str>)` 字符串尾部是否包含指定字符

* `<str> = <str>.replace(<old_str>, <new_str>)` 使用新字符串替换老字符串首次出现的地方
* `<str> = <str>.replaceAll(<old_str>, <new_str>)` 使用新字符串替换老字符串每次出现的地方
* `<str> = <str>.replace(<regex>, <new_str>)` 使用新字符串替换匹配正则表达式的每个地方
* `index = <str>.search(<regex>)` 搜索正则表达式，（-1表示找不到）

#### 示例
```ts
const str = 'Mozilla Over End'

str.substring(1,3)
//output: 'oz'

str.substring(2)
//output: 'zilla Over End'

str.split(' ')
//output: ['Mozilla', 'Over', 'End']


let name: string = "bob"
`Hello, my name is ${ name }`
//output: Hello, my name is bob
```

### 枚举
* `enum <name> { <prop> = 0 }` 数字枚举 
* `enum <name> { <prop> = "<str>" }` 字符串枚举
* `const enum <name> { <prop> }`  常量枚举
* 使用namespace来增加静态方法


#### 示例
```ts
enum Direction {
    Up = 1,
    Down,
    Left,
    Right
}

enum Direction {
    Up = "UP",
    Down = "DOWN",
    Left = "LEFT",
    Right = "RIGHT",
}

namespace Direction {

    function toString() {
        .....
    }

}
```


### 狭义对象
* `<obj> = { <field-name>: <field-value>, <field-name>: <field-value>}` 创建对象
* `<var> = Object.keys(<obj>)` 获取所有属性
* `<obj>[<attr-name>] = <attr-value>` 增加、修改属性
* `<bool> = <obj>.hasOwnProperty(name)` 判断自有属性是否存在
* `<bool> = name in <obj> ` 判断属性是否存在（自有和继承都算）


### 函数
* `<var> = function (<arg>, <arg>) { }`  创建函数


### 任意值
* 屏蔽编译器的类型检查
* 与Object的区别：仍然可以调用对象上的方法

#### 示例
```ts
let notSure: any = 4
notSure.ifItExists()
notSure.toFixed()
```

### 空值
* 表示没有任何类型
* 用于表示函数没有返回值

#### 示例
```ts
function warnUser(): void {
    alert("This is my warning message")
}
```

### 类型判断
* `<string> = typeof <variable>`  判断变量类型
* `<bool> = <variable> instanceof <class>` 判断方法或接口类型

### 类型转换
* `let text: string = String(xxx)` 转换成String类型


## 组织代码

### 类
* `class`       定义类
* `extends`     继承类
* `static`      定义静态成员和函数
* `abstract`    定义抽象类和函数
* `constructor` 定义成员变量
* 成员和函数默认为 public

#### 示例
```ts
class Point {

  static instances = 0

  x: number
  y: number
  members = []
    
  constructor(x: number, y: number) {
    this.x = x
    this.y = y
    Point.instances++
  }
}

class Point3D extends Point {
  z: number
  
  constructor(x: number, y: number, z: number) {
    super(x, y)
    this.z = z
  }
}
```


### 接口
* `interface` 定义接口
* 接口无法转换成javaScript
* `readonly` 定义只读字段
* `<name>?`  定义可选字段


#### 示例
```ts
interface Person {
  name?: string,
  readonly age: number,
  run?(): void,
}

const why: Person = {
  // name 键值对缺失不会报错
  age: 18
}

why.run&&why.run() //判断函数执行

```


### 模块

#### 概述
* 每个文件是一个模块，有一个局部的作用域
* 使用export进行命名导出，可以有多个
* 使用export default进行默认导出，只能有一个
* 模块依赖关系的建立是静态的，发生在代码编译阶段


#### 全局声明
* `declare var name: <type>`   声明全局变量
* `declare function name(<param>): <type>`  声明全局函数
* `declare global { ... }`     扩展已存在的全局变量
* `declare namespace name { ... }`  声明全局命名空间

##### 示例
```ts
declare var foo: number
declare function greet(greeting: string): void

declare global {
  interface String {
      hump(input: string): string
  }
}

declare namespace MyPlugin {
    var n: number
    var s: string
    var f: (s:string) => number
}

//使用
MyPlugin.s.substr(0,1)
MyPlugin.n.toFixed()
MyPlugin.f('文字').toFixed()

```


#### 导出
* `export <type> name`  直接导出
* `export { symbol, symbol }` 间接导出
* `export { symbol as symbol }` 重命名导出
* `export default ...` 直接导出默认值
* `export default symbol` 间接导出默认值

##### 示例
```ts
export const someVar = 123
export type someType = {
    foo: string
}

const someVar = 123
type someType = {
    type: string
}
export { someVar, someType }
export { someVar as aDifferentName }



export default function foo(): string

declare enum Directions {
    Up,
    Down,
    Left,
    Right
}
export default Directions
```


#### 导入
* `import { symbol, symbol } from path`   部分导入
* `import { symbol as symbol } from path` 部分重命名导入
* `import * as symbol from path` 整体重命名导入

##### 示例
```ts
import { someVar, someType } from './foo'
import { someVar as aDifferentName } from './foo'

//加载到指定对象上， foo.someVar, foo.someType
import * as foo from './foo'
```


## 其他

### 异步

#### Promise
* `pObj = new Promise((reslove, reject) => { ... })`  创建一个Promise对象
* `pObj = Promise.reslove(result)` 创建一个成功的Promise对象 
* `pObj = Promise.reject(error)` 创建一个出错的Promise对象 
* `pObj = pObj.then(result => { ... })`  订阅成功状态
* `pObj = pObj.catch(error => { ... })`   订阅错误状态
* `pObj = Promise.all([pObj_1, pObj_2])`  并行运行多个Promise

##### 示例
```ts
new Promise((resolve,reject) => {
        fs.readFile(filename, (err,result) => {
            if (err) reject(err)
            else resolve(result)
        })
    })

Promise.resolve(123)
    .then((res)=>{
         return iReturnPromiseAfter1Second()
    })
    .then((res) => {
        console.log(res)
    })
```

#### Async/Await
* async用于标记异步函数
* await用于暂停执行，直到异步函数返回的Promise对象执行完成

##### 示例
```ts
async function foo() {
    try {
        const val = await getMeAPromise();
        console.log(val);
    }
    catch(err) {
        console.log('Error: ', err.message);
    }
}
```



### 二进制数据
* `Blob`：浏览器上支持文件操作的二进制对象
* `Buffer`：node.js上的二进制缓冲区
* `ArrayBuffer`：浏览器上通用的二进制缓冲区，不能直接读写
* `TypeArray`：用于操作ArrayBuffer的一种视图，使用数组方式操作
  * 包括：Int8Array, Int16Array, Int32Array, Float32Array
* `DataView`：用于读写ArrayBuffer的一种视图


#### 图示
![](http://picbed.cc12703.com/20231020100447.png)


#### ArrayBuffer
* `obj = new ArrayBuffer(len)`  创建一个对象
* `len = <arrayBuf>.byteLength`  获取大小
* `<arrayBuf> = <arrayBuf>.slice(begin, [end])`  获取子数据

#### TypeArray
* `obj = <tArray>.from([val_1, val_2, ...])` 从数组中创建对象
* `obj = <tArray>.of(val_1, val_2, ...)` 从值中创建对象


### 正则表达式
* `/pattern/modifiers` 定义表达式
  * pattern：模式
  * modifiers：修饰符
    * i 执行对大小写不敏感的匹配
    * g 执行全局匹配
    * m 执行多行匹 
* `<str>.search(<reg>)` 在字符串中搜索指定的子字符串
* `<str>.replace(<reg>, <str>)` 搜索指定字符串并替换
* `/pattern/.test(<str>)` 检测一个字符串是否匹配某个模式
* `<array> = /pattern/.exec(<str>)` 对字符串执行匹配