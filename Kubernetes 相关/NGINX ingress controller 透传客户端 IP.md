修改 NGINX ingress controller 使用的 configmap：
```sh
$ kubectl edit cm -n ingress-nginx ingress-nginx-controller
apiVersion: v1
data:
  compute-full-forwarded-for: "true"
  forwarded-for-header: X-Forwarded-For
  use-forwarded-headers: "true"
kind: ConfigMap
metadata:
  name: ingress-nginx-controller
  namespace: ingress-nginx
```
- `compute-full-forwarded-for = true`
这个选项决定了 NGINX 是否应该计算完整的 X-Forwarded-For（XFF）头。默认情况下，NGINX 只会信任来自特定可信代理的 XFF 头，并将这些信息附加到它自己的记录中。但是，当设置了 `compute-full-forwarded-for = true` 时，NGINX Ingress Controller 会尝试解析并合并所有在 X-Forwarded-For 头中的IP地址，创建一个代表整个转发链的完整列表。

**用途：确保即使经过多个代理层后，仍然可以准确地追踪到原始客户端的 IP 地址（将客户端用户访问所经过的代理ip按逗号连接的列表形式记录下来）。**
注意事项：必须小心使用此选项，因为它可能会引入安全风险，例如允许伪造的 XFF 头欺骗系统。通常需要结合可信代理名单一起使用。

-  `forwarded-for-header = X-Forwarded-For`
这个配置指定了用于存储客户端 IP 地址的 HTTP 请求头名称，默认是 `X-Forwarded-For`。在某些环境中，你可能想要自定义这个头部的名字，或者适应不同的中间件/代理所使用的标准。

**用途：指定哪个 HTTP 请求头用来携带客户端的真实 IP 地址信息。**
注意事项：如果你改变了这个值，确保所有的相关组件（如日志分析工具、应用程序代码等）都知道并且能够正确处理新的头部名称。

- `use-forwarded-headers = true`
启用此选项后，NGINX Ingress Controller 将信任并使用传入请求中的 `X-Forwarded-*` 头部信息（包括但不限于 `X-Forwarded-For`），而不是依赖于直接连接到它的客户端的信息。这对于部署在云服务提供商后面或通过其他类型的反向代理访问的应用程序特别有用。

**用途：让 NGINX Ingress Controller 正确解析和应用由上游代理添加的 X-Forwarded-* 头部。**
注意事项：同样需要注意安全性问题，因为这使得 NGINX 相信任何提供给它的 X-Forwarded-* 头部内容。因此，应当仅在受控环境下启用，并且要确保只有可信赖的代理能够设置这些头部。