# Flume & Kafka Stream

Created: September 12, 2024 1:48 PM
Labels & Language: Java, Kafka, flume
Source: https://flume.apache.org/releases/content/1.11.0/FlumeUserGuide.html

## 引言

将`nginx`日志通过`Flume`采集到Kafka。在这个过程中，`nginx`作为`source`，`kafka`作为`sink`的对象。`Flume`是一个强大的日志采集工具，`kafka`则可以有效地对服务器进行分布式管理。

## Flume & Kafka 日志文件流

### Https Requests → Nginx → Flume → Kafka

1. 测试Flume功能 - 使用`Linux nc`命令测试`Flume`是否配置正确

```
# Name the components on this agenta1.sources= r1
a1.sinks= k1
a1.channels= c1

# Describe/configure the sourcea1.sources.r1.type= netcat
a1.sources.r1.bind= localhost
a1.sources.r1.port= 44444

# Describe the sinka1.sinks.k1.type= logger

# Use a channel which buffers events in memorya1.channels.c1.type= memory
a1.channels.c1.capacity= 1000
a1.channels.c1.transactionCapacity= 100

# Bind the source and sink to the channela1.sources.r1.channels= c1
a1.sinks.k1.channel= c1

# Flume启动
bin/flume-ng agent --conf conf --conf-file example.conf --name a1
bin/flume-ng agent -n a1 -c conf/ -f job/net-flume-logger.conf -Dflume.root.logger=INFO,console
```

使用nc命令监听指定接口，收到返回，测试成功：

```java
2023-08-02 19:37:33,287 (SinkRunner-PollingRunner-DefaultSinkProcessor) [INFO - org.apache.flume.sink.LoggerSink.process(LoggerSink.java:95)] Event: { headers:{} body: 31 39 32 2E 31 36 38 2E 31 30 2E 31 20 2D 20 2D 192.168.10.1 - - }
2023-08-02 19:39:07,300 (SinkRunner-PollingRunner-DefaultSinkProcessor) [INFO - org.apache.flume.sink.LoggerSink.process(LoggerSink.java:95)] Event: { headers:{} body: 31 39 32 2E 31 36 38 2E 31 30 2E 31 20 2D 20 2D 192.168.10.1 - - }
2023-08-02 19:39:29,518 (SinkRunner-PollingRunner-DefaultSinkProcessor) [INFO - org.apache.flume.sink.LoggerSink.process(LoggerSink.java:95)] Event: { headers:{} body: 31 39 32 2E 31 36 38 2E 31 30 2E 31 20 2D 20 2D 192.168.10.1 - - }
```

1. 编写`Flume`配置文件实现将`nginx`日志作为源，`sink`进入`kafka`

```java
pro.sources = s1
pro.channels = c1
pro.sinks = k1

pro.sources.s1.type = exec
pro.sources.s1.command = tail -F /var/log/nginx/access.log

pro.channels.c1.type = memory
pro.channels.c1.capacity = 1000
pro.channels.c1.transactionCapacity = 100

pro.sinks.k1.type = org.apache.flume.sink.kafka.KafkaSink
pro.sinks.k1.kafka.topic = moercredit_log_test
pro.sinks.k1.kafka.bootstrap.servers = localhost:9092
pro.sinks.k1.kafka.flumeBatchSize = 20
pro.sinks.k1.kafka.producer.acks = 1
pro.sinks.k1.kafka.producer.linger.ms = 1
pro.sinks.k1.kafka.producer.compression.type = snappy

pro.sources.s1.channels = c1
pro.sinks.k1.channel = c1

---------------------------------------------------------
bin/flume-ng agent -n pro -c conf/ -f conf/flume2kafka.conf -Dflume.root.logger=INFO,console
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic moercredit_log_test --from-beginning
```

1. 先后分别打开`zookeeper`，`kafka`，`flume`服务。并运行`http`自动访问`python`脚本
2. `kafka`信息接受窗口收到返回，成功：

```java
# kafka获得日志流数据形如：
192.168.10.102 - - [03/Aug/2023:02:06:58 +0800] "GET /index/ HTTP/1.1" 200 9825 "-" "Mozilla/5.0 (Windows NT 6.1; WOW64; rv:39.0) Gecko/20100101 Firefox/75.0"
192.168.10.103 - - [03/Aug/2023:02:06:58 +0800] "GET /index/ HTTP/1.1" 200 9825 "-" "Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.1; WOW64; Trident/4.0; SLCC2; .NET CLR 2.0.50727; .NET CLR 3.5.30729; .NET CLR 3.0.30729; Media Center PC 6.0; Zune 3.0)"
192.168.10.104 - - [03/Aug/2023:02:06:58 +0800] "GET /index/ HTTP/1.1" 200 9825 "-" "Opera/9.63 (X11; Linux x86_64; U; ru) Presto/2.1.1"
192.168.10.1 - - [03/Aug/2023:02:06:58 +0800] "GET /index/ HTTP/1.1" 200 2650 "-" "python-requests/2.31.0"
192.168.10.102 - - [03/Aug/2023:02:06:58 +0800] "GET /index/ HTTP/1.1" 200 9825 "-" "Mozilla/4.0 (compatible; MSIE 5.0; Linux 2.4.19-4GB i686) Opera 6.11  [en]"
192.168.10.103 - - [03/Aug/2023:02:06:58 +0800] "GET /index/ HTTP/1.1" 200 9825 "-" "Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10_6_2; en-US) AppleWebKit/533.2 (KHTML, like Gecko) Chrome/5.0.343.0 Safari/533.2"
192.168.10.104 - - [03/Aug/2023:02:06:58 +0800] "GET /index/ HTTP/1.1" 200 9825 "-" "Mozilla/5.0 (X11; U; Linux i686; en-US; rv:1.9.0.2) Gecko/2008110715 ASPLinux/3.0.2-3.0.120asp Firefox/3.0.2"
192.168.10.1 - - [03/Aug/2023:02:06:58 +0800] "GET /index/ HTTP/1.1" 200 2650 "-" "python-requests/2.31.0"
[^C mannual stop] Processed a total of 234 messages
```

1. 查看`kafka data`文件，发现部分乱码，暂无解决。

## Other sources & Reference

[Flume 1.11.0 User Guide — Apache Flume](https://flume.apache.org/releases/content/1.11.0/FlumeUserGuide.html)