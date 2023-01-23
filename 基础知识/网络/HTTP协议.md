
# HTTP协议

[TOC]

## 概述
* HTTP是Hyper Text Transfer Protocol（超文本传输协议）的缩写
* HTTP是一个基于TCP的应用层传输协议
* 工作于客户端/服务器架构


### 特点

#### 灵活可扩展
* 报文中的各个组成部分都可以定制
* 允许传输任意类型的数据

#### 请求-应答模式
* 连接和请求永远是请求方发起的，是主动的
* 应答方只有收到请求后才能答复，是被动的

#### 无状态
* 每个请求时互相独立，毫无关联的
* 优点：
	* 实现上简单，减轻服务器的负担
	* 更容易的组成集群，扩展性能
* 缺点：
	* 无法支持多步骤的事务操作


#### 明文
* 报文使用简单的文本形式
* 优点：
	* 开发调试更方便
* 缺点：
	* 传输过程完全透明
	* 不安全，无身份认证和完整性校验功能



## 定位

### URI
* 统一资源标志符（Uniform Resource Identifier）
* 一个用于标识互联网资源的字符串
* 进一步分为`URL`和`URN`

#### 编码方式
* 只能使用ASCII
* 其他字符都需要转为十六进制字节值

#### 格式
`[scheme][://]user:passwd@[host:port][path][?query][#fragment]`
* scheme：协议
	* 例子：`ftp`,`file`,`http`,`https`,`ssh`
* host：主机，一般都使用域名
* port：端口号
* path：路径，服务器上资源的路径
* query：查询，提供额外参数，通过`&`分隔的键/值对
* fragment：片段，表示资源某一部分的一个锚点

### URL
* 统一资源定位符（Uniform Resource Locator）
* URI的最常见的一种形式
* 示例
	* `http://www.aspxfans.com:8080/news/index.asp?boardID=5&ID=24618&page=1#name`


### URN
* 永久统一资源定位符（Uniform Resource Name）
* URI的另一种形式
* 示例
	* `urn:isbn:9780141036144`
	* `urn:ietf:rfc:7230`




## 消息格式

### 请求消息
* 请求行：请求方法、URL、协议版本号
* 请求头部：由一个或多个键值对组成
* 空行
* 请求数据

#### 图示
![](http://picbed.cc12703.com/20211025161908.png)

#### 示例
```http
POST / HTTP1.1
Host:www.wrox.com
User-Agent:Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 2.0.50727; .NET CLR 3.0.04506.648; .NET CLR 3.5.21022)
Content-Type:application/x-www-form-urlencoded
Content-Length:40
Connection: Keep-Alive

name=Professional%20Ajax&publisher=Wiley
```


### 响应消息
* 状态行：协议版本号、状态码、状态信息
* 请求头部：由一个或多个键值对组成
* 空行
* 响应数据

#### 图示
![](http://picbed.cc12703.com/20211025161921.png)

#### 示例

```http
HTTP/1.1 200 OK
Date: Fri, 22 May 2009 06:07:21 GMT
Content-Type: text/html; charset=UTF-8

<html>
      <head></head>
      <body>
            <!--body goes here-->
      </body>
</html>
``` 


### 请求方法
* GET：获取资源（具体数据）
* HEAD：获取元信息（报文头）

* POST：提交数据，用于新建资源
* PUT： 提交数据，用于修改资源
* DELETE：删除资源

* CONNECT：建立特殊的连接隧道
* OPTIONS：列出可用的方法


### 状态码

#### 分类
* 1xx：信息，服务器收到请求，需要请求者继续执行操作
* 2xx：成功，操作被成功接收并处理
* 3xx：重定向
* 4xx：客户端错误
* 5xx：服务器错误

#### 常见码
* 100：客户端需要继续请求
* 200：请求成功
* 201：已创建了新的资源
* 204：服务器处理成功，但未返回内容
* 301：永久移动，请求的资源被永久的移动到新的URI
* 302：临时移动 
* 304：未修改，从上次请求后数据未修改过
* 400：错误请求
* 401：未授权，需要身份验证
* 403：禁止，服务器拒绝请


### 头字段

#### 通用
* Date：报文的创建时间，固定格式(HTTP-Date格式)
* Referer：发起请求的源URI
* User-Agent：当前用户的代理信息

#### 分块传输
* 启用：`Transfer-Encoding: chunked`
* 每个分块包含一个数据长度（十六进制）和 真实数据
	* 图示：![](http://picbed.cc12703.com/20211025200259.png)
* 使用一个长度值为0的分块标记传输结束
* 使用`Trailer`在数据末尾追加一段数据


#### 身份认证
* WWW-Authenticate：告知客户端认证方式
	* 方式包括：Basic 和 Digest
* Authorization：上报服务器认证信息




## 实体数据


### 内容协商
* Accept：告知服务器，客户端可以处理的**媒体类型**
* Accept-Charset：告知服务器，客户端可以处理的**字符集类型**
* Accept-Encoding：告知服务器，客户端可以处理的**内容编码方式**
* Accept-Language：告知服务器，客户端可以处理的**自然语言**

* Content-Type：表明资源的**媒体类型**
* Content-Encoding：表明资源的**内容编码方式**
* Content-Language：表明资源的**自然语言**


#### 媒体类型
* MIME类型，互联网媒体类型
* 格式：`type/subtype(;parameter)`
	* type: 主类型，`*`代表所有
	* subtype：子类型，`*`代表所有
	* parameter：可选参数
* 示例
	* text/html
	* text/html; charset=utf-8
	* image/png
	* audio/video
	* application/json
	* application/octet-stream


#### 内容编码方式
* 指定压缩方式
* 值：`gzip`,`deflate`,`br`


#### 质量值
* 功能：用于在多种选项值中描述偏好
* 格式：`xxx;q=<val>`
* 值范围：0.0 - 1.0 (1.0优先级最高)
* 示例
	* `Accept-Language: en;q=0.5, fr;q=0.0, nl;q=1.0, tr;q=0.0`



## 大文件

### 数据压缩
* 使用`XXX-Encoding`头字段，协商压缩算法


### 分块传输
* 将数据分成许多块(chunk)逐个发送


#### 字段
* `Transfer-Encoding:chunked` 服务器支持分块
* `Accept-Ranges: bytes`  服务器支持范围请求
* `Range: bytes=x-y`  客户端请求的范围
* `Content-Range: bytes x-y/length`  服务器返回的范围


#### 响应值
* `416` 请求的范围越界
* `206` 返回的数据只是一部分 

#### 编码规则
* 每个分块分成：长度头、数据块
* 长度头以回车换行结尾、16进制表示长度
* 数据块以回车换行结尾
* 使用长度为0的分块结束

![](http://picbed.cc12703.com/20230122181903.png)


#### 范围请求
* 格式：`Range: bytes=x-y`
	* x,y为偏移量，从0计数




## 连接管理



### 长连接
* 将建立、关闭连接的成本均摊到多个`请求-应答`上


#### 短连接
* 通信过程为`请求-应答`方式
* 每次发生请求前都需要与服务器建立连接，收到应答后立即关闭连接
* 缺点：效率太低，时间都浪费在了连接的建立和关闭

#### 图示
![](http://picbed.cc12703.com/20230122230310.png)


#### 字段
* `Connection:keep-alive` 客户端要求使用长连接
* `Connection:close`  客户端要求关闭连接



### 队头阻塞

* `请求-应答`模型导致所有请求都必须排队
* 队列头部的请求延时，会阻塞队列后部请求的处理

#### 解决方法
* `并发连接`：对同一域名发起多个长连接
* `域名分片`：多个域名指向同一台服务器




## 重定向

### 概述
* 使用状态码表示重定向类型
* 使用`Location`表示要跳转的URI

### 类型
* `301` 永久重定向
* `302` 临时重定向
* `303` 临时重定向，重定向后使用`GET`方法
* `307` 临时重定向，重定向后方法和实体不允许变动
* `308` 永久重定向，重定向后方法和实体不允许变动


## Cookie

* 服务器委托浏览器存储一些数据

### 原理
* 服务器使用`Set-Cookie`字段将信息存入浏览器
* 浏览器使用`Cookie`字段将信息带回服务器
* 格式为`key=value`，多个使用`;`分隔

![](http://picbed.cc12703.com/20230122131757.png)


### 属性
* 生存周期：只在一段时间内有效
	* `Expires`：过期时间
	* `Max-Age`：最大有效时间，从收到报文开始算
* 作用域：只发送给特定的服务器和URI
	* `Domain`：所属于的域名
	* `Path`：所属于的路径
* 安全性：防止被其他人看到
	* `HttpOnly`：只能通过浏览器HTTP传输
	* `SameSite`：防范跨站请求伪造
	* `Secure`：只能用HTTPS传输

### 应用
* 身份识别：保存用户的登录信息，实现会话
* 广告跟踪：标识用户身份，用于推送广告



## 缓存（浏览器）

### 控制
* 服务器使用`Cache-Control`进行缓存控制
* 浏览器也可使用`Cache-Control`进行缓存控制
	* 使用`max-age=0`或`no-cache`刷新数据


#### `Cache-Control`
* max-age：多少秒内是有效的（相对于请求时间）

* no-store：禁止缓存
* no-cache：可以缓存，使用前需要验证是否过期
* must-revalidate：可以缓存，过期后需要验证是否过期


### 验证过期
* 使用`If`开头的`条件请求`字段


#### 流程

![](http://picbed.cc12703.com/20230122140719.png)


#### 字段
* 根据资源修改时间
	* Last-Modified：服务器告知浏览器资源的最后修改时间
	* If-Modified-Since：浏览器将资源的最后修改时间带回服务器
* 根据资源是否变化
	* ETag：服务器告知浏览器资源的唯一标识
	* If-None-Match：浏览器将唯一标识带回服务器


### 控制策略

![](http://picbed.cc12703.com/20230122161303.png)


## 缓存（服务器）

* 使用`缓存代理`实现


### 字段

* `Vary`： 标明内容协商时的参考头字段
	* 认为是响应报文的一个版本标记
	* 缓存代理用于缓存不同版本的数据


#### `Cache-Control`
* s-maxage：在代理上多少秒内是有效的

* public：浏览器和代理都可以缓存
* private：只能在浏览器上进行缓存

* no-transform：代理不能修改缓存的数据


### 控制策略

![](http://picbed.cc12703.com/20230122161044.png)

## 代理

### 类型
* `匿名代理`：完全屏蔽掉被代理的机器，外界只看到代理
* `透明代理`：外界既知道代理、也知道被代理的机器
* `正向代理`：靠近浏览器，代表浏览器向服务器发送请求
* `反向代理`：靠近服务器，代表服务器响应浏览器请求

### 功能
* `负载均衡`：将请求分散到多台机器上
* `内容缓存`：存储数据，减轻后端的压力
* `安全防护`：保护被代理的机器
* `数据处理`：提供压缩、加密功能


### 字段
* `Via`：标明代理的身份（主机名、域名）
	* 每经过一级代理，都会增加一次信息
* `X-Forwarded-For`：标明请求方的IP地址
	* 每经过一级代理，都会增加一次信息
* `X-Real-IP`：标明客户端IP地址

