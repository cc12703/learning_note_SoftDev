

# Docker命令


## 指令速查

### 进入容器调试
* `docker exec -it <container> /bin/bash`
* `docker exec -it <container> /bin/sh`


## 基础指令

### 生命管理

* `docker start` 启动已停止容器
* `docker stop` 停止容器
* `docker restart` 重启容器

* `docker run` 创建并启动容器
    * `-d` 后台运行
    * `--rm` 停止后自动删除容器
    * `--it` 交互模式 + 伪终端
* `docker kill` 强制停止容器
* `docker rm` 删除容器

### 容器操作

* `docker logs` 查看容器日志
* `docker inspect` 查看容器详情
* `docker stats` 查看容器资源使用
* `docker top`   查看容器内进程

### 镜像管理

* `docker pull` 拉取镜像
* `docker images` 列出本地镜像
* `docker rmi` 删除镜像

* `docker image ls` 列出本地镜像
* `docker image rm` 删除镜像
* `docker image prune` 清理未使用镜像

### 卷管理

* `docker volume ls` 列出数据卷
* `docker volume inspect` 查看卷详情
* `docker volume rm` 删除卷
* `docker volume prune` 清理未使用卷

### 系统管理

* `docker system df` 查看磁盘使用情况
* `docker system prune` 清除未使用的数据




