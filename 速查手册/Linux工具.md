
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