kafka 镜像站：
- 官方：<https://archive.apache.org/dist/kafka/>；
- 阿里云：<https://mirrors.aliyun.com/apache/kafka/>；

# 环境准备
以三节点集群为例，我这里使用如下三台主机（1 核 2G 以上）：
- 192.168.0.21；
- 192.168.0.22；
- 192.168.0.23；

在三台主机参考 [BIN.2. JDK](BIN.2.%20JDK.md) 安装 OpenJDK。

# kafka 集群部署
> 下面操作要在每台主机执行，各主机的差异点有注释说明。

下载二进制包并解压到指定目录：
```sh
$ wget https://mirrors.aliyun.com/apache/kafka/3.9.0/kafka_2.13-3.9.0.tgz
$ tar xf kafka_2.13-3.9.0.tgz -C /opt/
$ cd /opt && ln -s kafka_2.13-3.9.0/ kafka
```
创建数据目录：
```sh
$ mkdir -p /data/kafka
```

编辑配置文件：
```sh
$ vim /opt/kafka/config/kraft/server.properties
process.roles=broker,controller
node.id=<1|2|3> # 修改处，每个节点不同
controller.quorum.voters=1@192.168.0.21:9093,2@192.168.0.22:9093,3@192.168.0.23:9093
listeners=PLAINTEXT://:9092,CONTROLLER://:9093
inter.broker.listener.name=PLAINTEXT
advertised.listeners=PLAINTEXT://192.168.0.<21|22|23>:9092,CONTROLLER://192.168.0.<21|22|23>:9093 # 修改处，每个节点不同
controller.listener.names=CONTROLLER
listener.security.protocol.map=CONTROLLER:PLAINTEXT,PLAINTEXT:PLAINTEXT,SSL:SSL,SASL_PLAINTEXT:SASL_PLAINTEXT,SASL_SSL:SASL_SSL
num.network.threads=3
num.io.threads=8
socket.send.buffer.bytes=102400
socket.receive.buffer.bytes=102400
socket.request.max.bytes=104857600
log.dirs=/data/kafka/ # 修改处，指定数据存储目录
num.partitions=1
num.recovery.threads.per.data.dir=1
offsets.topic.replication.factor=1
transaction.state.log.replication.factor=1
transaction.state.log.min.isr=1
log.retention.hours=168 # 默认存储 1 周
log.segment.bytes=1073741824
log.retention.check.interval.ms=300000
```

配置环境变量：
```sh
$ cat << EOF > /etc/profile.d/kafka.sh
PATH=/opt/kafka/bin:\$PATH
export PATH
EOF

$ source /etc/profile.d/kafka.sh
```

初始化集群：
```sh
# 任选一个主机执行下面命令生成 uuid
$ kafka-storage.sh random-uuid
txPlNn_1T8mlc1JJbcCWwA

# 用生成的 uuid 在三台主机执行以初始化存储目录
$ kafka-storage.sh format -t txPlNn_1T8mlc1JJbcCWwA -c /opt/kafka/config/kraft/server.properties
Formatting metadata directory /data/kafka/ with metadata.version 3.9-IV0.
```

启动集群：
```sh
# 在三台主机启动测试
$ kafka-server-start.sh /opt/kafka/config/kraft/server.properties
```
> `kafka-server-start.sh -daemon /opt/kafka/config/kraft/server.properties` 可以直接以守护进程形式启动，但是建议使用 systemd 来管理。

创建 Systemd 配置：
```sh
$ cat > /usr/lib/systemd/system/kafka.service << EOF
[Unit]
Description=Kafka Server Service
After=network.target

[Service]
Type=forking
User=0
Group=0
KillMode=control-group
Environment="JAVA_HOME=/usr/local/jdk-17.0.2/"
ExecStart=/opt/kafka/bin/kafka-server-start.sh -daemon /opt/kafka/config/kraft/server.properties
ExecStop=/opt/kafka/bin/kafka-server-stop.sh
ExecReload=/bin/kill -s HUP $MAINPID
SuccessExitStatus=0 143
PrivateTmp=false
LimitNOFILE=infinity
LimitNPROC=infinity
TimeoutStopSec=10s
Restart=on-failure
RestartSec=30

[Install]
WantedBy=multi-user.target
EOF
```

启动并设置开机自启：
```sh
$ systemctl start kafka && systemctl enable kafka
```

Topic 测试：
```sh
$ kafka-topics.sh --bootstrap-server localhost:9092 --create --topic test --partitions 3 --replication-factor 3
$ kafka-topics.sh --bootstrap-server localhost:9092 --describe test
Topic: test	TopicId: EoUdfyOOTFKltD8BftldRQ	PartitionCount: 3	ReplicationFactor: 3	Configs: segment.bytes=1073741824
	Topic: test	Partition: 0	Leader: 3	Replicas: 3,1,2	Isr: 3,1,2	Elr: 	LastKnownElr: 
	Topic: test	Partition: 1	Leader: 1	Replicas: 1,2,3	Isr: 1,2,3	Elr: 	LastKnownElr: 
	Topic: test	Partition: 2	Leader: 2	Replicas: 2,3,1	Isr: 2,3,1	Elr: 	LastKnownElr: 
$ kafka-topics.sh --bootstrap-server localhost:9092 --delete --topic test
```