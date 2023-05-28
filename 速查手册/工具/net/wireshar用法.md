

# wireshark用法


## 过滤器表达式



### 协议类型
* `tcp`  只显示tcp协议的数据流
* `http` 只显示http协议的数据流
* `dns`  只显示dns协议的数据流

### IP地址
* `ip.addr == xxx` 只显示地址为xxx的数据流
* `ip.src == xxx`  只显示源地址为xxx的数据流
* `ip.dst == xxx`  只显示地址为xxx的数据流

### 端口号
* `tcp.port == xxx` 只显示端口号为xxx的tcp数据流
* `tcp.srcport == xxx` 只显示源地址的端口号为xxx的数据流
* `upd.port == xxx` 只显示端口号为xxx的upd数据流
* `tcp.port in {80 443 8080}` 只显示端口号为80,443,8080的tcp数据流

### HTTP协议
* `http.request.method == "GET"`  只显示get请求
* `http.request.url contains admin` 只显示url中包含admin的请求
* `http.request.code == 404`  只显示状态码为404的请求
* `http.content_encoding == "gzip"` 只显示带压缩的请求

### 其他
* `and` 与
* `or`  或
* `in { xxx xxx }` 在成员组内