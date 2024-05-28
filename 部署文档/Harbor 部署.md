1、下载安装包（Harbor 镜像使用 Docker-Compose 做编排）：
- Harbor 下载链接：<https://github.com/goharbor/harbor/releases>，我这里使用的是 [harbor-offline-installer-v2.9.4.tgz](https://github.com/goharbor/harbor/releases/download/v2.9.4/harbor-offline-installer-v2.9.4.tgz)；
- Docker-Compose 下载链接：<https://github.com/docker/compose/releases>，我这里使用的是 [docker-compose-Linux-x86_64 2.27.1](https://github.com/docker/compose/releases/download/v2.27.1/docker-compose-linux-x86_64)；
2、安装 Docker 并启动：
```bash
$ curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
$ systemctl start docker && systemctl enable docker
```
3、安装 Docker-Compose，由于这里我们下载的是 Docker-Compose 二进制包，所以直接添加执行权限就可以执行，为方便后续使用这里我将它添加到 `PATH` 包含的路径下：
```bash
$ chmod +x docker-compose-Linux-x86_64 && mv docker-compose-Linux-x86_64 /usr/local/bin/docker-compose
```
4、配置 Harbor，因为 Harbor 是基于 Docker-Compose 启动的，所以我们下载是一个压缩包，其中包含一些 Harbor 所需镜像，如下：
```
# 解压 Harbor 压缩包
$ tar xf harbor-offline-installer-v2.1.0.tgz
# 修改配置，由于这里是简单演示，需要注释掉该配置文件中的 https 节配置，如果是通过 IP 访问则 hostname 配置为 IP，否则配置为对应的域名
$ cd harbor && mv harbor.yml.tmpl harbor.yml && vim harbor.yml
hostname: <Harbor 所在主机的 IP 或域名>
...
# 注释 HTTPS 节
#https:
#  port: 443
#  certificate: /your/certificate/path
#  private_key: /your/private/key/path
...
```
5、加载镜像并启动：
```bash
$ ./install.sh 
# 检查是否正常启动
$ docker-compose ps
      Name                     Command                  State                 Ports          
---------------------------------------------------------------------------------------------
harbor-core         /harbor/entrypoint.sh            Up (healthy)                            
harbor-db           /docker-entrypoint.sh            Up (healthy)                            
harbor-jobservice   /harbor/entrypoint.sh            Up (healthy)                            
harbor-log          /bin/sh -c /usr/local/bin/ ...   Up (healthy)   127.0.0.1:1514->10514/tcp
harbor-portal       nginx -g daemon off;             Up (healthy)                            
nginx               nginx -g daemon off;             Up (healthy)   0.0.0.0:80->8080/tcp     
redis               redis-server /etc/redis.conf     Up (healthy)                            
registry            /home/harbor/entrypoint.sh       Up (healthy)                            
registryctl         /home/harbor/start.sh            Up (healthy)    
```