
# Java语法



## 基础

### lambda表达式
* `( parameters ) -> expression`
* `( parameters ) -> { statements; }`


### 方法引用
* `Class::new`     构造器，无参数
* `Class<T>::new`  构造器，无参数
* `Class::static_method` 静态方法，一个参数
* `Class::method`   类的任意对象方法，无参数
* `Instance::method`  对象方法，一个参数


## 数据类型

### Optional类
一个容器，用于解决空指针异常
* `get()` 获取值，会抛出`NoSuchElementException`
* `isPresent()` 若值存在，返回true
* `orElse(<other>)` 若值存在，返回值。不存在，返回other


## Stream

### 生成操作
* `Stream.of(<list>)` 生成流
* `<list>.stream()`   生成流


### 中间操作
* `<s>.filter((<elm>) -> <exp>)` 过滤，保留返回true的元素
* `<s>.filterNot((<elm>) -> <exp>)` 反向过滤，保留返回false的元素
* `<s>.map((<elm>) -> <exp>)`    映射
* `<s>.flatMap((<elm>) -> <exp>)` 扁平映射
* `<s>.sorted((<elm1>, <elm2>) -> <exp>)` 排序

* `<s>.limit(<num>)`  获取前N个元素
* `<s>.skip(<num>)`   跳过前N个元素
* `<s>.distinct()`    去重
* `<s>.withoutNulls()` 过滤掉null元素

### 最终操作
* `<s>.forEach(<elm> -> <exp> )` 遍历元素
* `<elm> = <s>.findFirst()` 获取第一个元素
* `<val> = <s>.count()` 获取总个数
* `<val> = <s>.max()` 获取最大值
* `<val> = <s>.min()` 获取最小值

* `<val> = <s>.reduce(<ival>, (<elm1>,<elm2>) -> <exp>)` 规约操作

* `<bool> = <s>.allMatch(<elm> -> <exp>)` 所有元素都符合条件，返回true
* `<bool> = <s>.anyMatch(<elm> -> <exp>)` 任何一个元素符合条件，返回true
* `<bool> = <s>.noneMatch(<elm> -> <exp>)` 没有元素符合条件，返回true


* `<s>.collect(Collectors.toList())` 收集元素，生成列表
* `<s>.collect(Collectors.toSet())`  收集元素，生成集合