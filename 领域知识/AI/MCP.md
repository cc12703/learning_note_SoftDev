

# MCP


## 概述


### 概述
* Model Context Protocol: 模型上下文协议
* 提供一个标准方法，将AI应用连接到不同的数据和工具


### 要点
* 采样客户端-服务器模式
* 使用`JSON-RPC 2.0`作为消息协议
* 支持多种数据传输方式


### 传输层
* `STDIO`: 标准输入/输出，连接本地服务器
* `HTTP+SSE`: 连接远程服务器



## 组件

### MPC主机 
* 一个AI应用程序
    * 例子：Cursor, Claude Desktop, 基于网页的AI聊天
* 初始化连接到多个`MCP服务器`

### MCP客户端
* 整合在`MPC主机`内部
* 与`MCP服务器`保持一对一连接

### MCP服务器
* 实现特定功能
* 通过`MCP协议`暴露


### 图示
![](https://raw.githubusercontent.com/cc12703/picbed/main/20251223174640077.png)



## 功能

### 提示词
* 预定义的模版、指令，用于指导大模型互动
* 被设计成用户驱动的，根据用户的意愿来选择

### 资源


### 工具




## 交互流程


### 图示
![](https://raw.githubusercontent.com/cc12703/picbed/main/20251224231042773.png)

### 建立连接
* 使用传输层来建立连接
* 握手过程：协商版本号，交互能力，创建会话




## 协议消息

### 初始化
* `initialize`: 建立连接，协商协议版本、协商能力
* `notifications/initialized`: 初始化完成

### 发现
* `tools/list`：发现可用工具
* `resources/list`：发现可用资源
* `prompts/list`： 发现可用提示模版


### 执行
* `tools/call`：执行工具
* `resources/read`：检索特定资源
* `prompts/get`：获取提示模版

### 客户端
* `sampling/complete`：服务器请求客户端进行LLM补全
* `notifications/resources/list_changed`：服务器通知客户端资源列表变更
* `notifications/prompts/list_changed`: 服务器通知客户端提示列表变更

