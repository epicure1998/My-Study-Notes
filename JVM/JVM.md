[TOC]

# JVM

1. 位置
2. 体系结构
3. 类加载器
4. 双亲委派机制
5. 沙箱安全机制
6. Native
7. PC寄存器
8. 方法去
9. 栈
10. 三种JVM
11. 堆
12. 新生区，老年区
13. 永久区
14. 堆内存调优
15. GC
16. JMM
17. 总结

## 垃圾收集器与内存分配

### 对象是否死亡的判断：

* 引用计数算法：
* 可达性分析算法：

### 再谈引用：

四种引用的类型：

* 强引用：永远不清除
* 软引用：没有直接引用切内存空间不足时，进行GC
* 弱引用：只要没有引用了就GC
* 虚引用：唯一作用是能让一个对象被收集器回收时得到一个系统通知

### 垃圾处理算法：方法论

Eden 和 Survivor 区域的比例是：8比1

* 标记-清除算法：易造成碎片化，容易导致以后需要分配大对象时，无法分配内存从而提前触发垃圾处理算法
* 复制算法：将存活的对象复制后重新整理，存活率高的情况下效率会很低
* 标记-整理算法：
* 分代收集算法：

### 如何进入GC

* 枚举根节点：Stop the world
* 安全点：
   * 抢先式中断
   * 主动式中断

### 垃圾收集器：具体实现

> 垃圾收集器中并发以及并行的概念：
>
> * 并行（Parallel）：指的是多线程并行进行垃圾收集，但用户线程仍然是暂停状态
> * 并发（Concurrent）：指的是垃圾收集与用户线程同时进行

* Serial：Client模式，单线程情况下是很好的选择，没有线程交互的开销
* ParNew：Serial的多线程版本，新生代采用复制算法，老年代采用标记-整理算法，同样是Stop the world
* Parallel Scavenge：新生代收集，可控制的吞吐量，吞吐量描述的是用户线程执行时间和垃圾收集时间的比
* Serial Old：
* Parallel Old：
* CMS(Concurrent Mark Sweep)：主要作用于老年代，大多应用在B/S系统的服务器上，标记清除算法，并发的低停顿的，默认启动的线程数量是（CPU num +3）/4
   * CMS有一个触发阈值，JDK1.5时默认的对老年代的内存回收的阈值是62%，而在1.6时提升到了92%，然而当老年代增速太快时而预留内存无法满足时，就会造成Concurrent mode failure.
   * 由于使用的是标记-清除算法，会有大量内存碎片，从而容易引发Full GC,
      * 可以使用开关参数：-XX:+UseCMSCompactAtFullCollection(默认是开启的)，在进行Full GC前先对空间进行整理
      * -XX:CMSFullGCsBeforeCompaction:用于设置多少次不压缩Full GC后，进行一次带压缩的，默认值是0，表示每次进行Full GC 时进行整理
* G1：同样立足于停顿时间，低停顿

GC 处理常用参数：

![image-20201018121557218](/Users/apple/Desktop/My-Study-Notes/JVM/image-20201018121557218.png)

![image-20201018121630337](/Users/apple/Desktop/My-Study-Notes/JVM/image-20201018121630337.png)



### 内存分配和回收策略：

要避免大对象，且避免短命大对象

## 虚拟机性能监控与故障处理工具：

常用的工具：![image-20201018155014063](/Users/apple/Desktop/My-Study-Notes/JVM/image-20201018155014063.png)

* 可视化的工具：JConsole,VisualVM

## 类文件结构
