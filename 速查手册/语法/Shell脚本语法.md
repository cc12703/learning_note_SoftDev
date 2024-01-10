
# Shell脚本语法

[TOC]


## 基础

* `#!/bin/sh` 指定解释器
* `#xxxxx`    单行注释
* `:<< <char> ..... <char>` 多行注释
* 反引号可以捕获命令输出，赋值给变量
* `[ expr ]` 或 `[[ expr ]]`  定义条件判断
* `;` 用于在单行语句中区分代码块

### 示例
```sh

# 注释内容...

:<<!
注释内容...
注释内容...
注释内容...
!

if [ <cond> ]; then <statement> ; fi

```

## 变量

* `var=value` 定义变量
* `local var` 定义成局部变量（只能用于函数体内）
* `readonly var` 定义成只读变量

* `${var}`  获取变量值
* `${var-defualt}` 若变量未被声明，则返回default
* `${var:-defualt}` 若变量未被声明或值为空，则返回default

* `unset val`  删除变量


### 示例
```sh

myUrl="https://www.google.com"
readonly myUrl
echo ${myUrl}


result=${a-'local'}
echo ${result}
# local

a=""
result=${a:-'local'}
echo ${result}
# local


```

## 数组

* `array=(val1 val2 val3 ... valN)` 定义
* `${array[index]}`  读取元素
* `array[*]` 或 `array[@]` 读取所有元素
* `#array[*]` 或 `#array[@]` 获取长度


## 命令行

* `$0` 执行的文件名
* `$1` 第一个参数
* `$2` 第二个参数
* `$#` 参数总数
* `$*` 将参数组成字符串
* `$@` 将参数组成数组
* `$?` 显示最后命令的退出状态


## 字符串

* `str='xxxx'` 定义
	* 无法使用变量
	* 无法使用转义符
* `str="xxxx"` 定义
	* 可以使用变量
	* 可以使用转义符
* `${str/sub/repl}` 将sub字符串替换成repl字符串，只替换一次
* `${str//sub/repl}` 将sub字符串替换成repl字符串，替换所有
* `${#str}`       获取长度
* `${str:pos:len}`  获取子串

### 示例
```sh
string="runoob is a great site"
${string:1:4} 
# 输出 unoo
```

## 运算符

### 算术
* `expr var1 + var2`   加法
* `expr var1 - var2`   减法
* `expr var1 \* var2`  乘法
* `expr var1 / var2`   除法
* `expr var1 % var2`   取余

#### 示例
```sh
a=10
b=20

val=`expr $a + $b`
# 30

val=`expr $a - $b`
# -10

val=`expr $a \* $b`
# 200

val=`expr $b / $a`
# 2

val=`expr $b % $a`
# 0
```


### 关系
* `[ var1 -eq var2 ]`  是否相等
* `[ var1 -ne var2 ]`  是否不相等
* `[ var1 -gt var2 ]`  是否大于
* `[ var1 -lt var2 ]`  是否小于
* `[ var1 -ge var2 ]`  是否大于等于
* `[ var1 -le var2 ]`  是否小于等于


### 逻辑
* `[ -n expr1 ]`  逻辑非
* `[[ expr1 && expr2 ]]` 逻辑与
* `[[ expr1 ]] && [[ expr2 ]]` 逻辑与
* `[ expr1 -a expr2 ]`  逻辑与
* `[[ expr1 || expr2 ]]` 逻辑或
* `[[ expr1 ]] || [[ expr2 ]]` 逻辑或
* `[ expr1 -o expr2 ]`  逻辑或


### 字符串
* `[ str1 = str2 ]`   是否相等
* `[ str1 != str2 ]`  是否不相等
* `[ -z str ]`        长度是否为0
* `[ -n str ]`        长度是否不为0
* `[ $str ]`          是否为空字符串


### 文件测试
* `[ -d file ]`    是否是目录
* `[ -f file ]`    是否是普通文件
* `[ -w file ]`    文件是否可写
* `[ -x file ]`    文件是否可执行
* `[ -e file ]`    是否存在（文件、目录）
* `[ -s file ]`    文件是否为空（大小为0）



## 重定向

* `command > file`  输出重定向到文件
* `command >> file` 输出重定向到文件，追加方式
* `command < file`  输入重定向到文件（从文件中获取输入）
* `n >& m`    将输出合并
* `n <& m`    将输入合并

### 标准文件
* 标准输入： stdin, 描述符为0
* 标准输出： stdout，描述符为1
* 标准错误： stderr, 描述符为2
* 空设备： /dev/null (内容会被丢弃)



## 流程控制


### if
```sh
if condition
then
    command
elif condition
then
    command
else
    command
fi
```

#### 示例
```sh
a=10
* 标准输出： stdout，描述符为1
* 标准错误： stderr, 描述符为2
b=20
if [ $a == $b ]; then
   echo "a 等于 b"
elif [ $a -gt $b ]; then
   echo "a 大于 b"
elif [ $a -lt $b ]; then
   echo "a 小于 b"
else
   echo "没有符合的条件"
fi
```


### case
```sh
case value in
	pattern )
		command
		;;
	pattern )
		command
		;;
	* )    # 无匹配时
		command
		;;
esac
```


### for
```sh
for var in item1 item2 ... itemN
do
	command
done
```

#### 示例
```sh
for loop in 1 2 3 4 5
do
    echo "The value is: $loop"
done
```

### while
```sh
while  condition 
 do
    command
 done
```


## 函数

* 格式：`<function-name>() { .... }`
* 返回值：`return <int>`
* 参数获取：使用`$n`，
	* `$1` 获取第一个参数值
	* `${10}` 获取第十个参数值
* 参数传入：`<function-name> param1 param2 parm3 ...`

### 示例
```sh
funcWithParam() {
	echo "first param $1"
	echo "second param $2"
}

funWithParam 1 2
# first param 1
# second param 2
```
