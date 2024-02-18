


# SDP


## 概述
* 一种会话描述格式，适用于不同的传输协议
* 一种基于文本的协议，由多个文本行组成



## 格式 
* 文本行格式：`<类型> = <值> [CRLF]`

### 类型
* `v` 协议版本：
    * `<version>`
* `o` 创建者
    * `<username> <address> <session-id>`
* `s` 会话名称
    * `<session-name>`
* `i` 会话信息
    * `<session desc>`
* `u` URL描述
* `e` Email
* `c` 连接信息
    * `<network-tyoe> <address-type> <connection-address>`
* `b` 带宽信息