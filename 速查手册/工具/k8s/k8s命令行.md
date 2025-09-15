

# k8s命令行


## 总体
* `kubectl apply -f xxx.yaml`  应用配置文件


## 命名空间
* `kubectl get ns` 获取所有
* `kubectl create ns <name>` 创建
* `kubectl delete ns <name>` 删除

## 上下文
* `kubectl config get-contexts` 查看当前
* `kubectl config set-context --current --namespace <name>` 切换到指定空间

## 资源
* `kubectl get all`  查看所有


## Pod管理
* `kubectl get pods` 查看

* 查看日志：`kubectl logs <pod-name>`
* 查看特定容器日志：`kubectl logs <pod-name> -c <container-name>`
* 执行命令：`kubectl exec --it <pod-name>  -- <cmd>`
* 执行特定容器命令：`kubectl exec --it <pod-name> -c <container-name> -- <cmd>`
* `kubectl run <pod-name> --image=<url>` 创建
* `kubectl delete pod <pod-name>`  删除


## 权限管理
* `kubectl get clusterroles` 查看内置角色
* `kubectl get role -n <ns>`  查看命令空间的角色