# Flink & Elasticsearch Stream

Created: September 12, 2024 1:48 PM

## Description

完成nginx日志数据处理数据流的最后一步，导入Flink进行流式计算，以及使用ES进行数据可视化。

## Flink & Elasticsearch Stream

### 安装Elasticsearch和Elasticsearch-head

1. 安装`es`图形化`chrome`插件`es-head`
2. 配置`es`并排除故障

```java
# 修改sysctl.conf解决虚拟内存溢出问题
vm.max_map_count=655360
sysctl -p # 完成修改
----------------------------------
./elasticsearch # 启动es服务
```

1. `es-head`进入`localhost:9200`查看，返回`green`，连接成功。

## Other sources & Reference

[flink 消费 kafka数据，将结果写入到elasticsearch中_flink读取kafka数据 写入es_慕雨潇潇的博客-CSDN博客](https://blog.csdn.net/dlhszn1mfy/article/details/89675942)