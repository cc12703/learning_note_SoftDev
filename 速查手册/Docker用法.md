
# Docker用法

[TOC]

## Dockerfile语法

### 基础
* `#` 单行注释
* 部分：基础信息、镜像操作指令、容器启动指令


### 开始指令
* `FROM  <image>:<tag>` 指定要继承的镜像
* `FROM  <image>:<tag> AS <name>` 初始化一个新的构建阶段

#### 示例
```dockerfile
FROM node:12.22.1-alpine as build
```

### 基础信息
* `LABEL <key>=<val> <key>=<val>` 在镜像中增加元信息
* `ENV <key>=<val> <key>=<val>`  设置环境变量，在镜像构建、容器运行时有效
* `VOLUME path path` 创建一个挂载点
	* `VOLUME ['path','path']`
* `WORKDIR path` 设置当前的工作目录
* `EXPOSE <port> <port> <port>/<protocol>` 暴露端口号


#### 示例
```dockerfile

LABEL multi.label1="value1" multi.label2="value2" other="value3"
LABEL multi.label1="value1" \
      multi.label2="value2" \
      other="value3"

```

### 操作指令
* 定义参数变量，可以由命令行传入值
	* `ARG <name>`
	* `ARG <name>=<default>`  带默认值
	* ·`$<name>`  使用变量
* 执行命令
	* `RUN <command>` shell方式，会被翻译成 /bin/sh -c "<command>"
	* `RUN ['cmd', 'param1', 'param2']` exec方式
* `COPY <src> <src> <dst>`  从本地主机中拷贝目录、文件到容器中
* `COPY --from=<name> <src> <src> <dst>` 从前一个构建阶段中拷贝文件、目录
* `ADD <src> <src> <dst>` 从本地主机中拷贝目录、文件到容器中


### 启动指令
* 设置容器启动时一定要执行的命令
	* `ENTRYPOINT command param` shell方式
	* `ENTRYPOINT ['executable' , 'param']` exec方式
* 设置容器启动时要执行的命令，只能存在一条，可以由命令行覆盖
	* `CMD ['executable' , 'param']` exec方式
	* `CMD command param` shell方式


## Dockerfile最佳实践

### 减少构建时间
* 调整指令顺序，将容易变动的指令排列在后面
	* 原因：更好的利用编译缓存
	* 示例
		```dockerfile
		FROM debian
		RUN apt-get update
		RUN apt-get -y install openjdk-8-jdk ssh vim
		COPY . /app
		CMD ["java", "-jar", "/app/app.jar"]
		```
* 只COPY需要的文件，避免使用`COPY .`指令
	* 原因：更好的避免缓存被破坏
	* 示例
		```dockerfile
		FROM debian
		RUN apt-get update
		RUN apt-get -y install openjdk-8-jdk ssh vim
		COPY target/app.jar /app
		CMD ["java", "-jar", "/app/app.jar"]
		```
* 尽量合并RUN中的命令，减少不必要的RUN指
	* 原因：更好的利用缓存
	* 示例
		```dockerfile
		ROM debian
		RUN apt-get update \
			&& apt-get -y install openjdk-8-jdk ssh vim 
		COPY target/app.jar /app
		CMD ["java", "-jar", "/app/app.jar"]
		```

### 减少镜像大小
* 使用最小化的基础镜像
	* 示例
		```dockerfile
		FROM openjdk:8-jre-alpine
		COPY target/app.jar /app
		CMD ["java", "-jar", "/app/app.jar"]
		```
* 使用多阶段构建，移除构建依赖
	* 示例
		```dockerfile
		FROM maven:3.6-jdk-8-alpine AS builder
		WORKDIR /app
		COPY pom.xml .
		RUN mvn -e -B dependency:resolve
		COPY src ./src
		RUN mvn -e -B package

		FROM openjdk:8-jre-alpine
		COPY --from=builder /target/app.jar /
		CMD ["java", "-jar", "/app/app.jar"]
		```
* 移除不必要的依赖
	* 示例
		```dockerfile
		FROM debian
		RUN apt-get update \
			&& apt-get -y install --no-install-recommends \
			openjdk-8-jdk 
		COPY target/app.jar /app
		CMD ["java", "-jar", "/app/app.jar"]
		```
* 移除包管理器的缓存
	* 示例
		```dockerfile
		FROM debian
		RUN apt-get update \
			&& apt-get -y install --no-install-recommends \
			openjdk-8-jdk \
			&& rm -rf /var/lib/apt/lists/* 
		COPY target/app.jar /app
		CMD ["java", "-jar", "/app/app.jar"]
		```

## Docker命令

### 通用选项
* `--config` 指定客户端配置文件
* `-D`或`--debug` 打开调试模式
* `-H`或`--host` 指定服务器连接模式
	* `tcp://host:port`
	* `unix:///path/to/socket`
	* `fd://socketfd`

### 镜像管理
* `docker image build <option> path`  构建镜像
	* `--build-arg` 设置变量
	* `-f`或`--file` 指定dockerfile文件
	* `--label` 设置元数据
	* `-t`或`--tag` 设置镜像的tag名字
* `docker image ls <option>`  显示镜像
	* `-a`或`--all` 显示所有镜像，包括中间镜像
* `docker image prune <option>` 移除悬空的镜像
	* `-a`或`--all` 移除所有无用镜像
* `docker image rm <option> <image> ...` 移除指定镜像
* `docker image inspect <image>`  显示镜像的详细信息


### 容器管理
* `docker container create <option> image` 创建一个容器
	* `--env-file` 从文件中读取环境变量
	* `-t`或`--tty` 分配一个虚拟的TTY
	* `-i`或`--interactive` 保持标准输入打开
	* `--name` 容器名字
* `docker container exec <option> name command` 在运行中容器中执行命令
	* `-d`或`--detach` 在后台运行命令

### 卷管理
* `docker volume create <option> <name>` 创建卷
	* `-d`或`--driver` 指定卷驱动
	* `-o`或`--opt` 指定驱动选项
	* `--label` 设置元信息
* `docker volume ls` 显示卷
* `docker volume inspect <name>` 显示卷的详细信息
* `docker volume prune` 移除不再使用的本地卷

### 系统管理
* `docker system df`  显示磁盘使用量
* `docker system prune <option>`  清除悬空的资源
	* `-a`或`--all` 移除所有无用的资源	

