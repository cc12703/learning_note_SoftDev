

# Redis用法

## 概述
* 所有的命令都是原子操作
* 键命名：对象类型`:`对象ID`:`对象属性


## 基础命令

### 字符串类型
存储任何形式的字符串、二进制数据

* `SET <key> <value>`  设置值
* `<value> = GET <key>`          获取值
* `<len> = APPEND <key> <value>` 向末尾添加值
* `<len> = STRLEN <key>`    获取长度（字节个数）
* `MSET <key> <value> <key> <value> ...` 设置多个值
* `MGET <key> <key> ...`  获取多个键的值

* `INCR <key>`         递增数字（字符串为整型）
* `DECR <key>`         递减数字
* `INCRBY <key> <value>` 增加指定的整数
* `DECRBY <key> <value>` 减少指定的整数

* `<value> = GETBIT <key> <offset>`  获取特定位的值
* `SETBIT <key> <offset> <value>`    设置特定位的值
* `BITCOUNT <key> [start] [end]`     统计值为1的位的个数

#### 自增ID
* 使用名为 `对象类型:count` 键来存储对象的数量
* 使用`INCR`指令进行自增


### 哈希类型
* 只能存储字符串，适合存储对象

* `HSET <key> <field> <value>`  设置字段值，包含插入和更新操作
* `HSETNX <key> <field> <value>`  当字段不存在时，才设置值
* `HGET <key> <field>`          获取字段值
* `HMSET <key> <field> <value> <field> <value> ...` 设置多个字段值
* `HDEL <key> <field>`             删除字段

* `HGETALL <key>`  获取所有字段的值
* `<int> = HEXISTS <key> <field>`  判断字段是否存在（存在为1）
* `HINCRBY <key> <field> <value>`  增加数字

* `HKEYS <key>`  获取所有的字段名
* `HVALS <key>`  获取所有的值
* `HLEN <key>`   获取字段数量

### 列表类型
* 内部使用双向链表实现
* 索引从0开始
* 支持负索引

* `LPUSH <key> <value> <value> ...` 向列表左边增加元素（列表头）
* `RPUSH <key> <value> <value> ...` 向列表右边增加元素（列表尾）
* `LPOP <key>`  从列表左边弹出一个元素
* `RPOP <key>`  从列表右边弹出一个元素

* `LLEN <key>`  获取列表中元素个数
* `LRANGE <key> <start> <stop>` 获取列表中的某一片段，stop包含在返回值中
* `LREM <key> <count> <value>`  删除前count个值为value的元素
    * `count > 0` 从列表左边开始删除
    * `count < 0` 从列表右边开始删除
    * `count = 0` 删除所有的值

* `LINDEX <key> <index>`  获取指定索引的元素值
* `LSET <key> <index> <value>` 设置指定索引的元素值
* `LINSERT <key> BEFORE|AFTER <pivot> <value>` 在pivot元素的前、后插入元素

* `RPOPLPUSH <source> <dest>` 从一个列表转移元素到另一个列表
    * 从source列表的右边弹出一个元素，加入dest列表的左边


### 集合类型
* 内部使用哈希表实现
* 每个元素都是不同的，没有顺序
* 支持并、交、差运算

* `SADD <key> <member> <member> ...` 增加元素
* `SREM <key> <member> <member> ...` 删除元素
* `SMEMBERS <key>`                   获取所有元素
* `SISMEMBER <key> <member>`         判断元素是否在集合中（存在返回1）
* `SCARD <key>`                      获取元素个数

* `SDIFF <key> <key> ...`            执行差集运算
* `SINTER <key> <key> ...`           执行交集运算
* `SUNION <key> <key> ...`           执行并集运算


### 有序集合类型
* 每个元素都关联了一个分数
* 内部使用哈希表 + 跳表 实现

* `ZADD <key> <score> <member> <score> <member> ...` 增加元素
* `ZREM <key> <member> <member> ...`   删除元素
* `ZSCORE <key> <member>`      获取元素的分数
* `ZCARD <key>`                获取集合中的个数

* `ZRANGE <key> <start> <stop> [WITHSCORES]` 获取在指定范围内的元素列表
    * 元素按分数从小到大进行排序
    * `WITHSCORES` 返回格式为`<memeber>,<score>`
* `ZREVRANGE <key> <start> <stop> [WITHSCORES]` 获取在指定范围内的元素列表
    * 元素按分数从大到小进行排序
* `ZRANGEBYSCORE <key> <min> <max> [WITHSCORES]` 获取指定分数范围内的元素列表
    * 元素按分数从小到大进行排序
    * `WITHSCORES` 返回格式为`<memeber>,<score>`
* `ZCOUNT <key> <min> <max>`   获取指定分数范围内的元素个数

* `ZINCRBY <key> <value> <member>`  增加指定元素的分数
    *  `<value>` 可用为负数
* `ZRANK <key> <member>`  获取元素的排名，按元素分数从小到大排序