


# android-adb

[TOC]

## 组件


### adb-client
* PC上运行，提供命令行界面
* 解析命令参数，发送给`adb-server`

### adb-server
* PC上运行，独立的后台进程
* 管理手机，与手机上的adb-daemon通信

### adb-daemon
* 手机上运行
* 与adb-server通信

### smart-socket
* 用于client和server之间的数据传输

### transport
* 用于server和daemon之间的数据传输


![](http://picbed.cc12703.com/20230717234200.png)





## 交互协议

### client-server

#### 格式

##### 请求
* "XXXX" + 负载字符串
* 示例：`"000Chost:version"`


##### 响应
* 成功："OKAY" + 负载数据
* 失败："FAIL" + "XXXX" + 原因字符串


#### 命令
* `host:version` 获取版本信息
* `host:devices` 获取有效的设备列表
* `host:track-devices` 跟踪设备，在设备发生变化时发送数据
* `host-serial:<serial-number>:<request>` 向指定设备发送请求

##### 请求
* `get-serialno` 获取设备串号
* `get-devpath`  获取设备路径
* `get-state`    获取设备状态
* `forward:<local>;<remote>` 转发数据到设备



### server-daemon

#### 格式
消息头
```c++
struct message {
    unsigned command;    //命令标识
    unsigned arg0;       //参数1
    unsigned arg1;       //参数2
    unsigned data_length;  //负载长度
    unsigned data_crc32;   //负载校验值
    unsigned magic;
};
```

#### 命令
* `A_CNXN` 建立连接
    * 消息：`命令标识，版本号，最大数据个数，标识符`
* `A_AUTH` 认证
    * 消息：`命令标识，类型，0，数据`
* `A_OPEN` 打开一个连接
    * 消息：`命令标识，本地标识符，0，目标地址`
* `A_OKAY` 通知连接已建立
    * 消息：`命令标识，本地标识符，远端标识符`
* `A_CLSE` 通知连接已关闭
    * 消息：`命令标识，本地标识符，远端标识符`
* `A_WRTE` 向连接写数据
    * 消息：`命令标识，本地标识符，远端标识符，数据`


#### 示例
![](http://picbed.cc12703.com/20230716003710.png)


## 初始化

### adb-server
* 位置 `./client/main.cpp#adb_server_main()`

#### 流程
1. 初始化传输层 `init_transport_registration`
1. 启动重连 `init_reconnect_handler`
1. 初始化usb `usb_init`
1. 安装ssocket监听器
1. 初始化鉴权
1. 启动事件循环

### adb-daemon
* 位置：`./daemon/main.cpp#adbd_main()`

#### 流程
1. 初始化传输层 `init_transport_registration`
1. 降级权限 `drop_privileges`
1. 初始化鉴权  `adbd_auth_init`
1. 初始化usb `usb_init`


## 检测手机插入
* 位置：`./client/usb_linux.cpp#usb_init()`
* 方法：循环读取`/dev/bus/usb`中的设备信息，查找所有的adb设备

#### 流程
1. 搜索设备 `find_usb_device`
1. 注册设备 `register_device`
    1. 生成usb对象：`usb_handle`
    1. 打开usb设备
    1. 读取设备串号  `/sys/bus/usb/devices/%s/serial`
    1. 注册usb transport `register_usb_transport` 
        1. 注册transport `register_transport`




## 传输层（transport）


### 数据格式

* `asocket`：处理`apacket`流
* `apacket`：包含`amessage` + 负载数据

![](http://picbed.cc12703.com/20230717234637.png)






## 参考资料
* [trivia-ADB通信协议](https://github.com/jakkypan/trivia/blob/master/ADB%E9%80%9A%E4%BF%A1%E5%8D%8F%E8%AE%AE.md)