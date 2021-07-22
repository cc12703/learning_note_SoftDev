
# SQL语法

[TOC]

## 基础
* SQL对大小写不敏感
* SQL分为DML(数据操作语言) 和 DDL(数据定义语言)
* SQL使用单引号来引用文本值
* 视图：基于SQL语句的结果集的可视化表

## 数据操作

### SELECT
从表中读取记录，并将结果存储在一个结果表中

* 读取所有列 `SELECT * FROM <table>`  
* 读取特定列 `SELECT <col1>,<col2> FROM <table>` 
* 读取特定列的唯一不同值 `SELECT DISTINCT <col> FROM <table>`
* 带条件的读取 `SELECT * FROM <table> WHERE <condition>`
* 对读取结果进行排序 `SELECT * FROM <table> ORDER BY <col>,<col> [DESC]`
	* 默认为升序
* 读取指定数量的记录 `SELECT TOP <num>|<percent> * FROM <table>`
	* `<num>` 记录数量
	* `<percent>` 记录数量的百分比
	* MySQL语法: `SELECT * FROM <table> LIMIT <num>`
* 给表指定别名 `SELECT * FROM <table> AS <alias>`
* 给列指定别名 `SELECT <col> as <alias> FROM <table>`
* 拷贝记录到另一个表 `SELECT * INTO <new-table> FROM <old-table>`
* 拷贝指定列到另一个表 `SELECT <col1>, <col2> INTO <new-table> FROM <old-table>`
* 拷贝记录到另一个数据库中的表 `SELECT * INTO <table> IN <database> `


#### 示例
```sql
SELECT LastName,FirstName FROM Persons
SELECT * FROM Persons

SELECT DISTINCT Company FROM Orders 


SELECT Company, OrderNumber FROM Orders ORDER BY Company
SELECT Company, OrderNumber FROM Orders ORDER BY Company, OrderNumber

SELECT TOP 2 * FROM Persons             //获取头2条记录
SELECT TOP 50 PERCENT * FROM Persons    //获取头百分之50的记录

SELECT po.OrderID, p.LastName, p.FirstName FROM Persons AS p, Product_Orders AS po 
	WHERE p.LastName='Adams' AND p.FirstName='John'
```


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


### WHERE子句
用于给SELECT、UPDATE、DELETE操作指定条件

* 列等于某值 `... WHERE <col> = <value>`
* 列不等于某值 `... WHERE <col> <> <value>`
* 列大于等于某值 `... WHERE <col> >= <value>`
* 列小于等于某值 `... WHERE <col> <= <value>`
* 列符合某个模式 `... WHERE <col> LIKE <pattern>`
	* `<pattern>`中需要使用通配符
* 列在某个值中 `... WHERE <col> IN (<val1>, <val2>)`
* 列在两个值的范围内 `... WHERE <col> BETWEEN <val1> AND <val2>`
	* 值可以是数值、文本、日期

#### 通配符
* `%`  替换零个或多个字符
* `_`  替换一个字符
* `[charlist]`  替换字符列中的任何单一字符
* `[^charlist] 或 [!charlist] 替换不在字符列中的任何单一字符`

#### 示例
```sql
SELECT * FROM Persons WHERE City = 'Beijing' 
SELECT * FROM Persons WHERE Year > 

SELECT * FROM Persons WHERE City LIKE 'Ne%'
SELECT * FROM Persons WHERE City LIKE '%lond%'
SELECT * FROM Persons WHERE City LIKE '[ALN]%'

SELECT * FROM Persons WHERE LastName IN ('Adams','Carter')
```


### JOIN
根据多个表中列之间的关系，从表中查询数据

* 内连接 `SELECT * FROM <table1> INNER JOIN <table2> ON <table1>.<col> = <table2>.<col>`
	* 从左表(table1)和右表(table2)中返回至少匹配一次的行
* 左连接 `SELECT * FROM <table1> LEFT JOIN <table2> on <table1>.<col> = <table2>.<col>`
	* 从左表(table1)中返回所有行，即使在右表(table2)中没有匹配的行
* 右连接 `SELECT * FROM <table1> RIGHT JOIN <table2> on <table1>.<col> = <table2>.<col>`
	* 从右表(table2)中返回所有行，即使在左表(table1)中没有匹配的行
* 全连接 `SELECT * FROM <table1> FULL JOIN <table2> on <table1>.<col> = <table2>.<col>`
	* 从左表(table1)和右表(table2)中返回所有的行

## 数据定义

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