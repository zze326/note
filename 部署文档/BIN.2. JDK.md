# Oracle JDK

下载二进制包：
```bash
wget https://repo.huaweicloud.com/java/jdk/8u202-b08/jdk-8u202-linux-x64.tar.gz
```
解压安装：
```bash
tar xf jdk-8u202-linux-x64.tar.gz -C /usr/local
```
配置环境变量：
```bash
cat << EOF > /etc/profile.d/jdk8.sh
JAVA_HOME=/usr/local/jdk1.8.0_202
JRE_HOME=\$JAVA_HOME/jre
CLASSPATH=.:\$JAVA_HOME/lib/dt.jar:\$JAVA_HOME/lib/tools.jar:\$JRE_HOME/lib
PATH=\$JAVA_HOME/bin:\$PATH
export PATH JAVA_HOME CLASSPATH
EOF
```
> 其中的 `JAVA_HOME` 根据实际解压位置修改。

重新连接会话或手动加载一下 `/etc/profile.d/jdk8.sh`：
```bash
source /etc/profile.d/jdk8.sh
```
执行 `java` 命令确认环境信息：
```bash
$ java -version 
java version "1.8.0_202"
Java(TM) SE Runtime Environment (build 1.8.0_202-b08)
Java HotSpot(TM) 64-Bit Server VM (build 25.202-b08, mixed mode)
```

> [更多 OracleJDK 版本可点击查看](https://repo.huaweicloud.com/java/jdk/)。

# OpenJDK

下载二进制包：
```bash
wget https://mirrors.huaweicloud.com/openjdk/17.0.2/openjdk-17.0.2_linux-x64_bin.tar.gz
```

解压安装：
```bash
tar xf openjdk-17.0.2_linux-x64_bin.tar.gz -C /usr/local/
```

配置环境变量：
```bash
cat << EOF > /etc/profile.d/openjdk.sh
JAVA_HOME=/usr/local/jdk-17.0.2
PATH="\$JAVA_HOME/bin:\$PATH"
export PATH JAVA_HOME
EOF
```

重新连接会话或手动加载一下 `/etc/profile.d/openjdk.sh`：
```bash
source /etc/profile.d/openjdk.sh
```

执行 `java` 命令确认环境信息：
```bash
$ java -version 
openjdk version "17.0.2" 2022-01-18
OpenJDK Runtime Environment (build 17.0.2+8-86)
OpenJDK 64-Bit Server VM (build 17.0.2+8-86, mixed mode, sharing)
```

> [更多 OpenJDK 版本可点击查看](https://mirrors.huaweicloud.com/openjdk/)。
