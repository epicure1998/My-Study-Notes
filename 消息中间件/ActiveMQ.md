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
   * 消息属性
   * 消息体