

# DockerCompose用法

[TOC]


## Compose文件语法

### 整体结构
```yml
version: "3.9"

services:    #定义服务
	servOne:
		xxxx

	servTwo:
		xxxx

networks:    #定义网络
	netOne:
	netTwo:

volumes:    #定义卷
	volOne:
	volTwo:
```

### build
* 功能：配置镜像构建
* 格式1：`build: <path>`
* 格式2：
	 ```yml
	build:
		context: <path>
      	dockerfile: <filename>
      	args:
        	buildno: <value>
	 ```

#### context
* 功能：指定包含dockerfile文件的目录
* 格式：
	```yml
	build:
  		context: <path>
	```

#### dockerfile
* 功能：指定要使用的dockerfile文件
* 格式：
	```yml
	build:
		context: <path>
		dockerfile: <filename>
	```

#### args
* 功能：添加构建参数
* 格式1：
	```yml
	build:
	context: <path>
	args:
		<name1>: <value1>
		<name2>: <value2>
	```

#### labels
* 功能：添加元数据