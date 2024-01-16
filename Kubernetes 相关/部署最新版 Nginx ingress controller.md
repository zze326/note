# 下载
部署 yaml 下载：
-  git 文件地址：[https://github.com/kubernetes/ingress-nginx/blob/main/deploy/static/provider/cloud/deploy.yaml](https://github.com/kubernetes/ingress-nginx/blob/main/deploy/static/provider/cloud/deploy.yaml)
- 裸文件下载地址：[https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.5.1/deploy/static/provider/cloud/deploy.yaml](https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.5.1/deploy/static/provider/cloud/deploy.yaml)
# 修改
有如下几处可按需修改
1. `hostNetwork`：使用宿主机的网络；
2. `nodeSelector`：添加标签选择器；
3. `DaemonSet`：修改 `Deployment` 为 `DaemonSet`；
4. 删除原有名为 `ingress-nginx-controller` 的 `Service`；
如：
```yaml
apiVersion: apps/v1
kind: DaemonSet  # 这里把 Deployment 改成 DaemonSet
metadata:
  labels:
    ...
  name: ingress-nginx-controller
  namespace: ingress-nginx
spec:
  minReadySeconds: 0
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app.kubernetes.io/component: controller
      app.kubernetes.io/instance: ingress-nginx
      app.kubernetes.io/name: ingress-nginx
  template:
    metadata:
      labels:
        app.kubernetes.io/component: controller
        app.kubernetes.io/instance: ingress-nginx
        app.kubernetes.io/name: ingress-nginx
    spec:
      hostNetwork: true  #这里加一句
      nodeSelector:
        kubernetes.io/os: linux
        hasIngress: "true"  # 在存在这个标签的 node 上部署
      containers:
      - args:
        ...
```