
# SQL语法

[TOC]

## 基础
* 对大小写不敏感
* 使用单引号来引用文本值
* 视图：基于SQL语句的结果集的可视化表


### 注释
* `--`  单行注释
* `#`  单行注释
* `/* */` 多行注释


### 查询过程

* 查询分为：逻辑查询 和 物理查询
* 逻辑查询：表示查询应该产生什么样的结果
* 物理查询：表示如何得到结果，会根据索引来优化

#### 执行顺序
```
(8)  SELECT (9) DISTINCT <sel-list>
(1)  FROM <left-table>
(3)  <join-type> JOIN <right-table>
(2)        ON <join-cond>
(4)  WHERE <where-cond>
(5)  GROUP BY <group-list>
(6)  WITH ...
(7)  HAVING <having-cond>
(10) ORDER BY <order-list>
(11) LIMIT <limit-num> 

```

1. 对左表和右表执行笛卡儿积
2. 执行`ON过滤`
3. 添加外部行
4. 执行`WHERE过滤`
5. 分组操作
6. 执行`CUBE`或`ROLLUP`操作
7. 执行`HAVING过滤`
8. 选择指定列
9. 去除重复数据
10. 执行排序操作
11. 取出指定行的记录



## 查询数据库

### 检索数据
从表中读取记录，并将结果存储在一个结果表中

* `SELECT * FROM <table>`  读取所有列  
* `SELECT <col1>,<col2> FROM <table>`  读取特定列 
* `SELECT DISTINCT <col> FROM <table>` 读取不同值 
* `SELECT * FROM <table> WHERE <condition>` 带条件的读取 



* 拷贝记录到另一个数据库中的表 `SELECT * INTO <table> IN <database>`


#### 示例
```sql
SELECT LastName,FirstName FROM Persons
SELECT * FROM Persons

SELECT DISTINCT Company FROM Orders 



SELECT po.OrderID, p.LastName, p.FirstName FROM Persons AS p, Product_Orders AS po 
	WHERE p.LastName='Adams' AND p.FirstName='John'
```


### 别名
用于给表名称、列名称指定其他名字

* `SELECT * FROM <table> AS <alias>` 给表指定别名 
* `SELECT <col> as <alias> FROM <table>` 给列指定别名 

#### 示例
```sql
SELECT name AS n, country AS c FROM Websites;

SELECT w.name, w.url, a.count, a.date
	FROM Websites AS w, access_log AS a
	WHERE a.site_id=w.id and w.name="菜鸟教程";
```


### 限制数据
* `SELECT TOP <num>|<percent> * FROM <table>` 读取开始部分的数据（SQL-Server）
	* `<num>` 记录数量
	* `<percent>` 记录数量的百分比
* `SELECT * FROM <table> LIMIT <num> OFFSET <offset>` 读取特定部分的数据（MySQL）  
	* `<num>` 记录数量
	* `<offset>` 开始的记录（从0行开始）


#### 示例
```sql
SELECT TOP 2 * FROM Persons             //获取头2条记录
SELECT TOP 50 PERCENT * FROM Persons    //获取头百分之50的记录
```


### 排序数据
* `SELECT * FROM <table> ORDER BY <col>` 升序排序 
* `SELECT * FROM <table> ORDER BY <col> DESC` 降序排序
	* `DESC`只对前面的列名有效
* `SELECT * FROM <table> ORDER BY <col>,<col>` 按多个列进行排序



#### 示例
```sql
SELECT Company, OrderNumber FROM Orders ORDER BY Company
SELECT Company, OrderNumber FROM Orders ORDER BY Company DESC, OrderNumber DESC
```


### 过滤数据
用于给SELECT、UPDATE、DELETE操作指定条件

* `... WHERE <col> = <value>`  列等于某值 
* `... WHERE <col> <> <value>` 列不等于某值 
* `... WHERE <col> != <value>` 列不等于某值
* `... WHERE <col> >= <value>` 列大于等于某值
* `... WHERE <col> > <value>` 列大于某值
* `... WHERE <col> <= <value>` 列小于等于某值
* `... WHERE <col> < <value>` 列小于某值

* `... WHERE <col> IS NULL`  列是否是空值
	* 空值：不包含任何值

* `... WHERE <col> BETWEEN <val1> AND <val2>`  列在两个值的范围内
	* 值可以是数值、文本、日期
* `... WHERE <col> IN (<val1>, <val2>)` 列在多个值中 
* `... WHERE <col> LIKE <pattern>` 列符合某个模式 
	* `<pattern>`中需要使用通配符，只能用于文本字段
	
* `... WHERE <cond> AND <cond>` 多个条件的与
* `... WHERE <cond> OR <cond>` 多个条件的或
* `... WHERE NOT <cond>`        否定条件


#### 通配符
* `%`  匹配任何字符（不匹配NULL）
* `_`  匹配单个字符
* `[charlist]`  匹配字符集中的任何单一字符
* `[^charlist]`或`[!charlist]` 匹配不在字符集中的任何单一字符

#### 示例
```sql
SELECT * FROM Persons WHERE City = 'Beijing';
SELECT * FROM Persons WHERE City LIKE 'Ne%';
SELECT * FROM Persons WHERE City LIKE '%lond%';
SELECT * FROM Persons WHERE City LIKE '[ALN]%';

SELECT * FROM Persons WHERE LastName IN ('Adams','Carter');

SELECT * FROM Persons WHERE NOT City = 'Beijing';
```


### 处理数据

#### 文本处理
* `+`或`||`  拼接字符串（SQL-Server）
* `Concat()` 拼接字符串（MySQL）
* `RTRIM()`  去除右边的所有空格
* `LTRIM()`  去除左边的所有空格
* `TRIM()`   去除左右两ZI87边的所有空格
* `UPPER()`  转换为大写
* `LOWER()`  转换为小写
* `SUBSTR()` 提取字符串

#### 数值处理
* `+`   列值相加
* `-`   列值相减
* `*`   列值相乘
* `/`   列值相除
* `ABS()` 获取绝对值
* `SQRT()` 获取平方根


#### 示例
```sql
SELECT vendName + '(' + vendCountry ')' FROM Vendors;
SELECT Concat(vendName, '(', vendCountry, ')') FROM Vendors;
```


### 聚合数据
* `COUNT(*)` 返回行数
* `COUNT(<col>)` 返回特定列的行数，忽略NULL值
* `SUM(<col>)`   返回列值的总和,忽略NULL值
* `MIN(<col>)`   返回列的最小值,忽略NULL值
* `MAX(<col>)`   返回列的最大值,忽略NULL值
* `AVG(<col>)`   返回列的平均值,忽略NULL值
* `AVG(DISTINCT <col>)` 使用不同的值计算平均值


#### 示例
```sql
SELECT AVG(price) AS avg_price FROM Products;

SELECT MIN(price) AS price_min, 
	   MAX(price) AS price_max,
	   AVG(price) AS price_avg
FROM Products;

```


### 分组数据

* `SELECT <col> aggregate(<col>) FROM <table> GROUP BY <col>`  分组数据
* `SELECT ... FROM <table> GROUP BY ... HAVING <cond>`  过滤分组


#### 示例
```sql
SELECT site_id, SUM(access_log.count) AS nums
		FROM access_log GROUP BY site_id;

//查找总访问量大于200的网站
SELECT name, url, SUM(access_count) AS nums FROM Websites 
	GROUP BY name HAVING SUM(access_count) > 200;

//对输出进行排序
SELECT order_num, COUNT(*) AS items FROM OrderItems
	GROUP BY order_num HAVING COUNT(*) >= 3
	ORDER BY items, order_num;
```


### 子查询
* 含义：嵌套在其他查询中的查询

* `SELECT ... FROM <table> WHERE EXISTS(SELECT <col> FROM <table> WHERE ...)`
	* 用于判断查询子句是否有记录，有返回True，无返回False


#### 示例
```sql
SELECT name, url FROM Websites WHERE EXISTS
	(SELECT access_count FROM Websites WHERE access_count > 200)
```



### 关联数据
根据多个表中列之间的关系，从表中查询数据

* `SELECT * FROM <table1> INNER JOIN <table2> ON <table1>.<col> = <table2>.<col>`
	* 内连接：从左表(table1)和右表(table2)中返回至少匹配一次的行
* `SELECT * FROM <table1> LEFT JOIN <table2> on <table1>.<col> = <table2>.<col>`
	* 左连接：从左表(table1)中返回所有行，即使在右表(table2)中没有匹配的行
* `SELECT * FROM <table1> RIGHT JOIN <table2> on <table1>.<col> = <table2>.<col>`
	* 右连接：从右表(table2)中返回所有行，即使在左表(table1)中没有匹配的行
* `SELECT * FROM <table1> FULL JOIN <table2> on <table1>.<col> = <table2>.<col>`
	* 全连接：从左表(table1)和右表(table2)中返回所有的行




## 组合查询

* 在一个查询中，从不同的表返回一个结构数据
* 对一个表执行多次查询，统一返回数据

* `SELECT ... WHERE ... UNION SELECT ... WHERE ...`  组合两次查询的结构
* `SELECT ... WHERE ... UNION SELECT ... WHERE ... ORDER BY ...` 对结果进行排序




## 操作数据库

### INSERT
向表中插入新行

* `INSERT INTO <table> VALUES (<val1>,<val2>)` 插入完整行
* `INSERT INTO <table> (<col1>,<col2>) VALUES (<val1>,<val2>)` 插入行时指定列


#### 示例
```sql
INSERT INTO Persons VALUES ('Gates', 'Bill', 'Xuanwumen 10', 'Beijing')
INSERT INTO Persons (LastName, Address) VALUES ('Wilson', 'Champs-Elysees')
```


### UPDATE
修改表中的数据

* `UPDATE <table> SET <col> = <val> WHERE <col> = <val>`

#### 示例
```sql
UPDATE Person SET FirstName = 'Fred' WHERE LastName = 'Wilson' 
UPDATE Person SET Address = 'Zhongshan 23', City = 'Nanjing'
WHERE LastName = 'Wilson'
```



### DELETE
删除表中的行

* `DELETE FROM <table> WHERE <col> = <val>` 删除特定行
* `DELETE FROM <table>` 删除所有行
* `DELETE * FROM <table>` 删除所有行



## 定义数据库

### 创建操作
* 创建数据库 `CREATE DATABASE <name>`
* 创建表 `CREATE TABLE <name> (<col-name> <type> [<constraints>], ...)`
* 创建索引 `CREATE INDEX <name> ON <table> (<col1>, <col2>, ...)`
* 创建唯一索引 `CREATE UNIQUE INDEX <name> ON <table> (<col1>, <col2>, ...)`
* 创建视图 `CREATE VIEW <name> AS SELECT <col>, ... FROM <table> WHERE <condition>`

#### 示例
```sql
CREATE DATABASE my_db

CREATE TABLE Persons
(
Id_P int,
LastName varchar(255),
FirstName varchar(255),
Address varchar(255),
City varchar(255)
)

CREATE INDEX PersonIndex ON Person (LastName, FirstName)
CREATE INDEX PersonIndex ON Person (LastName DESC) 

CREATE VIEW [Current Product List] AS
	SELECT ProductID,ProductName FROM Products WHERE Discontinued=No
```

#### 数据类型
* 整数 `int(size)`
* 带小数的数字 `decimal(size, d)`
* 固定长度的字符串 `char(size)`
* 可变长度的字符串 `varchar(size)`
	* size 为最大长度

#### 约束
* 不接受NULL值 `NOT NULL`
* 唯一值 `UNIQUE`
* 主键 `PRIMARY KEY`
	* 非NULL的，唯一值的，只能有一个
* 外键 `FOREIGN KEY REFERENCES <table>(<col>)`
	* 指向另一个表的主键
* 插入默认值 `DEFAULT <val>`

#### 示例
```sql
CREATE TABLE Persons
(
Id_O int NOT NULL PRIMARY KEY,
Alias NOT NULL UNIQUE,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255) DEFAULT 'Sandnes',
Id_P int FOREIGN KEY REFERENCES Persons(Id_P),
)
```

### 删除操作
* 删除数据库 `DROP DATABASE <name>`
* 删除表 `DROP TABLE <name>`
* 删除索引 `DROP INDEX <name> ON <table>`

### 修改操作
* 向表中添加列 `ALTER TABLE <table> ADD <col> <type>`
* 从表中删除列 `ALTER TABLE <table> DROP COLUMN <col>`
* 改变表中列的类型 `ALTER TABLE <table> ALTER COLUMN <col> <type>`

#### 示例
```sql
ALTER TABLE Persons ADD Birthday date
ALTER TABLE Person DROP COLUMN Birthday
```