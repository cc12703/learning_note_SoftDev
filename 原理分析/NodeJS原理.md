

# NodeJS原理

## 概述

### JS特点
* JS是同步的
* JS是阻塞的
* JS是单线程的


## 运行时

### 组件
* 外部依赖库：v8引擎，Libuv, crypt 等等
* C++功能库：文件操作、网络访问
* JS库：包装C++的功能库

### 图示
![](http://picbed.cc12703.com/20230509134133.png)


### Libuv
* 一个跨平台的开源库
* 提供对异步操作的支持



## 事件循环

### 消息队列
* 定时器队列：保存setTimeout, setInterval相关的回调
* IO队列：保存fs, http模块相关的回调
* 检查队列：保存setImmediate相关的回调
* 关闭队列：保存异步任务关闭事件相关的回调
* 微队列1：保存nextTick相关的回调
* 微队列2：保存promise相关的回调

### 执行逻辑
1. 执行微队列中的回调
1. 执行定时器队列中的回调
    a. 执行每个回调后，立刻执行微队列中的回调
1. 执行IO队列中的回调 
    a. 执行每个回调后，立刻执行微队列中的回调
1. 执行检查队列中的回调
    a. 执行每个回调后，立刻执行微队列中的回调
1. 执行关闭队列中的回调
    a. 执行每个回调后，立刻执行微队列中的回调


### 图示
![](http://picbed.cc12703.com/20230509134844.png)






## 参考资料
* [A Complete Visual Guide to Understanding the Node.js Event Loop](https://www.builder.io/blog/visual-guide-to-nodejs-event-loop)