


# WDA用法



## Predicate定位

### 比较
* `=`, `==`  等于
* `!=`, `<>`  不等于
* `>` 大于
* `<` 小于


### 集合
* `IN`  是否在集合中
* `BETWEEN` 是否在范围中
* `ALL`  满足表达式的所有元素
* `NONE` 不满足表达式的任意元素
* `array[index]` 数组指定索引的元素
* `array[FIRST]` 数组第一个元素
* `array[LAST]`  数组最后一个元素

#### 例子
* `name IN { ‘Ben’, ‘Melissa’, ‘Nick’ }`
* `1 BETWEEN { 0 , 33 }`
* `ALL children.age < 18`


### 逻辑
* `AND`, `&&` 逻辑与
* `OR`, `||`  逻辑或
* `NOT`，`!`  逻辑非


### 字符串
* `BEGINSWITH` 开头匹配
* `ENDSWITH` 结尾匹配
* `CONTAINS` 包含匹配   
* `LIKE`     通配符匹配
    * `?` 匹配一个字符
    * `*` 匹配0个或多个字符
* `MATCHES`  正则匹配

#### 例子
* `name BEGINSWITH "屏幕"`
* `name LIKE '*Total: $*'`