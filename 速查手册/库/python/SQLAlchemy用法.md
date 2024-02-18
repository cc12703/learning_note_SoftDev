


# SQLAlchemy用法


## 概述

### 链接
* [sqlalchemy](https://github.com/sqlalchemy/sqlalchemy)
* [中文文档](https://docs.sqlalchemy.org/en/20/orm/index.html)


## 定义表
* `Base = declarative_base()` 定义基类
* `__tablename__ = <name>`    定义表名
* `<name> = Column( ... )`      定义列名

### 列属性
* `name`  字段映射名字
* `primary_key` 是否为主键
* `unique` 是否唯一
* `index`  是否创建索引
* `nullable` 是否允许为空
* `default`  默认值
* `autoincrement`  是否自动增长
* `comment`     注释

### 数据类型
* `Interger` 整型
* `SmallInteger` 小整型
* `BigInteger`   大整型
* `Boolean`  布尔型
* `Float`    浮点型
* `Text`     可变字符串
* `PickleType` pickle序列化的对象
* `Date`     转化成datetime.date()对象
* `DateTime` 转化成datetime.datetime()对象


### 示例
```py
Base = declarative_base()

class User(Base):
    __tablename__ = "user"
    id = Column(Integer, primary_key=True)
    name = Column(String(20), default=None, nullable=False, comment="用户姓名")
    phone = Column(String(20), default=None, nullable=False, comment="电话")
    country = Column(Integer, default=0, nullable=False, comment="国家")
```


## 连接数据库

### 操作
* `<engine> = create_engine(DB_URI)`  创建引擎
* `<session> = sessionmaker(engine)() `  创建session对象



## 查询

### 格式
```
<result> = <session>.query(<Class>) 
                    .filter()       //过滤
                    .order_by()     //排序
                    .group_by()     //分组
                    .limit()        //限制数量
                    .<func>
```  
  

* `<session>.query(<Class>.<attr>, <Class>.<attr>)`  获取特定字段

### 方法
* `all()`  获取所有元素
* `first()`  获取第一个元素
* `one()`  获取一个元素，多个元素和无元素都会异常
* `one_or_none()`  获取一个元素或None
* `scalar()` 获取结果第一列的值 
* `count()`  获取元素个数

### 过滤
* `xxx.filter(<Class>.<attr> == <val>)` 等于
* `xxx.filter(<Class>.<attr> != <val>)` 不等于
* `xxx.filter(<Class>.<attr>.like(<val>))`  LIKE运算符
* `xxx.filter(<Class>.<attr>.in_(<list>))`  IN运算符
* `xxx.filter(~<Class>.<attr>.in_(<list>))`  NOT IN运算符
* `xxx.filter(<Class>.<attr> == None)`   IS NULL运算符
* `xxx.filter(<Class>.<attr>.is_(None))`   IS NULL运算符

### 字符串
* `xxx.filter(<Class>.<attr>.contains(<val>))` 包含子串
* `xxx.filter(<Class>.<attr>.endswith(<val>))` 结尾包含子串
* `xxx.filter(<Class>.<attr>.startswith(<val>))` 开头包含子串


### 多条件过滤
* `xxx.filter(and_(<cond>,<cond>))`      AND运算符
* `xxx.filter(<cond>,<cond>)`      AND运算符
* `xxx.filter(or_(<cond>,<cond>))`      OR运算符

