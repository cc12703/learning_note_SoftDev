
# UIAutomator2原理


## 总体

### 组件
* `atx-agent`: 运行在手机上的二进制程序
    * 提供HTTP接口
    * 屏蔽手机差异
* `stub`：运行在手机上的apk程序
    * 提供rpc接口
    * 操作系统的Instrumentation
* `atx`：运行在手机上的apk程序
    * 提供用户界面：查看状态、操作程序
* `wrapper` 运行在PC上的python库
    * 提供方便的操作接口
    * 对接`atx-agent`




## stub组件


### HTTP服务器
* 端口号：9008
* 类：<AutomatorHttpServer>

#### url
* `/jsonrpc/0`  RPC调用
* `/stop`       停止服务
* `/screenshot/0`  截屏


### 封装接口
* 类： <AutomatorServiceImpl>




## atx组件


### 用户界面  
* 类：<MainActivity>


### 输入法
* 类：<FastInputIME>
* 通过自定义输入法服务来实现功能 `InputMethodService`


### 监测系统
* 通过系统广播监测电池、屏幕旋转、wifi状态
* 将信息发送给`atx-agent`



## atx-agent组件

### HTTP服务器
* 文件：`httpserver.go`
* 端口号： `7912`



### 转发rpc
* 将rpc转发给stub组件



## wrapper组件


### 连接agent
* 通过wifi连接 `connect_wifi`
* 通过USB连接  `connect_usb`
    * 通过端口转发与agent通信


### 安装agent
* 函数：`Initer.install()`

#### 流程
1. 安装`minitouch`到`/data/local/tmp/`
1. 安装`minicap`到`/data/local/tmp/`
1. 安装`app-uiautomator.apk`
1. 安装`app-uiautomator-test.apk`
1. 安装`atx-agent`到`/data/local/tmp/`
1. 启动`atx-agent`
    命令： `/data/local/tmp/atx-agent server --nouia -d --addr <addr>`


### 输入字符
* 类：<_InputMethodMixIn>
* 通过发送系统广播给`atx`来实现功能


