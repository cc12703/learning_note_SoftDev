

# VSCode原理



## 概述


### 设计策略

#### 依赖注入
* 使用decorator标注元信息
* 角色：Service(服务的实现)，Interface(服务的接口描述)，Client(服务的使用方)，Manager(服务的管理器)


#### 命令系统
* 中心化的，各个功能模块变成扁平化的结构

![](http://picbed.cc12703.com/20230528233259.png)


#### 插件模型
* 进程隔离：所有插件运行在一个独立进程中
* 界面渲染隔离：插件无法更改界面DOM，所有用户操作被转化为请求发送给插件


### 核心组件

#### Electron
* 以Node.js作为运行时，Chromium作为渲染引擎
* 用于开发跨平台的GUI程序

#### Monaco Editor
* 基于浏览器的代码编辑器

#### Xterm.js
* 前端组件，在浏览器中实现了完整的终端功能

#### TypeScript
* 开发语言，JavaScript的严格超集

#### LSP
* 语言服务器协议（Language Server Protocol）
* 编辑器与语言服务器之间的一种协议
* 通过JSON-RPC传输消息

#### DAP
* 调试适配协议（Debug Adapter Protocol）
* 开发工具与调试工具之间的一种协议
* 通过JSON传输数据




### 架构
![](http://picbed.cc12703.com/20230528180513.png)

* 主进程：入口进程，负责一些全局任务
* 渲染进程：负责Web页面的渲染
* 插件宿主进程：负责运行插件代码



## 代码结构

### 分层模块化

![](http://picbed.cc12703.com/20230528233942.png)

```
/src/vs：         分层和模块化的 core
/src/vs/base:     通用的公共方法和公共视图组件
/src/vs/code:     VSCode 应用主入口
/src/vs/platform：可被依赖注入的各种纯服务
/src/vs/editor:   文本编辑器
/src/vs/workbench： 整体视图框架
/src/typings:       公共基础类型
/extensions：        内置插件
```

### 环境隔离

![](http://picbed.cc12703.com/20230528234051.png)

```
common:       公共的js方法
browser:      只使用浏览器API的代码，可以调用common
node:         只使用NodeJS API的代码，可以调用common
electron-browser: 使用electron渲染线程和浏览器 API 的代码，可以调用 common，browser，node
electron-main:    使用electron主线程和NodeJS API的代码，可以调用 common， node
test:             测试代码
```


## LSP




## 参考资料

* [从 VSCode 看大型 IDE 技术架构](https://zhuanlan.zhihu.com/p/96041706)
* [从 VSCode 源码中我看到的](https://zhuanlan.zhihu.com/p/631272487)
* [Visual Studio Code有哪些工程方面的亮点](https://zhuanlan.zhihu.com/p/35303567)
* [LSP协议](https://microsoft.github.io/language-server-protocol/)