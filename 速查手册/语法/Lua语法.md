

# Lua语法


## 基础

### 概述
* `--`  单行注释
* `--[[ ... --]]`  多行注释


### 变量
* `<name> = <val>`  定义全局变量
* `<name> = nil`    删除全局变量
* `local <name> = <val>` 定义局部变量
* `<var1>, <var2> = <val1>, <val2>`  多变量赋值
* `<var1>, <var2> = <var2>, <var1>`  交换变量


### 操作符
* `==`  等于
* `~=`  不等于

### 迭代器
* `for ... in ipairs()`  只遍历值，按键升序，遇到nil中止
* `for ... in pairs()`  遍历所有元素


### 控制
* `if(<cond>) then ... end`   条件判断
* `if(<cond>) then ... else ... end`  条件判断
* `while(<cond>) do ... end`  条件满足时一直循环
* `for <var>=<start>,<stop>,<step> do ... end` 循环
* `repeat ... until(<cond>)` 循环直到条件被满足

## 类型

### 基础
* `nil` 空，值为 `nil`
* `boolean` 布尔, 值为 `true`，`false`
* `number` 数字，实际为 双精度的实浮点数
* `string` 字符串，使用单引导、双引导表示
* `function` 函数，
* `thread` 独立线程，用于执行协程
* `table` 表，实际为 关联数组

### 字符串
* `"xxx"`  定义可转义的字符串
* `'xxx'`  定义不可转义的字符串
* `string.upper(<str>)`  转大写
* `string.lower(<str>)`  转小写
* `string.reverse(<str>)` 反转
* `string.gsub(<str>, <sub-str>, <repl-str>)` 替换
* `string.find()`

### 表
* 键：数字（初始索引为1）、字符串
* `<var> = {}`  创建空表
* `<var> = {xxx, xxx, xxx}` 创建表
* `<var> = <table>[<key>]`  访问表
* `<var> = <table>.<key>`   访问表
* `<table>[<key>] = <val>`  修改表
* `<table>.<key> = <val>`   修改表
* `<var> = table.concat(<table>, <sep>)` 连接表中元素
* `table.insert(<table>, <elm>)` 向尾部插入元素
* `table.insert(<table>, <pos>, <elm>)` 向指定位置插入元素
* `table.remove(<table>)` 移除尾部元素
* `table.remove(<table>, <pos>)` 移除置顶位置元素
* `table.sort(<table>)` 排序


### 函数
* `function <name>(<arg>) ... end`  定义全局函数
* `local function <name>(<arg>) ... end`  定义局部函数
* `function <name>(...)` 定义变长参数
* `local <var> = {...}`  读取变长参数