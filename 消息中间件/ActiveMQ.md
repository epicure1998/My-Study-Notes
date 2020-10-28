# 消息中间件

* 用处：
   * 削峰：设置流量缓冲池，提高吞吐能力
   * 解耦：新增模块方便
   * 异步：
* 弊端：
   * 

* 系统和系统之间耦合度高，互相牵连。

* 面对大流量的时候，容易被冲垮

* 等待同步存在性能问题

## 常用命令

* `ps -ef | grep activmq | grep -v grep`
* `./activemq stop`
* `./activemq start > ./activemq.log`

### 标准的API讲解(queqe)

![image-20201019221028630](/Users/apple/Desktop/My-Study-Notes/消息中间件/image-20201019221028630.png)

创建连接，一个连接创建多个Session，定义一个消息的类型，然后生产者和消费者。

两种模式，点对点和主题模式

```java
// brokerURL:activeMQ的地址 192.168.3.13:61616
```

#### 生产者

sendMessage();

#### 消费者通过receive（）的方式

consumer.receive()：阻塞式的方法，一直等待

consumer.receive(int time)：等待time时间，超过时间就关闭。

#### 监听模式messageListener

异步非阻塞式的，一人一半

### 标准的API讲解(topic)

类比微信公众号

订阅后，自动发送再订阅之后的消息

topic不保存消息，它是无状态的不落地，没人订阅，发布的就是一条废消息

![image-20201019221658185](/Users/apple/Desktop/My-Study-Notes/消息中间件/image-20201019221658185.png)

createTopic(String topicName); 

### 两种模式的比较

 ![image-20201019225920149](/Users/apple/Desktop/My-Study-Notes/消息中间件/image-20201019225920149.png)

## JMS 规范

Java Message Service

JAVAEE中的一个模块

什么是JAVA EE：13个企业级开发规范：

![image-20201019230221530](/Users/apple/Desktop/My-Study-Notes/消息中间件/image-20201019230221530.png)

四大元素：

* JMS provider
* JMS producer
* JMS consumer
* JMS message:深入了解
   * 消息头
      1. Destination：queqe,topic
      2. DeliveryMode:保证消息的持久化，持久化的消息，会在服务器宕机恢复后重发
      3. Expiration:过期时间，默认用不过期
      4. priority：优先级 0-4为普通，5-9是加急，默认4级
      5. MessageID: 最重要，唯一识别每个消息的标识
   * 消息属性： 识别，去重复，由键值对组成。重点标注
   * 消息体：封装具体的消息数据
      * Text
      * Map
      * Bytes
      * Stream
      * Object
      * 难道不会造成误收消息吗

在接收消息是如果用text作为消息体，而用map形式接收消息，会怎么样呢？类型不匹配错误？异常？还是直接出Null保证服务的运行

***

消息可靠性的保证：

持久化：

* 非持久：.setDiliveryMode(DiliveryMode.NO_PERSISTENT);宕机后消息不复存在
* 持久化：.setDiliveryMode(DiliveryMode.PERSISTENT); 队列是默认持久化的

> 持久化状态下，如果在发布订阅的目的下，先发布消息，再订阅，在消费者掉线后再连接，还是能收到持久化的消息

### JMS的事务：

什么是事务？保证程序的高可用。

偏生产者，开启了事务，需要手动提交commit()，如果事务中有消息出异常了，抛出异常后，可以子啊catch中回滚事务。

签收：面向消费者

* 自动签收：
* 手动签收：textMessage.acknowledge()，如果按照事务提交，就会默认签收了  
* 允许重复的签收：
* 签收和事务的关系，如果开启事务，那么一切消息的消费都基于事务，签收与否不关心。如果非事务，那么是否签收取决于是否.acknowledge()

***

Broker 相当于实例化的activeMQ

`./activemq start xbean:file:/....`启动指定配置文件

##  ActiveMQ传输协议

![image-20201023233846622](/Users/apple/Desktop/My-Study-Notes/消息中间件/image-20201023233846622.png)

### TCP:

配置格式：`tcp//hostname:port?key=value`

### NIO: 

适用场景：

1. 可能有大量的Client去连接到Broker, 一般情况下，大量的Client

 ## 消息中间件的持久化



