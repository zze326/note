# 资源名称缩写对照表
| 全名 | 缩写名 |
| ---- | ---- |
| pod | po |
| deployment | deploy |
| service | svc |
| statefulset | sts |
| configmap | cm |
| namespace | ns |
| endpoint | ep |
# 常用命令
```bash
# 查看 Kubernetes 版本信息
kubectl version
# 查看集群信息
kubectl cluster-info
# 查看集群中所有节点
kubectl get no
# 查看节点详细描述信息
kubectl describe no <node-name>
# 查看所有命名空间
kubectl get ns
# 查看 Pod 列表
kubectl get po
# 查看 Pod 详细描述信息
kubectl describe pod <pod-name>
# 查看 Pod 日志
kubectl logs po <pod-name> # 当只有一个容器时
kubectl logs po <pod-name> -c <container-name> # 查看指定容器日志
# 进入 Pod 终端
kubectl exec -it <pod-name> -- <command> # 当只有一个容器时
kubectl exec -it <pod-name> -c <container-name> -- <command> # 进入指定容器
# 查看当前命名空间中的 Service 列表
kubectl get svc
# 查看 Service 详细描述信息
kubectl describe svc <service-name>
# 查看当前命名空间中的 Deployment 列表
kubectl get deploy
# 查看 Deployment 详细描述信息
kubectl describe deploy <deployment-name>
# 重启 Deployment
kubectl rollout restart deploy <deployment-name>
# 查看 Deployment 的滚动发布状态
kubectl rollout status deploy <deployment-name>
# 查看 Deployment 滚动发布的历史记录
kubectl rollout history deploy <deployment-name>
# 查看当前命名空间中的 StatefulSet 列表
kubectl get sts
# 查看 StatefulSet 详细描述信息
kubectl describe sts <stateful-set-name>
# 临时运行一个测试 Pod
kubectl run -it --rm --restart=Never --image=busybox test-pod -- /bin/sh
# 清空指定节点的 Pod
kubectl drain <node-name> --ignore-daemonsets
# 封锁 Node 以禁止新 Pod 调度到指定 Node
kubectl cordon <node-name>
# 接触封锁
kubectl uncordon <node-name>
# 查看 Pod 内存和 cpu 情况
kubectl top po
# 查看 Node 内存和 cpu 情况
kubectl top no
# 查看当前命名空间中的 Endpoint 列表
kubectl get ep
```