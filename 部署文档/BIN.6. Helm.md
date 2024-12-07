华为镜像站：<https://mirrors.huaweicloud.com/helm/>。

下载二进制包：
```sh
$ wget https://mirrors.huaweicloud.com/helm/v3.16.3/helm-v3.16.3-linux-amd64.tar.gz
```

解压将二进制程序放到 `PATH` 环境变量包含的目录下即可：
```sh
$ tar xf helm-v3.16.3-linux-amd64.tar.gz
$ mv linux-amd64/helm /usr/local/bin/
```

配置命令补全：
```sh
$ echo 'source <(helm completion bash)' >> ~/.bashrc
```

重连会话生效。

如果补全过程中有异常报错，可以安装 `bash-completion` 试试：
```sh
$ apt install bash-completion -y
# 或 yum install bash-completion -y
```