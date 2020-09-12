# Paxos 分布式算法

## CAP 理论

* Consistency 一致性
* Availability可用性
* Partition Tolerance 分区容错性

### 一致性模型

* 弱一致性：最终一致性，不强调实时一致性，而是结果最终一致
   * DNS：典型的弱一致性
   * Gossip
* 强一致性
   * Paxos
   * Raft

## Paxos

基本想法：
==多数派==：每次写都保证写入大于N/2个节点，每次读保证从大于N/2个节点读

执行顺序非常重要

 分类：

* Basic Paxos
* Multi Paxos
* Fast Paxos

### Basic Paxos

角色介绍：

* Client：系统外部角色，民众
* Propser：接收Client请求，想集群提出议题（propose）。议员，提出问题者
* Acceptor：提议投票和接收者，像国会议员
* Learner：议题接收者，做备份。记录员

### Basic Paxos

引入新的角色 Leader







