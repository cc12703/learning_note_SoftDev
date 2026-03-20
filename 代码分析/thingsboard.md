
# thingsboard



## Actor系统

### 要点
1. 轻量级的单线程消息处理框架

### 特点
* Actor之间通过异步消息通信
* 每个 Actor 有独立的状态
* Actor 内部使用单线程处理消息

### 组件
* Actor 实体
* 消息邮箱
* 分发器

### TbActor
* 定义了 Actor 的公共接口
* `process()` 处理消息
* `init()` 初始化
* `destroy()` 清理资源
* `onInitFailure(), onProcessFailure()` 处理异常


#### 层级关系
AppActor (系统级)
  └─ TenantActor (租户级)
      ├─ DeviceActor (设备会话与状态)
      ├─ RuleChainActor (规则链实例)
      └─ AssetActor / AlarmActor 等

#### AppActor
* 为每个租户创建一个`TenantActor`
* 按消息类型，分发消息




### TbActorMailbox
* 每个 Actor 拥有一个邮箱
* 邮箱维护两个消息队列：普通优先级消息，高优先级消息

### Dispatcher
* 每个 Actor 绑定在一个调度器上
* 是线程池的简单包装


## 规则引擎

### 组件
* 消息载体
* 规则链
* 规则节点

### TbMsg
* 统一的消息载体
* 不可变的
* 包含：数据载荷、上下文信息

#### 字段
* `queueName` 目标处理队列
* `id` 唯一标识
* `dataType` 数据类型
* `data` 数据
* `metaData` 上下文信息
* `ruleChainId` 当前规则链位置
* `ruleNodeId`  当前规则节点位置

#### 消息类型
* 设备遥测：POST_TELEMETRY_REQUEST, TIMESERIES_UPDATED, TIMESERIES_DELETED
* 设备属性：POST_ATTRIBUTES_REQUEST, ATTRIBUTES_UPDATED, ATTRIBUTES_DELETED
* 连接状态：CONNECT_EVENT, DISCONNECT_EVENT, ACTIVITY_EVENT, INACTIVITY_EVENT

### TbNode
* 定义了 规则节点 公共接口
* `init()` 初始化
* `destroy()` 销毁
* `onMsg()` 处理消息

### TbContext
* 定义了 规则节点 与平台交互的接口

#### 消息流控制
* `tellSuccess()` 路由到`Success`连接到的所有节点
* `tellFailure()` 路由到`Failure`连接到的所有节点
* `tellSelf()` 延时后重新路由到当前节点


## 队列系统

### 接口层
* `TbQueueProducer` 生产者
* `TbQueueConsumer` 消费者
* `TbQueueAdmin`  主题管理


## 主要逻辑

### 接收MQTT消息

#### 流程
1. MqttTransportHandler解析 MQTT 消息
1. MqttAdapter 转换 MQTT 消息负载
1. TransportService 发送消息到对应的消息队列

#### 调用关系
* `MqttTransportHandler->processMqttMsg`
    * `DefaultTransportService->process`
        * `TbRuleEngineProducerService->sendToRuleEngine`