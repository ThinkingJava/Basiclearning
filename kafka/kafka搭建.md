docker单机版搭建

编写docker-compose.yml脚本，内容如下：

```yaml
version: '2'
services:
  zookeeper:
    image: wurstmeister/zookeeper
    ports:
      - "2181:2181"
  kafka1:
    image: wurstmeister/kafka:2.11-0.11.0.3
    ports:
      - "9092:9092"
    environment:
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://192.168.0.103:9092
      KAFKA_LISTENERS: PLAINTEXT://:9092
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      KAFKA_CREATE_TOPICS: "topic001:2:1"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock

```

上述配置中有两处需要注意：
第一，**KAFKA_ADVERTISED_LISTENERS**的配置，这个参数会写到kafka配置的advertised.listeners这一项中，应用会用来连接broker；

第二，**KAFKA_CREATE_TOPICS**的配置，表示容器启动时会创建名为"topic001"的主题，并且partition等于2，副本为1；

1. 在docker-compose.yml所在目录执行命令docker-compose up -d，启动容器；

2. 执行命令docker ps，可见容器情况，kafka的容器名为service_kafka1_1：

   ```java
   root@yang-Lenovo-G400:/usr/local/service# docker ps
   CONTAINER ID        IMAGE                              COMMAND                  CREATED             STATUS              PORTS                                                NAMES
   f1f6a608076e        wurstmeister/zookeeper             "/bin/sh -c '/usr/sb…"   3 hours ago         Up 3 hours          22/tcp, 2888/tcp, 3888/tcp, 0.0.0.0:2181->2181/tcp   service_zookeeper_1
   1763376f7df3        wurstmeister/kafka:2.11-0.11.0.3   "start-kafka.sh"         3 hours ago         Up 3 hours          0.0.0.0:9092->9092/tcp                               service_kafka1_1
   847ddbf46cc2        redis                              "docker-entrypoint.s…"   5 months ago        Up 2 days           0.0.0.0:6379->6379/tcp                               redis
   13e0cae86a9d        mysql:5.7.17                       "docker-entrypoint.s…"   5 months ago        Up 2 days           0.0.0.0:3306->3306/tcp                               mysql
   
   
   ```

3. 执行以下命令可以查看topic001的基本情况：

   ```lua
   docker exec service_kafka1_1 \
   kafka-topics.sh \
   --describe \
   --topic topic001 \
   --zookeeper zookeeper:2181
   
   ```

   看到的信息如下：

   ```lua
   Topic:topic001	PartitionCount:2	ReplicationFactor:1	Configs:
   	Topic: topic001	Partition: 0	Leader: 1001	Replicas: 1001	Isr: 1001
   	Topic: topic001	Partition: 1	Leader: 1001	Replicas: 1001	Isr: 1001
   
   ```

   

kafka springboot项目配置

```lua
spring:
  kafka:
    bootstrap-servers: yangchenghuan.develop:9092
    #生产者的配置，大部分我们可以使用默认的，这里列出几个比较重要的属性
    producer:
      #每批次发送消息的数量
      batch-size: 16
      #设置大于0的值将使客户端重新发送任何数据，一旦这些数据发送失败。注意，这些重试与客户端接收到发送错误时的重试没有什么不同。允许重试将潜在的改变数据的顺序，如果这两个消息记录都是发送到同一个partition，则第一个消息失败第二个发送成功，则第二条消息会比第一条消息出现要早。
      retries: 0
      #producer可以用来缓存数据的内存大小。如果数据产生速度大于向broker发送的速度，producer会阻塞或者抛出异常，以“block.on.buffer.full”来表明。这项设置将和producer能够使用的总内存相关，但并不是一个硬性的限制，因为不是producer使用的所有内存都是用于缓存。一些额外的内存会用于压缩（如果引入压缩机制），同样还有一些用于维护请求。
      buffer-memory: 33554432
      #key序列化方式
      key-serializer: org.apache.kafka.common.serialization.StringSerializer
      value-serializer: org.apache.kafka.common.serialization.StringSerializer
    #消费者的配置
    consumer:
      #Kafka中没有初始偏移或如果当前偏移在服务器上不再存在时,默认区最新 ，有三个选项 【latest, earliest, none】
      auto-offset-reset: latest
      #是否开启自动提交
      enable-auto-commit: true
      #自动提交的时间间隔
      auto-commit-interval: 100
      #key的解码方式
      key-deserializer: org.apache.kafka.common.serialization.StringDeserializer
      #value的解码方式
      value-deserializer: org.apache.kafka.common.serialization.StringDeserializer
      #在/usr/local/etc/kafka/consumer.properties中有配置
      group-id: 0


```

