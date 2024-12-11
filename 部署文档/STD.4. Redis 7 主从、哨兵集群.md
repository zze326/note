> 华为镜像站地址：<https://mirrors.huaweicloud.com/redis/>。

3 台主机：

| 系统       | IP           | 主机名     |
| -------- | ------------ | ------- |
| CentOS 7 | 192.168.0.21 | redis01 |
| CentOS 7 | 192.168.0.22 | redis02 |
| CentOS 7 | 192.168.0.23 | redis03 |

# 主从集群
下面操作先 `redis01` 主机执行。

安装编译环境：
```sh
$ yum install gcc -y
```
下载二进制包，解压并编译安装：
```sh
$ wget https://mirrors.huaweicloud.com/redis/redis-7.4.1.tar.gz
$ tar xf redis-7.4.1.tar.gz
$ cd redis-7.4.1
$ make # 如果在安装编译环境前执行了 make，在安装编译环境后再执行时可能会报错，可以执行一下 make distclean 再执行 make。
$ make install PREFIX=/opt/redis/ #  PREFIX 指定安装到指定目录 
```
拷贝配置文件到安装目录：
```sh
$ cp redis.conf /opt/redis/redis.conf.bak
$ cd /opt/redis
$ vim redis.conf
bind 0.0.0.0
port 6379
daemonize yes
pidfile "/var/run/redis_6379.pid"
logfile "/data/redis/logs/redis.log"
loglevel notice
dbfilename "dump.rdb"
dir "/data/redis"
appendonly yes
appendfilename "appendonly.aof"
# maxmemory 7812500kb
requirepass "123456"
masterauth "123456"

save 3600 1
save 300 100
save 60 10000
```
从 `redis01` 将 `/opt/redis` 拷贝到 `redis02` 和 `redis03` 主机。
```sh
$ scp -rp /opt/redis 192.168.0.22:/opt/
$ scp -rp /opt/redis 192.168.0.23:/opt/
```
编辑 `redis02` 和 `redis03` 主机的 `/opt/redis/redis.conf`，在底部加上如下配置：
```
replicaof 192.168.0.21 6379
```
在三台主机创建数据目录和日志目录并配置环境变量：
```sh
$ mkdir -p /data/redis/logs
$ cat << EOF > /etc/profile.d/redis.sh
PATH="/opt/redis/bin:$PATH"
EOF
$ . /etc/profile.d/redis.sh
```
先启动 master，再启动 slave：
```sh
$ redis-server /opt/redis/redis.conf
```
测试在 master 写入数据：
```sh
$ redis-cli
127.0.0.1:6379> auth 123456
OK
127.0.0.1:6379> set a 1
OK
```
测试在 slave 读取数据：
```sh
$ redis-cli 
127.0.0.1:6379> auth 123456
OK
127.0.0.1:6379> get a
"1"
```
停止服务：
```sh
$ redis-cli -a 123456 shutdown
```
配置 systemd 管理：
```sh
$ cat << EOF > /usr/lib/systemd/system/redis.service
[Unit]
Description=Redis In-Memory Data Store
After=network.target

[Service]
Type=forking
ExecStart=/opt/redis/bin/redis-server /opt/redis/redis.conf
ExecStop=/opt/redis/redis-cli -p 6379 -a 123456 shutdown
Restart=always
#User=redis
#Group=redis
#RuntimeDirectory=redis
#RuntimeDirectoryMode=0755

[Install]
WantedBy=multi-user.target
EOF
```
启动并配置开机自启：
```sh
$ systemctl start redis && systemctl enable redis
```
# 哨兵集群
在三个主机创建如下哨兵配置（一模一样）：
```
$ cat << EOF > /opt/redis/sentinel.conf
port 26379
bind 0.0.0.0
protected-mode no
dir "/data/redis"
logfile "/data/redis/logs/redis_sentinel.log"
pidfile "/var/run/redis_sentinel.pid"
daemonize yes

# 监控主节点配置 
sentinel monitor mymaster 192.168.0.21 6379 2
sentinel auth-pass mymaster 123456
sentinel down-after-milliseconds mymaster 5000
sentinel failover-timeout mymaster 10000
EOF
```
> 这里 `mymaster` 只是一个集群标示，没有特殊含义，可以自定义，多个节点保持一致即可。

在三台主机启动测试：
```sh
$ redis-sentinel /opt/redis/sentinel.conf --sentinel
```
在 redis01 查看复制信息：
```sh
$ redis-cli -a 123456
127.0.0.1:6379> info replication 
# Replication
role:master
connected_slaves:2 # 2 个 slave
slave0:ip=192.168.0.23,port=6379,state=online,offset=55336,lag=0
slave1:ip=192.168.0.22,port=6379,state=online,offset=55483,lag=0
master_failover_state:no-failover
...
```
停掉 `redis01` 主机：
```sh
$ systemctl stop redis
```

在 `redis03` 主机查看复制信息：
```sh
$ redis-cli -a 123456
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
127.0.0.1:6379> info replication 
# Replication
role:slave
master_host:192.168.0.22 # 可以看到 master 自动切换到了 0.22
master_port:6379
master_link_status:up
...
```

启动 `redis01` ：
```sh
$ systemctl start redis
```
停止三个主机上的哨兵服务：
```sh
$ /opt/redis/bin/redis-cli -p 26379 shutdown
```
配置 systemd 管理：
```
$ cat << EOF > /etc/systemd/system/redis-sentinel.service 
[Unit]
Description=Redis Sentinel
After=network.target

[Service]
Type=forking
ExecStart=/opt/redis/bin/redis-sentinel /opt/redis/sentinel.conf
ExecStop=/opt/redis/bin/redis-cli -p 26379 shutdown
Restart=always
#User=app
#Group=app
#RuntimeDirectory=app
#RuntimeDirectoryMode=0755

[Install]
WantedBy=multi-user.target
EOF
```
启动并设置开机自启：
```sh
$ systemctl start redis-sentinel && systemctl enable redis-sentinel
```

# 哨兵代理
代理使用 haproxy，拉取镜像：
```sh
$ docker pull haproxy:3.1.0
```

编写 haproxy 配置文件：
```sh
$ mkdir -p /opt/haproxy
$ vi /opt/haproxy/haproxy.cfg 
defaults REDIS
  mode tcp
  timeout connect 4s
  timeout server 330s
  timeout client 330s
  timeout check 2s

listen health_check_http_url
  bind 0.0.0.0:8888  v4v6
  mode http
  monitor-uri /healthz
  option      dontlognull
# Check Sentinel and whether they are nominated master
backend check_if_redis_is_master_0
  mode tcp
  option tcp-check
  tcp-check connect
  tcp-check send PING\r\n
  tcp-check expect string +PONG
  tcp-check send SENTINEL\ get-master-addr-by-name\ "${CLUSTER_NAME}"\r\n
  tcp-check expect string "${REDIS_HOST1}"
  tcp-check send QUIT\r\n
  server R0 "${REDIS_HOST1}:26379" check inter 1s
  server R1 "${REDIS_HOST2}:26379" check inter 1s
  server R2 "${REDIS_HOST3}:26379" check inter 1s
# Check Sentinel and whether they are nominated master
backend check_if_redis_is_master_1
  mode tcp
  option tcp-check
  tcp-check connect
  tcp-check send PING\r\n
  tcp-check expect string +PONG
  tcp-check send SENTINEL\ get-master-addr-by-name\ "${CLUSTER_NAME}"\r\n
  tcp-check expect string "${REDIS_HOST2}"
  tcp-check send QUIT\r\n
  server R0 "${REDIS_HOST1}:26379" check inter 1s
  server R1 "${REDIS_HOST2}:26379" check inter 1s
  server R2 "${REDIS_HOST3}:26379" check inter 1s
# Check Sentinel and whether they are nominated master
backend check_if_redis_is_master_2
  mode tcp
  option tcp-check
  tcp-check connect
  tcp-check send PING\r\n
  tcp-check expect string +PONG
  tcp-check send SENTINEL\ get-master-addr-by-name\ "${CLUSTER_NAME}"\r\n
  tcp-check expect string "${REDIS_HOST3}"
  tcp-check send QUIT\r\n
  server R0 "${REDIS_HOST1}:26379" check inter 1s
  server R1 "${REDIS_HOST2}:26379" check inter 1s
  server R2 "${REDIS_HOST3}:26379" check inter 1s

# decide redis backend to use
#master
frontend ft_redis_master
  bind 0.0.0.0:6379 v4v6
  use_backend bk_redis_master
# Check all redis servers to see if they think they are master
backend bk_redis_master
  mode tcp
  option tcp-check
  tcp-check connect
  tcp-check send "AUTH ${AUTH}"\r\n
  tcp-check expect string +OK
  tcp-check send PING\r\n
  tcp-check expect string +PONG
  tcp-check send info\ replication\r\n
  tcp-check expect string role:master
  tcp-check send QUIT\r\n
  tcp-check expect string +OK
  use-server R0 if { srv_is_up(R0) } { nbsrv(check_if_redis_is_master_0) ge 2 }
  server R0 "${REDIS_HOST1}:6379" check inter 1s fall 1 rise 1
  use-server R1 if { srv_is_up(R1) } { nbsrv(check_if_redis_is_master_1) ge 2 }
  server R1 "${REDIS_HOST2}:6379" check inter 1s fall 1 rise 1
  use-server R2 if { srv_is_up(R2) } { nbsrv(check_if_redis_is_master_2) ge 2 }
  server R2 "${REDIS_HOST3}:6379" check inter 1s fall 1 rise 1
frontend stats
  mode http
  bind 0.0.0.0:9101 v4v6
  http-request use-service prometheus-exporter if { path /metrics }
  stats enable
  stats uri /stats
  stats refresh 10s
```

启动容器：
```sh
$ docker run --rm \
    -v /opt/haproxy/haproxy.cfg:/usr/local/etc/haproxy/haproxy.cfg \
    -p 6388:6379 \
    -p 9101:9101 \
    -p 8888:8888 \
    -e CLUSTER_NAME=mymaster \
    -e AUTH=123456 \
    -e REDIS_HOST1=192.168.0.21 \
    -e REDIS_HOST2=192.168.0.22 \
    -e REDIS_HOST3=192.168.0.23 \
    -it haproxy:3.1.0
```
此时容器中的 6379 端口映射出来的 6388 就是代理到当前 redis 集群 master 节点：
```sh
$ redis-cli -h 127.0.0.1 -p 6388 -a 123456
```

9101 端口是给 prometheus 采集 metrics 的端口：
```sh
$ curl 127.0.0.1:9101/metrics
```

9101 端口是给 prometheus 采集 metrics 的端口：
```sh
$ curl 127.0.0.1:9101/metrics
```

8888 用来做健康检查（用 k8s 部署可以用）：
```sh
$ curl 127.0.0.1:8888/healthz
```