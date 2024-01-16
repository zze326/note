> 主机需要预装 JDK 环境。

下载二进制包：
```bash
$ wget https://mirrors.tuna.tsinghua.edu.cn/apache/rocketmq/5.0.0/rocketmq-all-5.0.0-bin-release.zip
```
解压到安装目录并建立软链接：
```bash
$ unzip rocketmq-all-5.0.0-bin-release.zip -d /opt/
$ cd /opt
$ ln -s rocketmq-all-5.0.0-bin-release/ rocketmq
```
调整内存分配大小（视机器配置而定）：
```bash
$ vim /opt/rocketmq/bin/runserver.sh 
# JAVA_OPT="${JAVA_OPT} -server -Xms4g -Xmx4g -Xmn2g -XX:MetaspaceSize=128m -XX:MaxMetaspaceSize=320m"
JAVA_OPT="${JAVA_OPT} -server -Xms1g -Xmx1g -Xmn512m -XX:MetaspaceSize=128m -XX:MaxMetaspaceSize=320m"

$ vim /opt/rocketmq/bin/runbroker.sh
# JAVA_OPT="${JAVA_OPT} -server -Xms8g -Xmx8g"
JAVA_OPT="${JAVA_OPT} -server -Xms1g -Xmx1g -Xmn512m"
```
后台启动 nameserver 服务：
```bash
$ screen -S rocketmq-nameserver
$ sh /opt/rocketmq/bin/mqnamesrv
# CTRL A D
```
后台启动 broker 服务：
```bash
$ screen -S rocketmq-broker
$ sh bin/mqbroker -n localhost:9876
# CTRL A D
```
关闭服务：
```bash
$ sh bin/mqshutdown broker
$ sh bin/mqshutdown namesrv
```