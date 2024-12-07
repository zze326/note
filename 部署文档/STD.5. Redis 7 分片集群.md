> 华为镜像站地址：<https://mirrors.huaweicloud.com/redis/>。

3 台主机 6 个 redis 实例：

| 系统             | IP           | 主机名    | redis 实例端口 |
| -------------- | ------------ | ------ | ---------- |
| Ubuntu 22.04.5 | 172.16.1.204 | host01 | 6381、6382  |
| Ubuntu 22.04.5 | 172.16.1.205 | host02 | 6381、6382  |
| Ubuntu 22.04.5 | 172.16.1.206 | host03 | 6381、6382  |
# 安装程序包
安装编译环境：
```sh
$ apt install make gcc -y
```

在 `host01` 主机下载二进制包并编译安装：
```sh
$ wget https://mirrors.huaweicloud.com/redis/redis-7.4.1.tar.gz
$ tar xf redis-7.4.1.tar.gz
$ cd redis-7.4.1
$ make # 如果在安装编译环境前执行了 make，在安装编译环境后再执行时可能会报错，可以执行一下 make distclean 再执行 make。
$ make install PREFIX=/opt/redis/ #  PREFIX 指定安装到指定目录 
```
从 `host01` 将 `/opt/redis/` 拷贝到其它两台主机：
```sh
$ scp -rp /opt/redis 172.16.1.205:/opt
$ scp -rp /opt/redis 172.16.1.206:/opt
```
# 配置

## host01
创建两个实例的配置：
```sh
$ cat << EOF > /opt/redis/redis-6381.conf 
port 6381
# 开启集群功能
cluster-enabled yes
# 集群的配置文件名称，不需要我们创建，由redis自己维护
cluster-config-file /data/redis-6381/nodes.conf
# 节点心跳失败的超时时间
cluster-node-timeout 5000
# 持久化文件存放目录
dir /data/redis-6381
# 绑定地址
bind 0.0.0.0
# 让redis后台运行
daemonize yes
# 注册的实例ip
replica-announce-ip 172.16.1.204
# 保护模式
protected-mode no
# 数据库数量
# databases 1
# 日志
logfile /data/redis-6381/logs/redis.log
requirepass 123456
masterauth 123456
EOF

$ cat << EOF > /opt/redis/redis-6382.conf 
port 6382
# 开启集群功能
cluster-enabled yes
# 集群的配置文件名称，不需要我们创建，由redis自己维护
cluster-config-file /data/redis-6382/nodes.conf
# 节点心跳失败的超时时间
cluster-node-timeout 5000
# 持久化文件存放目录
dir /data/redis-6382
# 绑定地址
bind 0.0.0.0
# 让redis后台运行
daemonize yes
# 注册的实例ip
replica-announce-ip 172.16.1.204
# 保护模式
protected-mode no
# 数据库数量
# databases 1
# 日志
logfile /data/redis-6382/logs/redis.log
requirepass 123456
masterauth 123456
EOF
```
创建数据目录：
```sh
$ mkdir -p /data/redis-{6381,6382}
```

## host02
创建两个实例的配置：
```sh
$ cat << EOF > /opt/redis/redis-6381.conf 
port 6381
# 开启集群功能
cluster-enabled yes
# 集群的配置文件名称，不需要我们创建，由redis自己维护
cluster-config-file /data/redis-6381/nodes.conf
# 节点心跳失败的超时时间
cluster-node-timeout 5000
# 持久化文件存放目录
dir /data/redis-6381
# 绑定地址
bind 0.0.0.0
# 让redis后台运行
daemonize yes
# 注册的实例ip
replica-announce-ip 172.16.1.205
# 保护模式
protected-mode no
# 数据库数量
# databases 1
# 日志
logfile /data/redis-6381/logs/redis.log
requirepass 123456
masterauth 123456
EOF

$ cat << EOF > /opt/redis/redis-6382.conf 
port 6382
# 开启集群功能
cluster-enabled yes
# 集群的配置文件名称，不需要我们创建，由redis自己维护
cluster-config-file /data/redis-6382/nodes.conf
# 节点心跳失败的超时时间
cluster-node-timeout 5000
# 持久化文件存放目录
dir /data/redis-6382
# 绑定地址
bind 0.0.0.0
# 让redis后台运行
daemonize yes
# 注册的实例ip
replica-announce-ip 172.16.1.205
# 保护模式
protected-mode no
# 数据库数量
# databases 1
# 日志
logfile /data/redis-6382/logs/redis.log
requirepass 123456
masterauth 123456
EOF
```
创建数据目录：
```sh
$ mkdir -p /data/redis-{6381,6382}
```

## host03
创建两个实例的配置：
```sh
$ cat << EOF > /opt/redis/redis-6381.conf 
port 6381
# 开启集群功能
cluster-enabled yes
# 集群的配置文件名称，不需要我们创建，由redis自己维护
cluster-config-file /data/redis-6381/nodes.conf
# 节点心跳失败的超时时间
cluster-node-timeout 5000
# 持久化文件存放目录
dir /data/redis-6381
# 绑定地址
bind 0.0.0.0
# 让redis后台运行
daemonize yes
# 注册的实例ip
replica-announce-ip 172.16.1.206
# 保护模式
protected-mode no
# 数据库数量
# databases 1
# 日志
logfile /data/redis-6381/logs/redis.log
requirepass 123456
masterauth 123456
EOF

$ cat << EOF > /opt/redis/redis-6382.conf 
port 6382
# 开启集群功能
cluster-enabled yes
# 集群的配置文件名称，不需要我们创建，由redis自己维护
cluster-config-file /data/redis-6382/nodes.conf
# 节点心跳失败的超时时间
cluster-node-timeout 5000
# 持久化文件存放目录
dir /data/redis-6382
# 绑定地址
bind 0.0.0.0
# 让redis后台运行
daemonize yes
# 注册的实例ip
replica-announce-ip 172.16.1.206
# 保护模式
protected-mode no
# 数据库数量
# databases 1
# 日志
logfile /data/redis-6382/logs/redis.log
requirepass 123456
# 这个 masterauth 用于建立集群后 slave 连接到 master，如果不配置的话，建立集群是没有问题，但是集群重启后 slave 是无法连接到 master 的
masterauth 123456
EOF
```
创建数据目录：
```sh
$ mkdir -p /data/redis-{6381,6382}/logs
```
# 启动服务
> 下面操作在 3 台主机都要执行。

配置环境变量：
```sh
$ cat << EOF > /etc/profile.d/redis.sh
PATH="/opt/redis/bin:$PATH"
EOF
$ . /etc/profile.d/redis.sh
```
启动服务：
```sh
$ redis-server /opt/redis/redis-6381.conf
$ redis-server /opt/redis/redis-6382.conf
```

# 创建集群
现在就已经在三台主机启动了 6  redis 实例，在任一主机执行下面命令建立集群关系：
```sh
# --cluster-replicas 1 表示每个 master 有一个 slave
$ redis-cli -a 123456 --cluster create --cluster-replicas 1 172.16.1.204:6381 172.16.1.204:6382 172.16.1.205:6381 172.16.1.205:6382 172.16.1.206:6381 172.16.1.206:6382
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
>>> Performing hash slots allocation on 6 nodes...
Master[0] -> Slots 0 - 5460
Master[1] -> Slots 5461 - 10922
Master[2] -> Slots 10923 - 16383
Adding replica 172.16.1.205:6382 to 172.16.1.204:6381
Adding replica 172.16.1.206:6382 to 172.16.1.205:6381
Adding replica 172.16.1.204:6382 to 172.16.1.206:6381
M: c35f8e5876798ed7256ae900bedee1bff5d2d29a 172.16.1.204:6381
   slots:[0-5460] (5461 slots) master
S: 9cae1a77dff5df7e462ca774a713684c596aa20c 172.16.1.204:6382
   replicates 00a6b94d12425aad98da5cd0b4ba844f09e74bf7
M: 0e26ccfd15a4a58476482d363103528d55622be9 172.16.1.205:6381
   slots:[5461-10922] (5462 slots) master
S: 082fa87ce28173538e4a9e326fd4a2ddbefe9d1c 172.16.1.205:6382
   replicates c35f8e5876798ed7256ae900bedee1bff5d2d29a
M: 00a6b94d12425aad98da5cd0b4ba844f09e74bf7 172.16.1.206:6381
   slots:[10923-16383] (5461 slots) master
S: fb9a885e358f8009315aa44f2a0c180bd3240d9f 172.16.1.206:6382
   replicates 0e26ccfd15a4a58476482d363103528d55622be9
Can I set the above configuration? (type 'yes' to accept): yes
>>> Nodes configuration updated
>>> Assign a different config epoch to each node
>>> Sending CLUSTER MEET messages to join the cluster
Waiting for the cluster to join
.
>>> Performing Cluster Check (using node 172.16.1.204:6381)
M: c35f8e5876798ed7256ae900bedee1bff5d2d29a 172.16.1.204:6381
   slots:[0-5460] (5461 slots) master
   1 additional replica(s)
M: 0e26ccfd15a4a58476482d363103528d55622be9 172.16.1.205:6381
   slots:[5461-10922] (5462 slots) master
   1 additional replica(s)
S: fb9a885e358f8009315aa44f2a0c180bd3240d9f 172.16.1.206:6382
   slots: (0 slots) slave
   replicates 0e26ccfd15a4a58476482d363103528d55622be9
S: 9cae1a77dff5df7e462ca774a713684c596aa20c 172.16.1.204:6382
   slots: (0 slots) slave
   replicates 00a6b94d12425aad98da5cd0b4ba844f09e74bf7
M: 00a6b94d12425aad98da5cd0b4ba844f09e74bf7 172.16.1.206:6381
   slots:[10923-16383] (5461 slots) master
   1 additional replica(s)
S: 082fa87ce28173538e4a9e326fd4a2ddbefe9d1c 172.16.1.205:6382
   slots: (0 slots) slave
   replicates c35f8e5876798ed7256ae900bedee1bff5d2d29a
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.
```
可以看到已经建立了集群关系：
- `172.16.1.204:6381`、`172.16.1.205:6381`、`172.16.1.206:6381` 为 master；
- `172.16.1.204:6382` 是 `172.16.1.206:6381` 的 slave；
- `172.16.1.205:6382` 是 `172.16.1.204:6381` 的 slave；
- `172.16.1.206:6382` 是 `172.16.1.205:6381` 的 slave；
此时是允许单台主机宕机的，比如 `172.16.1.206` 挂了，即：
- master `172.16.1.206:6381` 挂了，但它的 slave `172.16.1.204:6382` 会提升为 master；
- slave `172.16.1.206:6382` 挂了，其 master `172.16.1.205:6381` 不受影响；
数据依旧是完整的。

# Systemd 管理
> 下面操作在三台主机执行。

关闭服务：
```sh
$ redis-cli -a 123456 -p 6381 shutdown
$ redis-cli -a 123456 -p 6382 shutdown
```

创建 systemd unit 配置：
```sh
$ cat << EOF > /etc/systemd/system/redis-6381.service 
[Unit]
Description=Redis Cluster
After=network.target

[Service]
Type=forking
ExecStart=/opt/redis/bin/redis-server /opt/redis/redis-6381.conf
ExecStop=/opt/redis/bin/redis-cli -a 123456 -p 6381 shutdown
Restart=always
#User=app
#Group=app
#RuntimeDirectory=app
#RuntimeDirectoryMode=0755

[Install]
WantedBy=multi-user.target
EOF

$ cat << EOF > /etc/systemd/system/redis-6382.service 
[Unit]
Description=Redis Cluster
After=network.target

[Service]
Type=forking
ExecStart=/opt/redis/bin/redis-server /opt/redis/redis-6382.conf
ExecStop=/opt/redis/bin/redis-cli -a 123456 -p 6382 shutdown
Restart=always
#User=app
#Group=app
#RuntimeDirectory=app
#RuntimeDirectoryMode=0755

[Install]
WantedBy=multi-user.target
EOF
```

启动服务：
```sh
$ systemctl start redis-638{1,2}
```