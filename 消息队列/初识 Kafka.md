kafka Api：https://kafka-python.readthedocs.io/en/master/apidoc/KafkaConsumer.html

kafka安装：https://www.cnblogs.com/xichji/p/11696266.html

kafka入门：https://baijiahao.baidu.com/s?id=1651919282506404758&wfr=spider&for=pc

##                                            **初识 Kafka**

### **什么是 Kafka**

Kafka 是由 Linkedin 公司开发的，它是一个分布式的，支持多分区、多副本，基于 Zookeeper 的分布式消息流平台，它同时也是一款开源的基于发布订阅模式的消息引擎系统。

它有三个关键能力

订阅发布记录流，它类似于企业中的消息队列 或 企业消息传递系统

以容错的方式存储记录流

实时记录流

### **Kafka 的基本术语**

消息：Kafka中的数据单元被称为消息，也被称为记录，可以把他作数据库某一行的记录。

批次：为了提高效率，消息回分批次写入Kafka，批次就代指一组消息。

主题：消息的种类被称之为主题（topic）,可以说是一个主题代表了一类消息。相当于对消息进行分类。主题就像数据库中的表。

分区：主题可以分为若干分区（partition）,同一主题的分区可以不在一个机器上，有可能会部署在多个机器上，由此来实现kafka的伸缩性，单一主题中分区有序，但是无法主题中的分区有序。

![img](D:\work\notbook\xinsixiangyi7@163.com\44f458810ed74d3d8af3da472af8326e\clipboard.png)

生产者：向主题发布消息的客户端应用程序称为生产者（Producer），生产者用于持续不断的向某个主题发送消息。

消费者：订阅主题消息的客户端程序称为消费者（Consumer），消费者用于处理生产者产生的消息。

消费者群组：生产者与消费者的关系就如同餐厅中的厨师和顾客之间的关系一样，一个厨师对应多个顾客，也就是一个生产者对应多个消费者，消费者群组（Consumer Group）指的就是由一个或多个消费者组成的群体。

![img](D:\work\notbook\xinsixiangyi7@163.com\af5bd4e0055642b5854dcfb65064c536\clipboard.png)

偏移量：偏移量（consumer offset）是一种元数据，它是一个不断递增的整数值，用来记录消费者发生重平衡时的位置，以便用来恢复数据。

broker:一个独立的Kafka服务器被称为broker，broker接受生产者的消息，为消息设置偏移量，并提交消息到磁盘保存。

broker集群：broker是集群的组成部分，broker集群由一个或多个broker组成，每个集群都有一个或多个broker组成，每个集群都有一个broer同事充当了集群控制器的角色（自动从集群的活跃成员中选举出来）。

副本：Kafka 中消息的备份又叫做 副本（Replica），副本的数量是可以配置的，Kafka 定义了两类副本：领导者副本（Leader Replica） 和 追随者副本（Follower Replica），前者对外提供服务，后者只是被动跟随。

重平衡：Rebalance。消费者组内某个消费者实例挂掉后，其他消费者实例自动重新分配订阅主题分区的过程。Rebalance 是 Kafka 消费者端实现高可用的重要手段。

### **Kafka 的应用**

1. 作为消息系统
2. 作为存储系统
3. 作为流处理器

Kafka 可以建立流数据管道，可靠性的在系统或应用之间获取数据。

建立流式应用传输和响应数据。

### **Kafka 作为消息系统**

Kafka 作为消息系统，它有三个基本组件

![img](D:\work\notbook\xinsixiangyi7@163.com\af32e2170ce14afca4a4a42b3a10b7e6\3-815729534.jpeg)

- Producer : 发布消息的客户端
- Broker：一个从生产者接受并存储消息的客户端
- Consumer : 消费者从 Broker 中读取消息

在大型系统中，会需要和很多子系统做交互，也需要消息传递，在诸如此类系统中，你会找到源系统（消息发送方）和 目的系统（消息接收方）。为了在这样的消息系统中传输数据，你需要有合适的数据管道

![img](D:\work\notbook\xinsixiangyi7@163.com\a9a5573e7df24e8f8a023993dbbf456b\4-427472992.jpeg)

这种数据的交互看起来就很混乱，如果我们使用消息传递系统，那么系统就会变得更加简单和整洁

![img](D:\work\notbook\xinsixiangyi7@163.com\656c7a6655ef4af89f47e2126ac4df9a\-1540882394.jpeg)

- Kafka 运行在一个或多个数据中心的服务器上作为集群运行
- Kafka 集群存储消息记录的目录被称为 topics
- 每一条消息记录包含三个要素：**键（key）、值（value）、时间戳（Timestamp）**

### **核心 API**

Kafka 有四个核心API，它们分别是

- Producer API，它允许应用程序向一个或多个 topics 上发送消息记录
- Consumer API，允许应用程序订阅一个或多个 topics 并处理为其生成的记录流
- Streams API，它允许应用程序作为流处理器，从一个或多个主题中消费输入流并为其生成输出流，有效的将输入流转换为输出流。
- Connector API，它允许构建和运行将 Kafka 主题连接到现有应用程序或数据系统的可用生产者和消费者。例如，关系数据库的连接器可能会捕获对表的所有更改

![img](D:\work\notbook\xinsixiangyi7@163.com\612b9a9b6a3d443bba27b26ef1e50b15\7-937799681.jpeg)