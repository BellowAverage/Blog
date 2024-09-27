# Kafka3.x Basics

Created: September 12, 2024 1:48 PM
Labels & Language: CentOS, Kafka
Source: https://www.bilibili.com/video/BV1vr4y1677k/

## 引言

Kafka是一个开源的分布式流处理平台，最初由LinkedIn开发并于2011年开源。它被设计用于处理高容量的实时数据流，包括日志、事件流和其他实时数据。Kafka通常被用作一种可靠的消息系统，采用发布-订阅模式，其中数据被组织成所谓的"topic"，并且可以被多个消费者订阅。生产者可以将消息发布到特定的topic中，而消费者则可以从这些topic中读取消息并进行处理。

## Kafka3.x 基础篇

### 安装和配置kafka于CentOS

1. 修改`config`文件`server.properties`

```bash
# A comma separated list of directories under which to store log files
# log.dirs=/tmp/kafka-logs 修改为：
log.dirs=/home/chriswang/Applications/kafka/data
# zookeeper.connect = localhost:2181 修改为：
zookeeper.connect=hadoop102:2181,hadoop103:2181,hadoop104:2181/kafka
# hadoop服务器102-104配置信息见[Hadoop3.x Basics](http://124.222.120.214/media/notion_files/070523/hadoop_basics_notes.html)
```

1. 修改hadoop服务器的`broker.id`，即分别在`hadoop103`和`hadoop104`上修改配置文件中的`/home/chriswang/Applications/kafka/config/server.properties`中的 `broker.id=1`， `broker.id=2`
2. 基础zookeeper操作

```java
bin/zkServer.sh start 开启服务端
bin/zkCli.sh 开启客户端
quit 退出界面
```

## Other References

[【尚硅谷】Kafka3.x教程（从入门到调优，深入全面）_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1vr4y1677k/)