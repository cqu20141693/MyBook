### kafka 学习
1. kafka java API
2. spring Kafka -> /spring/kafka

#### windows 安装kafka
1. 首先安装JDK
2. 下载zookeeper和kafka
    1. 启动zookeeper : 也可以使用kafka自带的zookeeper
    2. 启动kafka 

3. kafka 使用命令
    1. 启动zookeeper: .\bin\windows\zookeeper-server-start.bat .\config\zookeeper.properties
       关闭zookeeper: .\bin\windows\zookeeper-server-stop.bat
    2. 启动kafka :  .\bin\windows\kafka-server-start.bat .\config\server.properties
       关闭kafka : .\bin\windows\kafka-server-stop.bat
    3. 创建topic :  .\kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic input_stream
    4. list 所有topic : .\kafka-topics.bat --zookeeper localhost:2181 --list
    5. 生产数据 : .\kafka-console-producer.bat --broker-list localhost:9092 --topic input_stream
    6. 消费数据 : .\kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic input_stream --from-beginning
    7. 描述一个topic :  .\kafka-topics.bat --describe --zookeeper localhost:2181 --topic test
    8. 查看topic 分组 : ./kafka-consumer-groups.bat  --bootstrap-server 127.0.0.1:9092 --list
        1. 查看group消费情况. /kafka-consumer-groups.bat  --bootstrap-server 127.0.0.1:9092 --describe --group foreach_test
        2. 删除某个group信息 : .\kafka-consumer-groups.bat --delete --bootstrap-server 127.0.0.1:9092 --group foreach_test
    9. 

4. 验证命令 sh 
    1. 启动 zk : ./bin/zookeeper-server-start.sh ./config/zookeeper.properties
        关闭zk : ./bin/zookeeper-server-stop.sh 

    2. 启动 kafka : 

    1. bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test --producer.config config/producer.properties
    2. bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning --consumer.config config/consumer.properties
### kafak Stream 
0. kafka stream的
1. 创建消费kafka 的source Stream
    ```

    ```
2. 对流进行流计算处理

3. 对计算的结果进行处理

4. 

device1 : tag : java, c
device2 : tag : java,python

### 启动错误
1. 找不到jdk : 需要对启动脚本进行修改


### 配置sasl 鉴权通信

### kafka 配置生产和消费鉴权
1. 安装好kafka

2. 配置 broker
```

 1. 添加 jaas 配置文件: kafka_server_jaas.conf
KafkaServer {
    org.apache.kafka.common.security.plain.PlainLoginModule required
    username="admin"
    password="admin"
    user_admin="admin"
    user_alice="alice-secret";
};


 2. 修改启动文件 kafka-server-start.sh 
 在倒数第二行添加jvm 参数 指定jaas文件: 
export KAFKA_OPTS="-Djava.security.auth.login.config=/root/kafka/kafka2/kafka_2.11-2.3.1/config/kafka_server_jaas.conf"

3. 现在使用一般的客户端进行连接;就会出现鉴权失败情况:
 bin/kafka-console-producer.sh --broker-list localhost:9292 --topic test-topic

4. 配置client jaas 进行认证配置
    ```
    1. vim config/producer.properties | consumer.properties
    在末尾加上

    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
        username="alice" \
        password="alice-secret";

    security.protocol=SASL_PLAINTEXT
    sasl.mechanism=PLAIN



    2. 启动producer

    bin/kafka-console-producer.sh --broker-list localhost:9292 --topic test --producer.config config/producer.properties

    

    3. 生产者 acl 进行授权
        bin/kafka-acls.sh --authorizer-properties zookeeper.connect=localhost:2281 --add --allow-principal User:alice --producer --topic test-topic
    4. 消费者 acl 
        bin/kafka-acls.sh --authorizer-properties zookeeper.connect=localhost:2281 --add --allow-principal User:alice --consumer --topic test-topic --group test-consumer

    5. 消费数据
        bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test-topic --from-beginning --consumer.config config/consumer.properties
    
    ```
5. 配置java api sasl jaas 进行认证;



```