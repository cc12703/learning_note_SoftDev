
# HTTP协议

[TOC]

## 概述
* HTTP是Hyper Text Transfer Protocol（超文本传输协议）的缩写
* HTTP是一个基于TCP的应用层传输协议
* 工作于客户端/服务器架构


### 特点
* 灵活：HTTP允许传输任意类型的数据对象
* 无连接：每次连接只处理一个请求
* 无状态：协议对于事务处理没有记忆能力



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


## 请求方法
* GET：请求指定页面，获取具体内容
* HEAD：请求指定页面，只获取报文头
* POST：向指定页面提交数据
* PUT： 提交新数据来替换指定的内容
* DELETE：删除指定页面
* OPTIONS：查询服务器性能


## 状态码

### 分类
* 1xx：信息，服务器收到请求，需要请求者继续执行操作
* 2xx：成功，操作被成功接收并处理
* 3xx：重定向
* 4xx：客户端错误
* 5xx：服务器错误

### 常见码
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




## 头字段

### 通用
* Date：报文的创建时间，固定格式(HTTP-Date格式)
* Referer：发起请求的源URI
* User-Agent：当前用户的代理信息

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
	* application/json
	* application/octet-stream

#### 质量值
* 功能：用于在多种选项值中描述偏好
* 格式：`xxx;q=<val>`
* 值范围：0.0 - 1.0 (1.0优先级最高)
* 示例
	* `Accept-Language: en;q=0.5, fr;q=0.0, nl;q=1.0, tr;q=0.0`

#### 内容编码方式
* 指定压缩方式
* 值：`gzip`,`deflate`,`br`


### 缓存协商

#### 强制缓存
* 基于时效性进行缓存
* Cache-Control：缓存策略
	* max-age：多少秒内缓存是有效的（相对于请求时间）
	* s-maxage：多少秒内是共享缓存有效的（被CDN，代理缓存）
	* public：可以由CDN,代理进行缓存
	* private：只能由用户的客户端缓存
	* no-cache：不能被缓存，不影响协商缓存
	* no-store：禁止浏览器、CDN保存该资源
	* no-transform：禁止任何形式的修改资源


#### 协商缓存
* 基于变化检测进行缓存
* 根据资源修改时间
	* Last-Modified：服务器告知客户端资源的最后修改时间
	* If-Modified-Since：客户端将资源的最后修改时间带回服务器
* 根据资源是否变化
	* ETag：服务器告知客户端资源的唯一标识
	* If-None-Match：客户端将唯一标识带回服务器


### 分块传输
* 启用：`Transfer-Encoding: chunked`
* 每个分块包含一个数据长度（十六进制）和 真实数据
	* 图示：![](http://picbed.cc12703.com/20211025200259.png)
* 使用一个长度值为0的分块标记传输结束
* 使用`Trailer`在数据末尾追加一段数据


### 身份认证
* WWW-Authenticate：告知客户端认证方式
	* 方式包括：Basic 和 Digest
* Authorization：上报服务器认证信息