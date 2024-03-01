
# Linux工具

[TOC]


## crontab
* 功能：创建、修改定期执行的程序
* 格式：`crontab <option>`

### option
* `-e` 执行文字编辑器来设置日程表
* `-l` 列出目前的日程表
* `-r` 删除目前的日程表


### 配置

#### 格式
`min hour day month week command`
* min 分钟（0 -- 59）
* hour 小时（0 -- 23）
* day 天 （1 -- 31）
* month 月 （1 -- 12）
* week 星期（0 -- 6），0表示星期天

#### 值
* `*` 指取值范围内的所有值
* `/` 指每
* `-` 指连续范围内的值
* `,` 值分离的值

#### 示例
```cron
5 * * * * ls  #每个小时的第5分钟执行一次ls命令
*/3 * * * * ls #每隔3分钟执行一次ls命令
0 6-12/3 * 12 * ls  #在12月中，每天的6点到12点，每隔3个小时的第0分钟执行一次ls命令
```

### 环境变量
* 需要添加在`~/.bash_profile`中才会生效
* 需要在功能脚本中增加`. ~/.bash_profile`


### 日志

#### 开启
1. 命令：`sudo vim /etc/rsyslog.d/50-default.conf`
1. 去除 `cron.*` 前面的注释
1. 命令：`sudo service rsyslog restart`
1. 命令：`sudo apt-get install -y postfix`

#### 查看
* cron本身的日志：`tail -f /var/log/cron.log`
* 执行的命令的日志：`cat /var/mail/[username]`



## curl
* 功能：发出网络请求，提取数据并在标准输出上显示
* 格式：`curl <option> url`

### option
* `-s` 不输出错误和进度信息
* `-L` 进行自动跳转
* `-I` 只显示HTTP响应头信息
* `-X` 指定HTTP动词
* `-H` 增加HTTP头信息
	* 格式："<name>:<value>"
* `-d` 发送POST请求的数据体
* `--data-urlencode` 等同于`-d`，会对数据进行URL编码
* `-G` 用于构造URL的查询字符串，需要和`-d`一起使用
* `-x` 指定请求代理

### 示例
```sh

#post json数据
curl -H "Content-type:application/json" -X POST -d '{ ... }' http://google.com/login


curl -G -d 'g=kitties' -d 'count=20' http://google.com/search
#out: http://google.com/search?q=kitties&count=20
```



## iptables
* 功能：添加、删除防火墙的策略规则
* 格式：`iptables [-t table] COMMAND [chain] CRETIRIA -j ACTION`

### table（规则表）
* `filter`：过滤规则表（默认表）
* `nat`：地址转换规则表
* `mangle`：修改数据标记位规则表
* `raw`：跟踪数据表规则表

### COMMAND（子命令）
* `-P`：设置默认策略
* `-A`：添加规则
* `-D`：删除规则
* `-I`：插入规则
* `-F`：清空规则
* `-L`：列出规则


### CRETIRIA（匹配参数）
* `-s` 匹配源地址
* `-d` 匹配目标地址
* `-i` 匹配入站网卡接口
* `-o` 匹配出站网卡接口
* `-p` 匹配协议
* `-dport` 匹配目标端口号
* `-sport` 匹配源端口号
* `--src-range` 匹配源地址范围
* `--dst-range` 匹配目标地址范围
* `--mac-source`  匹配源MAC地址

### chain（链表）
* `PREROUTING`：路由前过滤
* `POSTROUTING`：路由后过滤
* `FORWARD`：转发数据过滤
* `INPUT`：入站数据过滤
* `OUTPUT`：出站数据过滤

### ACTION（触发动作）
* `ACCEPT`：允许包通过
* `REJECT`：拒绝包通过
* `DROP`：丢弃包
* `DNAT`：目标地址转换，目的地址转换，让外网机器可以访问内网
* `SNAT`：源地址转换，让内网机器可以访问外网
* `MASQUERADE`：动态伪装，自动寻找外网地址并改为正确的外网地址
* `REDIRECT`：重定向



### 用法示例

* `iptables –A INPUT –i lo –j ACCEPT` 
	接受所有来自于lo网卡的数据包
* `iptables -A INPUT -i eth0 -s 192.168.1.10 -j DROP`  
	丢弃所有来自于eth0网卡的源IP为192.168.1.10的数据包
* `iptables –A INPUT –i eth0 –p udp –dport 137:138 –j ACCEPT` 
	接收来自于UDP端口137，138的数据包
* `iptables -A INPUT -m mac --mac-source 00:0C:29:56:A6:A2 -j ACCEPT`
	接受来自于MAC地址00:0C:29:56:A6:A2的数据包

* `iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-ports 8080`
	将80端口的数据包转递到8080端口

* `iptables –t nat –A POSTROUTING –s 192.168.10.10 –o eth1 –j SNAT --to-source 111.196.221.212`
	将192.168.10.10转换为111.196.211.212

* `iptables -t nat -A POSTROUTING -s 192.168.10.0/24 -j MASQUERADE`
	使用动态伪装

* `iptables –t nat –A PREROUTING –i eth1 –d 61.240.149.149 –p tcp –dport 80 –j DNAT --to-destination 192.168.10.6`
	将61.240.149.149转换为192.168.10.6



### 方案示例

#### 网关
* `net.ipv4.ip_forward = 1` 打开内核路由转发
* `iptables -t nat -A POSTROUTING -s 192.168.30.0/24 -d 192.168.40.0/24 -o eth1 -j MASQUERADE`
	将源30网段，转换成40网段，从eth1中发出
* `iptables -t nat -A PREROUTING -s 192.168.40.0/24 -d 192.168.30.0/24 -o eth0 -j MASQUERADE`
	将源40网段，转换成30网段，从eth0中发出




## tcpdump
* 功能：用于网络抓包


### 通用
* `-i` 指定网络接口
* `-w` 将输出重定向到文件
* `-c` 指定要捕获的报文数上限




## netstat

### 功能
1. 显示网络协议相关的统计信息
2. 检验端口的网络连接情况

### 参数
* `-a` 显示所有连接
* `-t` 显示tcp连接
* `-u` 显示udp连接
* `-r` 显示路由信息  

* `-n` 显示IP地址和端口信息
* `-p` 显示使用socket的程序名称和识别码


### 示例
* `netstat -tlnp` 显示TCP连接的监听地址、端口和程序信息
* `netstat -tn | grep -v ESTABLISHED` 查看非正常连接
* `netstat -t | wc -l` 统计TCP连接数


## systemctl

### 功能
* 检查和控制各种系统服务

### 用例
* `systemctl enable xxx.service` 激活服务、设为开机启动
* `systemctl disable xxx.service` 取消开机启动
* `systemctl start xxx.service` 启动服务
* `systemctl stop xxx.service`  停止服务
* `systemctl restart xxx.service`  重启服务
* `systemctl daemon-reload`  加载所有配置信息


## journalctl

### 功能
* 查看系统日志和各个服务日志

### 用例
* `journalctl -r -u xxx.service` 查看指定服务的日志，最新的日志在前面
* `journalctl -b` 查看本次启动的日志
* `journalctl -f` 实时显示最新日志
