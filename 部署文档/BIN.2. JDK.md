下载二进制包：
```bash
wget https://repo.huaweicloud.com/java/jdk/8u202-b08/jdk-8u202-linux-x64.tar.gz
```
> [点击获取更多版本](https://repo.huaweicloud.com/java/jdk/)。
解压安装：
```bash
tar xf jdk-8u202-linux-x64.tar.gz -C /usr/local
```
修改 `/etc/profile` 在文件末尾添加如下内容以设置环境变量：
```bash
JAVA_HOME=/usr/local/jdk1.8.0_202
JRE_HOME=/usr/local/jdk1.8.0_202/jre
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
PATH=$JAVA_HOME/bin:$PATH
export PATH JAVA_HOME CLASSPATH
```
> 其中的 `JAVA_HOME` 和 `JRE_HOME` 根据实际解压位置修改。

重新连接会话或手动加载一下 `/etc/profile`：
```bash
source /etc/profile
```
执行 `java` 命令确认环境信息：
```bash
$ java -version 
java version "1.8.0_202"
Java(TM) SE Runtime Environment (build 1.8.0_202-b08)
Java HotSpot(TM) 64-Bit Server VM (build 25.202-b08, mixed mode)
```

> [更多 jdk 版本可点击查看](https://repo.huaweicloud.com/java/jdk/)。