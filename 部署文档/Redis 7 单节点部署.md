> 华为镜像站地址：<https://mirrors.huaweicloud.com/redis/>。

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

save 3600 1
save 300 100
save 60 10000
```
在三台主机创建数据目录和日志目录并配置环境变量：
```sh
$ mkdir -p /data/redis/logs
$ cat << EOF > /etc/profile.d/redis.sh
PATH="/opt/redis/bin:$PATH"
EOF
$ . /etc/profile.d/redis.sh
```
启动：
```sh
$ redis-server /opt/redis/redis.conf
```
测试写入数据：
```sh
$ redis-cli
127.0.0.1:6379> auth 123456
OK
127.0.0.1:6379> set a 1
OK
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
