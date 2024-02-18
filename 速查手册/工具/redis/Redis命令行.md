



# Redis命令行



## 集群
格式：`redis-cli --cluster xxx`

### 操作
* 创建主节点：`create <host:port> ...`
* 创建主从节点：`create <host:port> --cluster-replicas 1`
    * 1 表示一个主节点需要一个从节点
* 添加主节点：`add-node <new-host:port> <exists-host:port>`
* 添加从节点：`add-node <new-host:port> <exists-host:port> --cluster-slave --cluster-master-id <node-id>`

* 检查状态：`check <host:port>  --cluster-search-multiple-owners`
* 获取信息：`info <host:port>`