
# k8s配置语法


## 基本结构
* `apiVersion` API版本，不同的资源对象，版本也不一样
* `kind` 资源对象类型
* `metadata` 元数据
* `spec` 期望的资源状态


### metadata
* `name` 名字，集群内唯一
* `namespace` 命名空间
* `labels` 标签信息
* `annotations` 备注信息