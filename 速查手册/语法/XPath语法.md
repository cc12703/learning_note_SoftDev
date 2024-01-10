

# xpath语法

## 基本规则
* `/`  从当前节点选取直接子节点
* `//` 从当前节点选取子孙节点
* `.`  选取当前节点
* `..` 选取当前节点的父节点
* `@`  选取属性
* `*`  通配符


### 例子
* `/bookstore`    选择根元素bookstore
* `//book`        选择所有的book节点
* `//@lang`       选取名为lang的所有属性

* `//*`              选取所有节点 
* `/bookstore/*`     选取bookstore节点下的所有子节点
* `//title[@*]`      选取所有带有属性的title节点



## 运算符
* `|`  节点集合
* `=`  等于
* `!=` 不等于
* `or` 或
* `and` 与


### 例子
* `//title | //price` 选取所有的title和price节点

## 谓语
* `[ ... ]` 用于设置查找条件

### 例子
* `/bookstore/book[1]`    选取bookstore子节点的第一个book节点
* `/bookstore/book[last()]` 选取bookstore子节点的最后一个book节点
* `//title[@lang]`     选取所有拥有lang属性的title节点
* `//title[@lang='eng']`     选取所有lang属性值为eng的title节点
* `/bookstore/book[price>35.00]/title`  选取bookstore节点中book节点下的所有title节点, 并且price元素的值必须大于35.00


## 轴
* `child`  选取当前节点的所有子节点
* `parent` 选取当前节点的父节点
* `ancestor` 选取当前节点的所有祖先（父、祖父）
* `self` 选取当前节点
* `attribute` 选取当前节点的所有属性


### 例子
* `child::book`  选取当前节点的所有book子节点
* `child::text()`  选取当前节点的所有文本子节点
* `attribute::lang` 选取当前节点的lang属性
* `attribute::*`   选取当前节点的所有属性


## 函数
* `text()` 获取节点中的文本
* `contains(str1, str2)` str1中是否包含str2, 用于匹配属性多值
* `starts-with(str1, str2)`  str1是否以str2开头
* `ends-with(str1, str2)`    str1是否以str2结尾
* `not()`  获取条件不匹配的节点

### 例子
* `//li[contains(@class, "li")]` 选取li节点class属性中包含有li的节点