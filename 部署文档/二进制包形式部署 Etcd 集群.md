> 官方文档：<https://etcd.io/docs/v3.5/tutorials/how-to-setup-cluster/>。
> Github Releases：<https://github.com/etcd-io/etcd/releases/>。
> 华为镜像站：<https://mirrors.huaweicloud.com/etcd/>。

# 主机准备
我这里准备三台主机：
- etcd1：192.168.0.21；
- etcd2：192.168.0.22；
- etcd3：192.168.0.23；
# 部署
>下面操作需要在所有主机执行。

下载二进制包并配置环境变量：
```sh
$ wget https://mirrors.huaweicloud.com/etcd/v3.5.17/etcd-v3.5.17-linux-amd64.tar.gz
$ tar xf etcd-v3.5.17-linux-amd64.tar.gz -C /opt/
$ cd /opt && ln -s etcd-v3.5.17-linux-amd64 etcd
$ cat << EOF > /etc/profile.d/etcd.sh
PATH="/opt/etcd:$PATH"
EOF
$ . /etc/profile.d/etcd.sh
```
创建数据目录：
```sh
$ mkdir -p /data/etcd
```
创建配置文件：
```sh
$ cat /opt/etcd/env.conf
# 注意替换 <etcd1|etcd2|etcd3>
ETCD_NAME=<etcd1|etcd2|etcd3>
ETCD_DATA_DIR=/data/etcd
ETCD_LISTEN_CLIENT_URLS=http://192.168.0.<21|22|23>:2379,http://127.0.0.1:2379
ETCD_LISTEN_PEER_URLS=http://192.168.0.<21|22|23>:2380
ETCD_ADVERTISE_CLIENT_URLS=http://192.168.0.<21|22|23>:2379
ETCD_INITIAL_ADVERTISE_PEER_URLS=http://192.168.0.<21|22|23>:2380
ETCD_INITIAL_CLUSTER=etcd1=http://192.168.0.21:2380,etcd2=http://192.168.0.22:2380,etcd3=http://192.168.0.23:2380
ETCD_INITIAL_CLUSTER_STATE=new
ETCD_INITIAL_CLUSTER_TOKEN=etcd-cluster
```
配置 systemd 管理：
```sh
$ vim /usr/lib/systemd/system/etcd.service
[Unit]
Description=etcd

[Service]
Type=notify
EnvironmentFile=/opt/etcd/env.conf
ExecStart=/opt/etcd/etcd
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
```
启动并设置自启：
```sh
$ systemctl start etcd && systemctl enable etcd
```
确认状态：
```sh
$ etcdctl --endpoints=http://127.0.0.1:2379 member list
165583720ecd010, started, etcd3, http://192.168.0.23:2380, http://192.168.0.23:2379, false
64113736f52b426f, started, etcd2, http://192.168.0.22:2380, http://192.168.0.22:2379, false
eed8c8c0d6cd3132, started, etcd1, http://192.168.0.21:2380, http://192.168.0.21:2379, false
```
# 开启认证
```sh
# 创建角色
etcdctl --endpoints=http://127.0.0.1:2379 role add root
# 创建管理员用户
etcdctl --endpoints=http://127.0.0.1:2379 user add root:<密码>
# 授权角色给用户
etcdctl --endpoints=http://127.0.0.1:2379 user grant-role root root
# 开启认证
etcdctl --endpoints=http://127.0.0.1:2379 auth enable
```
测试访问：
```sh
# 匿名写 key
$ etcdctl --endpoints=http://127.0.0.1:2379  put /mykey '1'
Error: etcdserver: user name is empty
# 指定用户写 key
$ etcdctl --endpoints=http://127.0.0.1:2379 --user=root:<密码> put /mykey '1'
OK
# 获取 Key
$ etcdctl --endpoints=http://127.0.0.1:2379 --user=root:<密码> get /mykey
```