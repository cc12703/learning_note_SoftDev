

# Lua语法


## 基础

### 概述
* `--`  单行注释
* `--[[ ... --]]`  多行注释


### 定义
* `<name> = <val>`  定义全局变量
* `local <name> = <val>` 定义局部变量
* `<var1>, <var2> = <val1>, <val2>`  多变量赋值



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