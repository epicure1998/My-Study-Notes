[TOC]



# Redis

> Nosql+RDBMS关系型数据库
>
> 应用：
>
> > * **数据库**
>
> > * **缓存**
>
> > * 消息中间件 mq



**互联网基本的架构：**

![image-20200905125210865](/Users/apple/Desktop/My-Study-Notes/Redis读书笔记/image-20200905125210865.png)

## 为什么要用Nosql(非关系型数据库)

### 3高

* 高并发

* 高可扩

* 高性能

### 3v

* Volume

* Variaty

* Velocity

### 持久化

1. RDB

2. AOF

## Redis 的安装

1. 从官网下载压缩包[redis官网](redis.io)

2. 更改配置文件，`daemonize yes`默认启动

3. 在src/目录下使用`redis-server [配置文件名称]`进行redis服务器启动

4. `redis-cli -h [host] -p [port]`启动客户端

5. 可以使用将`redis-server`以及`redis-cli`加入环境变量中

```bash
cp -l src/redis-server /usr/local/bin && cp -l src/redis-cli /usr/local/bin

\# -l:表示创建link连接文件
```

***

## redis基本指令

> #默认有16个数据库
>
> > #端口号：6379
> >
> > #redis是单线程的，基于内存操作
> >
> > #redis的瓶颈是内存和网络带宽
> >
> > #redis将所有的数据放在内存中，单线程效率最高
>
> 

* 先`shutdown`然后`exit`:退出redis

* `redis-server [conf/path]`:启动redis服务端

* `select 3`选择数据库，切换数据库

* `flushall` 清空数据库

* `flushdb` 清空当前数据库

* `keys *` 查看当前数据库所有数据

* `exists` 判断键是否存在

* `move [key] 1` 从指定数据库移除

* `expire [key] 10`设置过期的时间

* `ttl` 查看过期剩余的时间

* `type [key]`查看当前key的类型

### redis的基本数据类型

> **五大基本数据类型**

#### String

> 基本命令：

> * `strlen`:字符串长度
> * append`:追加字符串，如果当前key不存在，相当于set`
> * `incr`:加一
> *  `decr`:减一
> *  `incrby/decrby`:步长加减
> *  `getrange [from] [to]`: 查看一定范围的字符串
> *  `setrange [key] [from]`: 相当于java的从指定字符串位置进行替换
> *  `setex []`:设置过期时间
> *  `setnx []`:如果这个值不存在再执行设置
> *  `mset/msetnx`:批量设置值,mset是原子性的操作，一起失败
> *  `mget`:批量获取
> *  `set user:1:name zhangsan`:对象化设置值，复用
> *  `getset`:先get再set

#### list列表

> 可以模拟队列，栈
> 所有的list命令都是l开头
> * `[LR]PUSH [listname] [value]`:从左或者右插入
> * `LRANGE [listname] [from][to]`:按照范围查看
> * `lpop list`:移除list的第一个元素
> * `rpop list`:移除list最后一个元素
> * `lindex [listname] [下标]`:通过下标获得list中的某一个值
> * `llen [listname]`:返回列表长度
> * `lrem [listname] [数量] [key]`:移除list集合中指定的值
> * `ltrim [list] 1 2`:剪枝
> * `rpushlpop [source] [destination]`:移除最后一个元素，放到另一列表中
> * `lset`:在指定列表的指定下表设置值，如果存在就更新，不存在就报错
> * `linsert`:在指定位置插入新的值
>

#### Set

>Set中元素不能够重复，无顺序不重复集合
>
>* `sadd`:将set中添加值
>* `smembers`:获取所有元素
>* `sismember`:判断某一个元素是不是在set中
>* `srem [list Name] [key]`:移除某一个元素
>* `srandmember [list]`:随机取出一个key
>* `spop [list]`:随机移除元素
>* `smove [from list] [to list]`: 集合间的移动
>* `sdiff [key1] [key2]`:差集
>* `sinter [key1] [key2]`:交集
>* `sunion [key1] [key2]`:并集

#### Hash

>
>
>* `hset [setname] [key] [value]` :存储指定的key-value
>* `hget [hash name][key] `:获取指定的值
>* `hmset`:多值设置
>* `hmget`:多值获取
>* `hgetall`:获取所有keyvalue
>* `hlen`:hash表字段数量
>*  `hkeys`:
>* `hvals`:
>* `hincrby [hashname] [key] [num]`:
>* `hsetnx [hashname] [key] [value]`:如果不存在则设置 

#### Zset 有序集合

> * `zadd [list] [priority] [name]`:给添加的值设置优先级
> * `zrangebyscore [setname] -inf +inf`: 按照顺序排序
> * `zrevrangebyscore`:倒叙排列
> * `zcard [setname]`:获取有序集合的元素个数

***

##  Redis 三种特殊数据类型以及应用场景

###  **Geospatial**

> 应用场景：
>
> > * 推算两地之间的距离
> > * 探探附近的人
>
> 地理位置经纬度计算
>
> 底层的实现是zset:
>
> ```bash
> 127.0.0.1:6379> type china:city
> zset
> ```
>
> 
>
> 

>* ` geoadd`:添加地理位置
>
>* `geopos [china:city] [city]:`获取指定的城市的经度和纬度
>
>* `geodist [key] [city1] [city2]:`计算指定位置之间的距离
>
>   ```bash
>   geodist china:city beijing chongqing km
>   "1464.0708"
>   ```
>
>* `georadius`:查看指定为中心，指定半径内的元素
>
>   ```bash
>   127.0.0.1:6379> georadius key longitude latitude radius m|km|ft|mi [WITHCOORD]
>   ```
>
>* `georadiusbymember`:以成员为中心，来查询半径内的成员
>
>   ```bash
>   127.0.0.1:6379> georadiusbymember key member radius m|km|ft|mi [WITHCOORD] [WITH
>   ```
>

### **Hyperloglog 基数统计**

> 什么是基数？
>
> 一个集合去重复后的剩下的元素的个数

用来做基数统计的算法：

>网页的UV(统计一个网站的访问次数，去除重复ip)
>
> > 传统的方式利用set集合来保存用户id，然后统计set中的元素数量作为标准判断，这种方式会浪费大量的空间
>
>然而使用hyperloglog能够很好的解决这个问题，2^64^个不同的数据只需要耗费12kb的内存
>
>缺点是会有误差

#### 基本的命令使用：

* `pfadd`：添加元素

	```bash
	127.0.0.1:6379> pfadd key element [element ...]
	```

* `pfcount`:统计元素的数量

	```bash
	127.0.0.1:6379> pfcount key [key ...]
	```

* `pfmerge`:合并两组key

   ```
   127.0.0.1:6379> pfcount key [key ...] 
   ```

### **Bitmaps** 位存储

> * 统计用户信息
> * 活跃不活跃
> * 登录或者未登录
> * 打卡系统
> * 用0和1来代表两种结果

#### 基本命令

* `setbit`:

   ```bash
   127.0.0.1:6379> setbit key offset value
   ```

* `getbit`:

	```bash
	127.0.0.1:6379> getbit key offset
	```

* `bitcount`:

	```bash
 	bitcount key [start end]
	```







