

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

## date
* 功能：显示、设定系统的日期与时间

### 按格式显示
* 格式：`date '+format'`
* format元素
	* `%Y` 完整年份(0000 - 9999)
	* `%y` 年份的最后两位
	* `%m` 月份(01-12)
	* `%d` 日(01-31)
	* `%w` 一周中的第几天(0-6)
	* `%H` 小时(00-23)
	* `%I` 小时(01-12)
	* `%M` 分钟(00-59)
	* `%S` 秒(00-60)



## awk
* 用于处理文本文件
