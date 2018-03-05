Centos7下Zookeeper安装及使用
<1> 安装Kafka
从清华镜像下载安装包
[root@localhost ~]# wget https://mirrors.tuna.tsinghua.edu.cn/apache/kafka/1.0.0/kafka_2.11-1.0.0.tgz
解压缩
[root@localhost ~]# tar -xvf kafka_2.11-1.0.0.tgz

<2> 配置Kafka
[root@localhost ~]# vi /opt/kafka_2.11-1.0.0/config/server.properties
主要修改如下内容：

# brokerid就是指各台服务器对应的id，所以各台服务器值不同
broker.id=0
# 端口号，无需改变
port=9092
# 当前服务器的IP，各台服务器值不同
host.name=localhost
# Zookeeper集群的ip和端口号
zookeeper.connect=localhost:2181
# 日志目录
log.dirs=/tmp/kafka-logs

<3> 启动Kafka
[root@localhost ~]# /opt/kafka_2.11-1.0.0/bin/kafka-server-start.sh /opt/kafka_2.11-1.0.0/config/server.properties

<4> 创建一个topic
[root@localhost ~]# /opt/kafka_2.11-1.0.0/bin/kafka-topics.sh --create --zookeeper localhost:2181,localhost:2181,localhost:2181,localhost:2181,localhost:2181 --replication-factor 1 --partitions 1 --topic testkafka
Created topic "testkafka".

<5> 查看topic信息
[root@localhost ~]# /opt/kafka_2.11-1.0.0/bin/kafka-topics.sh --describe --zookeeper localhost:2181,localhost:2181,localhost:2181,localhost:2181,localhost:2181 --topic testkafka
	Topic:testkafka	PartitionCount:1	ReplicationFactor:1	Configs:
	Topic: testkafka	Partition: 0	Leader: 0	Replicas: 0	Isr: 0

<6> 创建一个消息订阅
[root@localhost ~]# /opt/kafka_2.11-1.0.0/bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic testkafka --from-beginning
	
<7> 发布一条消息
[root@localhost ~]# /opt/kafka_2.11-1.0.0/bin/kafka-console-producer.sh --broker-list localhost:9092 --topic testkafka
>hello kafka
>this is my kafka test
>

可以在订阅者端看到消息内容如下：
[root@localhost ~]# /opt/kafka_2.11-1.0.0/bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic testkafka --from-beginning
hello kafka
this is my kafka test







