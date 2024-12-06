> 官方文档：<https://www.elastic.co/guide/en/elasticsearch/reference/7.17/targz.html#targz-enable-indices>。

# 主机准备
我这里准备三台主机：
- es01：192.168.0.21；
- es02：192.168.0.22；
- es03：192.168.0.23；
# 部署
## 基础环境
> 下面操作需要在所有主机执行。

下载二进制包并配置环境变量：
```sh
$ wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.17.26-linux-x86_64.tar.gz
$ tar xf elasticsearch-7.17.26-linux-x86_64.tar.gz -C /opt/
$ cd /opt/ && ln -s elasticsearch-7.17.26  es7
```
创建数据目录：
```sh
$ mkdir -p /data/es7/logs
```
添加用户并授权：
```sh
$ groupadd elastic -g 1200 && useradd elastic -u 1200 -g 1200
$ chown -RL 1200.1200 /opt/es7 /data/es7
```
系统配置：
```sh
$ vim /etc/security/limits.conf
* soft nofile 65535
* hard nofile 65535
* soft nproc 65535
* hard nproc 65535
elastic soft memlock unlimited
elastic hard memlock unlimited

$ vim /etc/sysctl.conf
vm.max_map_count=262144

$ sysctl -p
$ systemctl stop firewalld && systemctl disable firewalld
```
## 配置
es 的配置文件为 `/opt/es7/config/elasticsearch.yml`，三个节点参考如下配置：
```yaml
cluster.name: devops
node.name: <es01|es02|es03> # 注意替换
node.master: true
node.data: true
path.data: /data/es7
path.logs: /data/es7/logs
bootstrap.memory_lock: true
network.host: 0.0.0.0
network.publish_host: 192.168.0.<21|22|23> # 注意替换
http.port: 9200

discovery.seed_hosts: 
- 192.168.0.21:9300
- 192.168.0.22:9300
- 192.168.0.23:9300
cluster.initial_master_nodes: ["es01", "es02", "es03"]
action.destructive_requires_name: true
http.cors.enabled: true
http.cors.allow-origin: "*"
ingest.geoip.downloader.enabled: false
```
## 启动
> 下面操作需要在所有主机执行。

以 elastic 用户启动测试（可选）：
```sh
$ cd /opt/es7
$ sudo -u elastic ./bin/elasticsearch
```
配置 systemd 管理：
```sh
$ cat /usr/lib/systemd/system/elasticsearch.service
[Unit]
Description=elasticsearch
After=network.target

[Service]
Type=simple
User=elastic
Group=elastic
Restart=no
ExecStart=/opt/es7/bin/elasticsearch
PrivateTmp=true
LimitMEMLOCK=infinity
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target
```
启动并设置自启：
```sh
$ systemctl start elasticsearch && systemctl enable elasticsearch
```
# 启用 ssl
> 参考：<https://www.cnblogs.com/gqdw/p/16624139.html>

在 es01 节点创建证书：
```
$ cd /opt/es7
$ ./bin/elasticsearch-certutil ca
$ ./bin/elasticsearch-certutil cert --ca elastic-stack-ca.p12
$ openssl pkcs12 -in elastic-stack-ca.p12 -out es-crt.pem -clcerts -nokeys
$ mv elastic-certificates.p12 elastic-stack-ca.p12 es-cert.pem config
```
这里 `elastic-certificates.p12`、`elastic-stack-ca.p12` 要同步到其它两个节点的  `/opt/es7/config` 目录。
在三个节点添加配置：
```sh
$ vim config/elasticsearch.yml
...
xpack.security.transport.ssl.enabled: true
xpack.security.transport.ssl.verification_mode: certificate 
xpack.security.transport.ssl.client_authentication: optional # required
xpack.security.transport.ssl.keystore.path: elastic-certificates.p12
xpack.security.transport.ssl.truststore.path: elastic-certificates.p12
```
重启生效。
# 设置密码
设置密码的前提是**启用 ssl**。
执行下面命令为每个用户设置密码（可以设置一样的）：
```sh
$ ./bin/elasticsearch-setup-passwords interactive
Changed password for user [apm_system]
Changed password for user [kibana_system]
Changed password for user [kibana]
Changed password for user [logstash_system]
Changed password for user [beats_system]
Changed password for user [remote_monitoring_user]
Changed password for user [elastic]
```
添加配置：
```
$ vim config/elasticsearch.yml
...
xpack.security.enabled: true
```
重启生效。
# 访问测试
我这里准备了一个脚本：
```sh
$ cat es-status.sh
clusterUrl='http://127.0.0.1:9200'
[[ $1 == "health" ]] && uri="/_cluster/health?pretty"
[[ $1 == "node-status" ]] && uri="/_nodes/_all/jvm?pretty"

# curl ${clusterUrl}${uri} # 未配置密码则用这行
curl --user <username>:<password> ${clusterUrl}${uri} # 配置了密码则用这行
```
执行就可以查看集群状态：
```sh
$ bash es-status.sh health
{
  "cluster_name" : "devops",
  "status" : "green",
  "timed_out" : false,
  "number_of_nodes" : 3,
  "number_of_data_nodes" : 3,
  "active_primary_shards" : 33,
  "active_shards" : 46,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 0,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "ac
```