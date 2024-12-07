在 ACK 中创建 `LoadBalancer` 类型的 `Service` 时默认会自动创建一个关联的 lb，如果需要绑定已有的 lb 可参考如下配置：
```bash
apiVersion: v1
kind: Service
metadata:
  annotations:
    # 指定使用的是内网 lb  外网不需要指定
    service.beta.kubernetes.io/alicloud-loadbalancer-address-type: intranet
    # 绑定的 lb id
    service.beta.kubernetes.io/alibaba-cloud-loadbalancer-id: "<lb id>"
    # 配置是否覆盖已有的监听
    service.beta.kubernetes.io/alicloud-loadbalancer-force-override-listeners: 'true'
  name: <service-name>
spec:
  ports:
  # 此 port 会直接监听到 lb
  - port: <port> 
    protocol: TCP
    targetPort: 8080
  selector:
    app: <service_name>
  type: LoadBalancer
```
> 更多可参考：https://help.aliyun.com/document_detail/420269.html。