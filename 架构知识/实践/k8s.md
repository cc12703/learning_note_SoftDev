

# k8s


[TOC]

## 概述

### 功能
* 一个基于容器的分布式架构解决系统
* 自动的部署、扩容、管理分布式应用


### 思想
* 资源模型：将一切视为资源
* 控制器模式：每种资源都有`期望状态`和`实际状态`，由对应的控制器监视、调整
* 应用模型: 由标签和标签选择器共同组成



## 架构

### 系统构成
* 管理节点
* 工作节点
* 键值存储系统(etcd)

#### 架构图示
![](https://raw.githubusercontent.com/cc12703/picbed/main/20250902180700317.png)

### 管理节点
* 集群的控制节点（控制平面），是整个系统的大脑
* 通常需要3台服务器，保证高可用性

#### 组件
* `api-server`：提供API服务，负载处理请求
* `scheduler`：负责`Pod`调度
* `controller-manager`：负责管理多个控制器
* `etcd`：存储集群的数据


### 工作节点
* 提供运行容器所需要的资源和环境

#### 组件
* `kubelet`：负责管理`Pod`和容器的生命周期
* `kube-proxy`：负责集群内部的网络代理、负载均衡
* `container-runtime`：运行和管理容器




## 核心资源

### 关系图示
![](https://raw.githubusercontent.com/cc12703/picbed/main/20250902182743655.png)

### Pod
* k8s中的最小可部署单元
* 组成：一个Pause容器 + 多个业务容器
* 共享：网络栈和挂载卷
* 每个Pod都有一个唯一的IP地址
* 两个Pod之间可以直接进行网络通信


### Deployment
* 部署、管理应用程序
* 监控相关Pod的状态

### Service
* 定义一组Pod的访问方式
* 有唯一的名字
* 有固定的入口：虚拟IP和端口

#### 虚拟IP（Cluster IP）
* 全局唯一的
* 在Service的整个生命周期内都不会变动


### Namespace
* 将集群划分为多个独立的工作环境
* 资源是相互隔离的



## Pod


### 重启策略
* `Always`: 容器失效时，自动重启
* `OnFailure`: 容器中止且退出码不为0时，自动重启
* `Never`：不自动重启

### 健康检查
* `livenessProbe`: 存活探针, 判断容器是否存活
* `readinessProbe`：就绪探针， 判断容器服务是否可用
* `startupProbe`：启动探针，判断容器服务是否已启动

#### 检查方式
* `ExecAction`：能够在容器内执行一条命令，且返回码为0
* `TCPSocketAction`：能够对容器建立起TCP连接
* `HTTPGetAction`：能够对容器发起HTTP Get请求，且返回码在200 - 400之间



### 工作负载

#### 概述
* 用于更高层次地创建、管理Pod
* 无状态应用部署：`Deployment`
* 有状态应用部署：`StatefulSet`
* 节点级别的守护应用：`DaemonSet`
* 一次性任务：`Job`
* 定时任务：`CronJob`


#### Deployment
* 副本管理：保证指定数量的Pod在集群中运行
* 滚动更新：逐步用新版本的Pod替换旧版本的Pod
* 版本回滚：快速恢复到先前的可用版本


#### DaemonSet
* 在每个节点上都运行一个Pod
* 节点变更时，会自动启动、删除Pod
* 用途：运行日志采集程序、监控代理程序、存储组件、网络插件


### HPA
* 自动扩缩容：基于CPU使用率的自动Pod扩缩容功能

#### 原理
1. 通过`Metric Server`持续采集Pod副本的指标数据
1. HPA使用指标数据，通过扩缩容规则，计算出目标Pod副本数量
1. 若目标数量与当前数量不符，则通过`ReplicaSet`进行扩缩容


## Service

### 功能
* 为Pod提供一个稳定、统一的访问入口
* 负载均衡：将请求分发到多个Pod上
* 服务发现：自动维护Pod IP的变化

### 公开类型
* `ClusterIP`：分配一个虚拟IP地址，通过该IP来访问
* `NodePort`：在每个节点上开发一个固定端口，通过节点ID和端口号来访问
* `LoadBalancer`：使用云服务商创建的负载均衡器
* `ExternalName`：将Service名称映射到外部域名

### Endpoints
* 动态维护后台Pod的变化
* 确保请求被转发到正确的Pod


#### 图示
![](https://raw.githubusercontent.com/cc12703/picbed/main/20250909171503888.png)


### 服务发现

#### 环境变量
* 虚拟地址：`<service_name>_SERVICE_HOST`
* 端口号：`<service_name>_SERVICE_PORT`


#### DNS
* 默认使用`CoreDNS`，解析service名称
* 格式：`<service_name>.<namespace>.svc.cluster.local`


### 流量转发
* 实现：`kube-proxy`使用主机上的`iptables`和`ipvs`技术



## Ingress

### 功能
* 管理HTTP、HTTPS流量
* 基于应用层信息将流量转发给不同服务

#### 图示
![](https://raw.githubusercontent.com/cc12703/picbed/main/20250909174718971.png)


### 实现
* 通过7层负载均衡技术实现（Nginx, HAProxy, Kong）


### 原理
* `nginx-controller`：控制器进程，动态感知Ingress对象的变化，生成nginx配置文件
* `nginx`: 服务进程，将外部请求转发到Pod




## Volume

### 功能
* 用于在Pod中存储和共享数据
* 将存储设备挂载到Pod中


### emptyDir
* 用于Pod中容器之间的数据共享
* 生命周期与Pod一致

#### 实现图示
![](https://raw.githubusercontent.com/cc12703/picbed/main/20250909180532255.png)


### hostPath
* 用于将主机上的目录、文件挂载到Pod中


### nfs
* 用于将NFS服务器上的共享目录挂载到Pod中

#### 实现图示
![](https://raw.githubusercontent.com/cc12703/picbed/main/20250909180911856.png)


## PV

### 功能
* 职责分离：将外部存储配置交由其他工程师完成

### 组成
* PV（PersistentVolume）：管理持久性存储，屏蔽存储系统的细节
* PVC（PersistentVolumeClaim）：声明对持久性存储的需求
* VolumeController：监控PVC对象，确保匹配到合适的PV


### 动态供给
* 在需要自动创建PV

#### 组件
* `StorageClass`：资源对象，定义持久性存储的属性
* `Provisioner`：控制器，监控PVC对象，调用API创建符号需求的PV

#### 图示
![](https://raw.githubusercontent.com/cc12703/picbed/main/20250909182033752.png)


### 生命周期
* `Provisioning`：供给，静态、动态创建
* `Binding`：绑定，该PV与PVC绑定
* `Using`：使用，Pod获取到PV提供的存储资源
* `Releasing`：释放，绑定的PVC被删除
* `Reclaiming`：回收，根据回收策略执行不同的操作


## 内置存储对象

### ConfigMap
* 用于存储配置信息的资源对象
* 使用键值对保存数据

#### 自动更新
* `kubelet`会定期检查该对象，若发生变更，则自动同步到Pod中


### Secret
* 用于存储敏感信息的资源对象

#### 数据类型
* `docker-registry`：镜像仓库的认证凭据
* `generic`：任意格式数据，密码、密钥
* `tls`：TLS证书



## StatefulSet

### 特点
* 稳定、唯一的网络标识符
* 稳定、独享的持久存储
* 有序的部署、扩展、升级

#### 网络标识符
* Pod名称：`<StatefulSet名称>-<序号>`
* Pod域名：`<Pod名称>.<Service名称>.svc.cluster.local`

#### 独享存储
* 通过`volumeClaimTemplates`为每个Pod创建一个独立PVC


### Operator
* 用于部署、配置、管理有状态的应用程序
* 组件：CRD、控制器

#### CRD
* 自定义资源定义：创建应用程序的配置

#### 图示
![](https://raw.githubusercontent.com/cc12703/picbed/main/20250909184900517.png)




## 调度管理

### 节点选择器
* 将Pod调度到具有特定标签的节点上


#### 图示
![](https://raw.githubusercontent.com/cc12703/picbed/main/20250909185538013.png)


### 节点亲和性
* 与节点选择器类似
* 更精细地定义与节点的关联性

#### 类型
* 硬性条件：Pod必须被调度到满足条件的节点上
* 偏好条件：Pod尽量被调度到满足条件的节点上


### Pod亲和性、反亲和性
* 用于控制Pod与同一节点上其他Pod的关系
* 支持：硬性条件, 偏好条件

#### 亲和性
* 尽量将多个Pod调度到同一个节点上


#### 反亲和性
* 尽量避免将多个Pod调度到同一个节点上
* 用途：将应用副本的Pod调度到不同的节点上


### 污点和容忍
* 污点：节点上的标记，表明只有符合特定条件的Pod才能被调度上
* 容忍：定义Pod对节点上污点的容忍规则

#### 设置污点
* `kubectl taint node <node-name> <key>=<value>:<effect>`



## 安全管理

### 认证流程
* 认证：验证身份合法性
* 鉴权：验证用户是否有权进行特定操作
* 准入控制：对请求进行验证或修改

#### 图示
![](https://raw.githubusercontent.com/cc12703/picbed/main/20250909231934713.png)

#### 认证方式
* 客服端证书
* Token
* 用户名 + 密码

### RBAC

#### 资源对象
* Role：角色
* ClusterRole: 集群角色
* RoleBinding：角色绑定
* ClusterRoleBinding: 集群角色绑定


#### 图示
![](https://raw.githubusercontent.com/cc12703/picbed/main/20250909233106368.png)

#### 主体类型
* 用户：一个具体的用户
* 组：一组用户的集合
* 服务账号：Pod中应用程序的身份


### Pod安全上下文

#### 功能
* 用于配置、控制Pod运行时的安全属性和限制

#### 例子
* 容器以普通用户运行
* 容器启动特权
* 容器设置为只读文件系统


### 网络策略

#### 要点
* 策略实现依赖于网络插件（Calico, Cilium）
* 默认情况下，Pod之间没有任务限制


#### 资源对象
* `NetworkPolicy`
* `podSelector`：选择要应用策略的Pod
* `policyTypes`: 策略类型
* `ingress`：入站流量规则列表
* `egress`：出站流量规则列表


## 网络

### 网络插件
* 都遵循CNI接口（container network interface）
* 每个Pod拥有集群中的唯一IP地址
* Pod能与其他所有节点上的Pod进行通信
* 节点网络能与集群中的所有Pod进行通信


### Calico插件

#### 结构
* 每个节点都运行一个`calico-node`的Pod
* `Felix`进程：配置、管理节点上的路由表、策略规则
* `Bird`进程：BGP协议的守护进程，学习其他节点的路由表
* `Confd`进程：配置管理工具


#### 工作模式
* `VXLAN`：覆盖网络，将IP包封装在底层网络的UDP包中
* `IPIP`：覆盖网络，将IP包封装在底层网络的IP包中
* `BGP`：基于路由直接实现Pod间的通信
