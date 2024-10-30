

# python



## 动态内容

### CGI
* Common Gateway Interface 通用网关接口
* 一个协议，定义了Web服务器调用外部程序时的接口标准
* 通过标准输入、标准输出和环境变量进行交互

#### 机制
* 一个请求生成一个新的CGI进程，处理完后退出


#### 图示
![](http://picbed.cc12703.com/20220426115816.png)


### FastCGI
* 一个协议，定义了Web服务器和语言解释器底层的通信协议
* 是对CGI协议的扩展


#### 机制
* 启动一个master进程和多个worker进程
* worker进程处理完不退出



### 服务器 VS 框架

### 概述
* Web服务器用于接受客户端请求，建立连接，转发响应
* Web框架用于处理理业务逻辑

#### 示例
* Nginx 一个Web服务器
* Django，Flask 一个Web框架


### WSGI
* WEB SERVER GATEWAY INTERFACE Web服务器网关接口
* 一个协议，定义了Web服务器和Web框架之间的通信协议


#### 图示
![](http://picbed.cc12703.com/20220426121605.png)


### uWSGI
* 一个Web服务器，实现了WSGI、uwsgi、http协议
* 运行模式
	* 服务器：uWSGI+App
	* 中间件：Nginx+uWSGI+App

#### 中间件模式

##### 构成
* Nginx：代理服务器，负责静态资源的发送
* uWSGI：后端服务器，转发请求给App
* App：应用，处理数据，渲染页面

##### 图示
![](http://picbed.cc12703.com/20220426121950.png)



### ASGI
* 一个异步网关协议接口
* 处理多种通用协议：http, http2, websocket