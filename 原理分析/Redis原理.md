


# Redis原理


## 概述
* Redis是一个键值数据库


## 单机

### 模块
* 访问框架
* 索引模块
* 操作模块
* 存储模块


## 集群

### 特点
* 多主多从，去中心化
* 支持动态扩容
* 不支持处理多个键

### 结构
* 至少3个主节点，每个主节点至少一个从节点
* 从节点只作为一个数据备份角色，不对外提供服务

![](http://picbed.cc12703.com/20240225155702.png)


### 数据分区
* 数据在多个节点如何存储
* 方案：一致性哈希复合分区（哈希槽）


#### 哈希槽
* 将存储空间分成16384个槽位（Slot）
* 每个槽映射一个数据子集
* 每个节点维护一份`Slot<->节点`的映射关系
* 键通过CRC16获取哈希值，再和槽位数取模，获取对应的槽位


![](http://picbed.cc12703.com/20240225162033.png)




### 通信协议
* 节点之间使用`gossip`协议交互数据


#### 交换信息
* 故障信息
* 节点变化信息
* 槽位迁移信息

#### 消息类型
* `MEET` 请求新节点加入集群
* `PING` 检测节点是否在线
* `PONG` 响应`MEET`,`PING`消息/主动广播信息
* `FAIL` 广播对某个节点的宕机判断
* `PUBLISH` 向指定Channel发送消息
    



## 参考资料
* https://zhuanlan.zhihu.com/p/337878020
* https://www.cnblogs.com/Chary/articles/15021729.html