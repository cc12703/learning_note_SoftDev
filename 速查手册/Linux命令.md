

# Linux命令

[TOC]


## find
* 功能：在指定目录下查找文件
* 格式：`find <path> <option> [ -exec  <command> {}\;]`

### option
* `-type <type>` 指定文件类型
	* `d` 目录
	* `f` 文件
* `-name <name>`  指定文件名，支持通配符
* `-iname <name>` 指定文件名，忽略大小写，支持通配符
* `-atime <day>`  指定访问时间
	* `+N` 最近N天前被访问过的文件
	* `-N` 最近N天内被访问过的文件
* `-mtime <day>`  指定修改时间
	* `+N` 最近N天前被修改过的文件
	* `-N` 最近N天内被修改过的文件
* `!` 否定关系

### 示例
```sh
# 在当前目录及其子目录下，查找所有后缀为.c的文件
find . -name "*.c"  

# 查找/var/log目录中更改时间在7日以前的普通文
find /var/log -type f -mtime +7 -exec rm {} \;

# 找出/home下不是以.txt结尾的文件
find /home ! -name "*.txt"
```


## tar
* 功能：用于建立和还原备份文件
* 格式：`tar <option> file/dir`

### option
* `-c` 创建文件
* `-x` 还原文件
* `-r` 新增文件追加到备份文件的尾部
* `-z` 使用gzip处理文件
* `-f <name>`  指定输出的备份文件
* `-C <dir>`   切换到指定目录
* `--remove-files` 文件加入后就删除

### 示例
```sh

tar -czf data.tar.gz -C ./data .
```

## sudo
* 功能：以root的身份执行命令
* 格式：`sudo <option> command`

### option
* `-E` 执行时保留当前用户已存在的环境变量
* `-b` 将要执行的命令放在后台中执行

### 执行流程
1. 读取解析`/etc/sudoers`文件，检查用户是否有权限
1. 提示用户输入密码、跳过密码验证
1. 创建一个子进程，调用`setuid()`切换到目标用户
1. 在子进程中执行shell命令

### 配置文件
* 文件：/etc/sudoers
	* 修改命令：`sudo visudo`
* 授权配置项
	* 格式：`USER/GROUP HOST=(USER[:GROUP]) [NOPASSWD:] COMMANDS`
	* USER/GROUP: 要被授权的用户或组
	* HOST：允许的主机，ALL表示任何主机
	* (USER[:GROUP])：可以切换到的用户或组，ALL表示可以切换到任何用户
	* NOPASSWD：跳过密码验证
	* COMMANDS：允许执行的命令，ALL表示允许执行所有命令



## nohup
* 功能：在后台不挂断地运行命令（退出终端不会影响程序运行）
* 格式：`nohup command <arg> &`

### 选项
* `&` 让命令在后台运行

### 示例
```sh

nohup /root/runoob.sh > runoob.log 2>&1 &
```


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