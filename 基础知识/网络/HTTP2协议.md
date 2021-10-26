
# HTTP2协议



## 特性
* 减少传输的数据量
* 支持多路复用
* 服务器消息推送


## 核心改进
* 加入二进制分帧层，改变了HTTP消息的传输
* HTTP语义都不改变


## 二进制分帧

### 概述
* 所有通信都在一个TCP连接上进行
	* 承载多条数据流
* 每条数据流都是一条双向字节流
	* 唯一标识符和优先级信息
	* 承载多条消息
* 每条消息都是一条逻辑HTTP消息（请求或响应）
	* 包含一个HEADER帧、多个DATA帧
* 帧是最小的通信单位
	* 每个帧都包含帧头
	* 承载着特定类型的数据

### 图示
![](https://gitee.com/cc12703/figurebed/raw/master/img/20211026171148.png)



## 标头压缩

### 概述
* 使用HPACK格式压缩标头元数据
* 使用静态霍夫曼编码
* 需要双方缓存一份头域索引表




## 参考资料
* [HTTP/2中的帧定义](https://halfrost.com/http2-http-frames-definitions/)
* [JavaScript Guidebook HTTP](https://tsejx.github.io/javascript-guidebook/computer-networks/http)