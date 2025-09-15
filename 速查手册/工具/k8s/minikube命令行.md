


# minikube命令行


## 集群管理
* `minikube start`  创建并启动
* `minikube stop`   停止
* `minikube status` 查看状态
* `minikube delete` 删除集群

* `minikube profile list`    查看所有集群
* `minikube profile <name>`  切换到指定集群


```bash
 # 使用国内源
 minikube start  --nodes=3  --driver=docker \
       --registry-mirror=https://docker.m.daocloud.io   \
       --image-mirror-country=cn \
       --image-repository=registry.cn-hangzhou.aliyuncs.com/google_containers
```


## 节点管理
* `minikube node add` 添加节点
* `minikube node list` 查看所有节点
* `minikube node delete <name>` 删除节点