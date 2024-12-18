- 官网：<https://goproxy.cn/>；
- Github：<https://github.com/goproxy/goproxy>；

到 [releases](https://github.com/goproxy/goproxy/releases) 页下载二进制包。

解压：
```bash
mkdir /opt/goproxy
tar xf goproxy_0.18.2_linux_amd64.tar.gz -C /opt/goproxy
```

编写脚本运行：
```sh
#!/bin/bash

export GOPROXY=https://goproxy.cn,direct
/opt/goproxy/bin/goproxy server --address :8080 --cacher-dir /data/goproxy
```