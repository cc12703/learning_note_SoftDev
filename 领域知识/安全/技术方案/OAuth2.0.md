

# OAuth2.0

[TOC]

## 概述

### 是什么
* 是一个授权协议，允许应用代表资源拥有者去访问资源拥有者的资源
* 应用向资源拥有者请求授权，然后获取令牌，最后用它访问资源
* 令牌可以限制应用只能执行资源拥有者授权的操作

### 角色
* 资源拥有者：有权访问资源，并能将资源访问权限委托出去
* 受保护资源：大多数情况下是某种形式的Web API
* 客户端：软件，能代表资源拥有者访问受保护资源

### 目标
* 让客户端为资源拥有者访问受保护资源

## 要点

### 授权访问
* 授权服务器提供了一种机制，来弥补客户端和受保护资源之间的间隙
* 受保护资源依赖于OAuth访问令牌
* 访问令牌是授权服务器向客户端颁发的专用的安全凭据
* 过程：客户端请求令牌，用户对客户端授权，客户端管理令牌，用户管理客户端

#### 图示
![](http://picbed.cc12703.com/20201231183044.png)

### 授权委托
* 委托是OAuth强大功能的根基
* OAuth也是一个委托协议
* 被委托的是用户权限的子集，OAuth本身不承载或者传递权限
* OAuth令牌中的授权信息对系统中大部分组件是不透明的

### 用户主导安全
* 重要的安全决策由最终用户来做
* OAuth遵循TOFU原则（首次使用时信任原则）

#### TOFU原则
* 用户在第一次运行是需要进行安全决策
* 不为安全决策预设任何先决条件
* 系统会记住用户的决策，以便以后使用

##### 优点
* 让用户从功能而不是安全性的角度做成决策


#### 3层名单机制
* 白名单：已知良好和受信任的应用
* 黑名单：已知不良的应用
* 灰名单：使用TOFU原则

### 不能做什么
* 没有定义HTTP协议之外的情形：不应该脱离HTTPS
* 不是身份认证协议：事务本身不透露关于用户的信息
* 没有定义用户对用户的授权机制：假设资源拥有者能够控制客户端
* 没有定义授权处理机制：不定义授权的内容
* 没有定义令牌格式：令牌本身对客户端是不透明的
* 没有定义加密方法：允许借用通用的加密机制
* 不是单体协议：规范被分成了多个定义和流程，每个定义和流程都有各自适用的场景


## 协议概述
### 要点
* 重要步骤：颁发令牌、使用令牌
* 令牌表示授权客户端的访问权

### 事务流程
1. 资源拥有者向客户端表示希望客户端代表它执行的一些任务
1. 客户端在授权服务器上向资源拥有者请求授权
1. 资源拥有者许可客户端的请求
1. 客户端接收到来自授权服务器的令牌
1. 客户端向受保护资源出示令牌

### 授权码许可流程
#### 图示
![](http://picbed.cc12703.com/3311609687281.png)


### 角色
#### 客户端
* 代表资源拥有者访问受保护资源的软件
* 使用OAuth来获取访问权限
* 通常是OAuth系统中最简单的组件

##### 职责
* 从授权服务器获取令牌
* 在受保护资源上使用令牌

#### 受保护资源
* 能够通过HTTP服务器进行访问，访问时需要访问令牌
* 需要验证收到的令牌
* 对是否认可令牌拥有最终决定权

#### 授权服务器
* 一个HTTP服务器，充当中央组件

##### 职责
* 对资源拥有者、客户端进行身份认证
* 让资源拥有者向客户端授权
* 为客户端颁发令牌

#### 资源拥有者
* 是有权将访问权限授权给客户端的主体
* 大多数情况下，资源拥有者是一个人
* 使用客户端访问受他控制的资源

### 组件
#### 访问令牌
* 由授权服务器发给客户端，表示已被授予权限
* 令牌对于客户端是不透明的（无需知道内容）
* 受保护的资源需要验证令牌
* OAuth没有定义令牌本身的格式和内容

#### 权限范围
* 表示一组访问受保护资源的权限
* OAuth使用字符串表示权限范围：用空格分离的列表
* 由受保护资源根据自身提供的API来定义

#### 刷新令牌
##### 概述
* 由授权服务器发给客户端
* 客户端使用该令牌向授权服务器请求新的访问令牌

##### 流程图示
![](http://picbed.cc12703.com/20210104232155.png)

##### 原因
* 刷新令牌取代了永不过期的访问令牌
* 以一种独立但互补的方式限制了刷新令牌、访问令牌的暴露范围

#### 授权许可
* 是OAuth中一种获取权限的方法
* 流程：客户端将用户重定向至授权端点，接收授权码，用授权码换取令牌


### 交互
#### 后端信道
* 使用标准的HTTP请求和响应格式
* 发生在资源拥有者和用户代理的可见范围之外

#### 前端信道
* 一种间接通信方法
* 将web浏览器作为媒介，使用HTTP请求来实现
* 隔离了浏览器两端的会话，实现了跨安全域工作

##### 实现原理
* 发起方在一个URL中附加参数并指示浏览器跳转至该URL
* 接收方解析该入站URL（经过浏览器跳转）
* 接收方将浏览器重定向至发起方托管的URL,并附加参数

##### 流程图示
![](http://picbed.cc12703.com/3331609775964.png)


### 授权许可类型
#### 隐式许可
##### 场景
* 用于内嵌在网站上的js应用

##### 特点
* 直接从授权端点返回令牌
* 只使用前端信道和授权服务器通信
* 不可用于获取刷新令牌

##### 假设
* 资源拥有者一直在场，必要是可以对客户端重新授权

#### 客户端凭据许可
##### 场景
* 没有明确的资源拥有者
* 对于客户端来说，资源拥有者不可区分

##### 特点
* 客户端代表自己从令牌端点获取令牌
* 只使用后端信道和授权服务器通信


#### 资源拥有者凭据许可
##### 特点
* 客户端使用用户名和密码获取令牌
* 只使用后端信道和授权服务器通信


#### 断言许可
##### 特点
* 客户端会得到一条结构化的被加密保护的信息（称为断言）
* 客户端使用断言向授权服务器换取令牌
* 只使用后端信道和授权服务器通信

##### 断言格式
* 安全断言标记语言 - SAML
* JSON Web Token - JWT

#### 如何选择
![](http://picbed.cc12703.com/20210108234524.png)



### 客户端部署
#### web应用
##### 特点
* 运行在远程服务器上，需要通过浏览器访问
* 配置和运行状态有服务器维护，使用会话cookie与浏览器保持连接
* 容易使用前端信道、后端信道

##### 许可流程
* 授权码
* 客户端凭据
* 断言

#### 浏览器应用
##### 特点
* 完全运行在浏览器内
* 应用的运行状态和所有执行动作都发生在浏览器内
* 只容易使用前端信道

##### 许可流程
* 隐式

#### 原生应用
##### 特点
* 直接在用户设置上运行
* 只容易使用后端信道

##### 许可流程
* 授权码
* 客户端凭据
* 断言



## 常见漏洞




## 令牌实现

### 令牌是什么
* 表示的是授权行为的结果
* 一个信息元组：资源拥有者、客户端、授权服务器、受保护资源、权限范围、其他信息


### 结构化令牌（JWT）
* 授权服务器将受保护资源需要知道的信息全部打包
* 受保护资源解析令牌包含的信息，做出授权决策

#### 结构
* 核心是将一个JSON对象封装为一种用于网络传输的格式
* 以句点分隔不同部分
* 句点之间的部分是一个经过Base64URL编码的JSON对象

##### JWT头部
* 第一部分的JSON对象
* 用于描述其他部分的相关信息
* typ : 第二部分（载荷）的类型
* alg : 签名类型

```json
{
    "typ" : "JWT",
    "alg" : "none"
}
```

##### 载荷
* 第二部分的JSON对象

```json
{
    "sub" : "1234567890",
    "name" : "John Doe",
    "admin" : true
}
```

##### JWT声明
* iss 令牌颁发者，表示令牌是谁创建的
* sub 令牌的主体，表示令牌是关于谁的
* aud 令牌的受众，表示令牌的接收者
* exp 令牌的过期时间戳
* nbf 令牌的生效时间戳


#### JOSE
* 用于令牌的加密保护
* 一套标准，对JSON对象进行签名和加密的标准

##### 功能
* 签名：JSON Web签名
* 加密：JSON Web加密
* 密钥存储格式：JSON Web密钥


### 令牌内省
* 在线获取令牌信息

#### 内省协议
* 定义了一种机制，让受保护资源可以主动向授权服务器查询令牌状态
* 是对OAuth的一个简单增强

##### 请求
* 发送给授权服务器的内省端点
* 是表单形式的HTTP请求

##### 响应
* 是一个JSON对象，用于描述令牌信息
* 内容与JWT的载荷相似


### 结合实现
* JWT可以携带基本信息：有效期、唯一标识符、颁发者
* 通过令牌内省获取更详细的令牌信息


### 令牌撤回

#### 令牌撤回协议
* 是一个简单的协议，让客户端告诉授权服务器：我持有这个令牌，并希望撤销它
* 服务器需要实现一个专门的撤回端点
* 客户端需要发送带身份认证的HTTP POST请求


#### 生命周期
* 访问令牌、刷新令牌都由授权服务器创建
* 由客户端使用
* 由受保护资源验证

##### 图示
![](http://picbed.cc12703.com/oauth2_token_life.png)




## 客户端动态注册

### 注册协议
#### 功能
* 客户端可以自行加入授权服务器，并注册自己的相关信息
* 授权服务器会提供唯一的客户端ID


