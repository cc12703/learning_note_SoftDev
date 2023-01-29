
# HTTP2协议

[TOC]

## 概述

* 唯一目标就是改进性能


### 特性
* 通过`语义不变`来兼容HTTP
* 通过`二进制格式`和`头部压缩`，来减少传输的数据量
* 通过`虚拟流`来支持多路复用和服务器推送


### 图示
![](http://picbed.cc12703.com/20230123231500.png)



### 要点
* 所有通信都在一个TCP连接上进行
	* 承载多条`数据流`
* 每条`数据流`都是一条双向字节流
	* 唯一标识符和优先级信息
	* 承载多条`消息`
* 每条`消息`都是一条逻辑HTTP消息（请求或响应）
	* 包含一个`HEADER帧`、多个`DATA帧`
* `帧` 是最小的通信单位
	* 每个帧都包含帧头
	* 承载着特定类型的数据






## 头部压缩

### 概述
* 使用HPACK算法
	* 客户端和服务器建立字典，用索引号表示重复的字符串
	* 采用哈夫曼编码来压缩整数和字符串



## 二进制格式

### 帧

#### 格式
![](http://picbed.cc12703.com/20230124000827.png)

* 帧类型
	* 数据帧：HEADERS帧、DATA帧
	* 控制帧：SETTING帧、PING帧、PRIORITY帧
* 帧标志：携带一些简单的控制信息
	* END_HEADERS：头数据结束
	* END_STREAM：单向数据发送结束


### 流

* 含义：二进制帧的双向传输序列

#### 特点
* 流可并发：实现多路复用
* 客户端、服务器都可以创建流
* 流是双向的：客户端和服务器都可以发送、接收数据帧
* 流之间彼此独立
* 流可以设置优先级
* 流ID不能重用，只能顺序递增
* RST_STREAM帧可以随时终止流


### 图示

![](http://picbed.cc12703.com/20230124004717.png)


![](http://picbed.cc12703.com/20211026171148.png)








## 参考资料
* [HTTP/2中的帧定义](https://halfrost.com/http2-http-frames-definitions/)
* [JavaScript Guidebook HTTP](https://tsejx.github.io/javascript-guidebook/computer-networks/http)