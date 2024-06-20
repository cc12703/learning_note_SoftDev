



# Redis命令行



## 登录
格式：`redis-cli -c <ip>`

### 集群命令
* 获取节点信息：`cluster nodes`
* 加入节点：`cluster meet <ip> <port>`
* 移除节点：`cluster forget <node-id>`


## 集群操作
格式：`redis-cli --cluster xxx`

### 子命令
* 创建主节点：`create <host:port> ...`
* 创建主从节点：`create <host:port> --cluster-replicas 1`
    * 1 表示一个主节点需要一个从节点
* 添加主节点：`add-node <new-host:port> <exists-host:port>`
* 添加从节点：`add-node <new-host:port> <exists-host:port> --cluster-slave --cluster-master-id <node-id>`

* 检查状态：`check <host:port>  --cluster-search-multiple-owners`
* 获取信息：`info <host:port>`