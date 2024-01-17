下载二进制压缩包：
```bash
$ wget https://repo.huaweicloud.com/docker-ce/linux/static/stable/x86_64/docker-24.0.7.tgz
```
> [点击获取其它版本](https://repo.huaweicloud.com/docker-ce/linux/static/stable/x86_64/)。

解压到指定目录：
```bash
$ tar xf docker-24.0.7.tgz -C /opt
```
创建软链接到 `/usr/local/bin`：
```bash
$ cd /opt/docker
$ ls | xargs -i ln -s /opt/docker/{} /usr/bin/
```
创建配置文件：
```bash
$ mkdir /etc/docker
$ vim /etc/docker/daemon.json
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "registry-mirrors": [
    "https://docker.mirrors.ustc.edu.cn",
    "http://hub-mirror.c.163.com"
  ],
  "insecure-registries": ["registry.zze.xyz"],   
  "max-concurrent-downloads": 10,
  "log-driver": "json-file",
  "log-level": "warn",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
    },
  "data-root": "/var/lib/docker"
}
```

创建 Systemd unit 文件：
```bash
$ vim /usr/lib/systemd/system/docker.service
[Unit]
Description=Docker Application Container Engine
Documentation=https://docs.docker.com
After=network-online.target firewalld.service
Wants=network-online.target

[Service]
Type=notify
# the default is not to use systemd for cgroups because the delegate issues still
# exists and systemd currently does not support the cgroup feature set required
# for containers run by docker
ExecStart=/usr/bin/dockerd
ExecReload=/bin/kill -s HUP $MAINPID
# Having non-zero Limit*s causes performance problems due to accounting overhead
# in the kernel. We recommend using cgroups to do container-local accounting.
LimitNOFILE=infinity
LimitNPROC=infinity
LimitCORE=infinity
# Uncomment TasksMax if your systemd version supports it.
# Only systemd 226 and above support this version.
#TasksMax=infinity
TimeoutStartSec=0
# set delegate yes so that systemd does not reset the cgroups of docker containers
Delegate=yes
# kill only the docker process, not all processes in the cgroup
KillMode=process
# restart the docker process if it exits prematurely
Restart=on-failure
StartLimitBurst=3
StartLimitInterval=60s

[Install]
WantedBy=multi-user.target
```
启动并设置开机自启：
```bash
$ systemctl start docker && systemctl enable docker
```