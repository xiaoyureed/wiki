---
title: Distributed System
tags:
  - distributed
date: 2019-07-23 21:05:56
categories: distributed system
---


<div align="center">
[CAP是什么](https://mwhittaker.github.io/blog/an_illustrated_proof_of_the_cap_theorem/)

https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf

http://www.royans.net/wp/2010/02/14/brewers-cap-theorem-on-distributed-systems/

https://github.com/qiurunze123 淘宝分布式大牛
</div>

<!--more-->

<!-- TOC -->

- [什么是分布式系统and为什么需要它](#什么是分布式系统and为什么需要它)
    - [分布式存储系统](#分布式存储系统)
    - [分布式计算系统](#分布式计算系统)
- [分布式系统的特性and如何衡量](#分布式系统的特性and如何衡量)
- [涉及哪些技术规范](#涉及哪些技术规范)
- [CAP 理论](#cap-理论)
    - [cap 介绍](#cap-介绍)
    - [保证一致性的算法](#保证一致性的算法)
        - [Raft 算法](#raft-算法)
        - [Paxos 算法](#paxos-算法)
- [BASE 理论](#base-理论)
- [分布式事务](#分布式事务)
    - [事务概念and选型](#事务概念and选型)
    - [选型比较](#选型比较)
    - [sharding jdbc](#sharding-jdbc)
        - [对比 概念](#对比-概念)
        - [使用 shardingjdbc](#使用-shardingjdbc)
    - [基于 xa 协议的](#基于-xa-协议的)
        - [2PC](#2pc)
        - [3PC](#3pc)
    - [TCC 失败补偿模式](#tcc-失败补偿模式)
    - [基于异步消息](#基于异步消息)
        - [只使用消息中间件](#只使用消息中间件)
        - [本地消息表 + mq](#本地消息表--mq)
- [分布式锁](#分布式锁)
- [分布式 id 生成](#分布式-id-生成)
- [并发请求数据冲突-超卖问题](#并发请求数据冲突-超卖问题)
- [幂等问题](#幂等问题)
- [限流](#限流)
- [负载均衡](#负载均衡)
    - [负载均衡算法](#负载均衡算法)
        - [轮询](#轮询)
        - [加权轮询](#加权轮询)
        - [随机算法](#随机算法)
        - [加权随机算法](#加权随机算法)
        - [哈希法hash](#哈希法hash)
        - [一致性哈希hash](#一致性哈希hash)
        - [最少连接算法（least connection）](#最少连接算法least-connection)
- [分布式 session 一致性问题](#分布式-session-一致性问题)
    - [为什么需要 sso](#为什么需要-sso)
        - [单系统中的登陆是怎样的](#单系统中的登陆是怎样的)
        - [多系统中的登陆](#多系统中的登陆)
    - [解决方案](#解决方案)
        - [session 复制同步](#session-复制同步)
        - [客户端存储 session](#客户端存储-session)
        - [NGINX 中对 ip 进行hash 路由](#nginx-中对-ip-进行hash-路由)
        - [后端集中 session](#后端集中-session)
- [怎么实现子系统间的通信 进程通信](#怎么实现子系统间的通信-进程通信)
- [灰度发布](#灰度发布)
- [分布式架构之soa](#分布式架构之soa)
    - [基于 sca 实现 soa](#基于-sca-实现-soa)
    - [基于 esb 实现中心化的 soa](#基于-esb-实现中心化的-soa)
    - [基于spring cloud 实现去中心化的soa](#基于spring-cloud-实现去中心化的soa)
- [微服务 micro service](#微服务-micro-service)
    - [为什么需要微服务](#为什么需要微服务)
    - [什么是微服务架构](#什么是微服务架构)
    - [如何搭建微服务架构](#如何搭建微服务架构)
        - [整体选型](#整体选型)
        - [微服务系统要素](#微服务系统要素)
            - [客户端访问服务 API Gateway](#客户端访问服务-api-gateway)
            - [服务间通信](#服务间通信)
            - [服务注册、发现](#服务注册发现)
            - [服务挂了怎么办-熔断降级](#服务挂了怎么办-熔断降级)
            - [运维的压力怎么解决](#运维的压力怎么解决)
    - [相关开源项目](#相关开源项目)

<!-- /TOC -->

# 什么是分布式系统and为什么需要它

- 为什么需要？

    普通的单机无法完成计算/存储量大的任务， 于是就将这个任务分解为多个子任务， 交给多台机器计算/存储， 最后汇总结果， 这样多台廉价的机器可以处理大计算量的任务了， 这就是分布式计算系统

    分布式系统要做的任务就是把多台机器连接起来，通过网络通信, 让其协同完成任务， 可以是计算任务，也可以是存储任务

- 区分两个概念：

    - 分布式：一个业务分拆多个子业务，部署在不同的服务器上; (强调更多的机器提供更强的数据处理能力, 保量)

    - 集群：同一个业务，重复部署在多个服务器上， 通过一个单独服务器在最前端进行负载均衡 (强调更多的机器提供更稳定的服务, 保质)

- 分类:

    - 分布式存储系统 

    - 分布式计算系统 


只有当单个节点的处理能力无法满足日益增长的计算、存储任务的时候，且硬件的提升（加内存、加磁盘、使用更好的CPU）高昂到得不偿失的时候，应用程序也不能进一步优化的时候，我们才需要考虑分布式系统。因为分布式系统多节点、通过网络通信的结构，会引入很多单机系统没有的问题，所以不到万不得已不要引入分布式

## 分布式存储系统

将数据分片， 存储到多个节点

1. 结构化存储
2. 非结构化存储 
3. 半结构化存储 
4. In-memory 存储

> 分布式存储系统有一系列的理论、算法、技术作为支撑：例如 Paxos, CAP, Consistent Hash, Timing (时钟), 2PC, 3PC 等等([参考](https://zhuanlan.zhihu.com/p/25074310))

- 结构化存储（structured storage systems）

  - 典型的场景就是事务处理系统或者关系型数据库（RDBMS, 如MySQL, PostgreSQL ...）

  - 传统的结构化存储系统强调的是: (正是由于这些性质和限制，结构化存储系统的可扩展性通常都不是很好，这在一定程度上限制了结构化存储在大数据环境下的表现)

    - `结构化的数据`（例如关系表）

    - `强一致性` （例如，银行系统，电商系统等场景）

    - `随机访问`（索引，增删查改，SQL 语言）

- 非结构化存储 （no-structed storage systems)

  - GFS (google file system) 则是做出了里程碑的一步，其开源实现对应为 HDFS.

  - 非结构化存储强调的是`高可扩展性`，典型的系统就是分布式文件系统; 相应的, 几乎无法支持随机访问（如随机 update, read）操作，通常只能进行`文件进行追加`（append）操作。而这样的限制使得非结构化存储系统很难面对那些实时性较强的应用。

- 半结构化存储 （semi-structure storage systems）

  - 是为了`解决结非构化存储系统随机访问性能差的问题`

  - NoSQL, Key-Value Store, 甚至包括对象存储，例如 protobuf，thrift 都属于半结构化存储

  - NoSQL 系统既有分布式文件系统所具有的可扩展性，又有结构化存储系统的随机访问能力 （例如随机update, read 操作）

- In-memory 存储(将数据存储在内存中, 从而获得读写的高性能)

  - 比较有名的系统包括 memcahed ，以及 Redis

## 分布式计算系统

传统的并行计算要的是：投入更多机器，数据大小不变，计算速度更快。------强调更高的性能

分布式计算要求：投入更多的机器，能处理更大的数据。-------------------强调处理更大规模的数据

1. 传统基于msg的系统 
2. MapReduce-like 系统 
3. 图计算系统
4. 基于状态（state）的系统 
5. Streaming 系统

对于Java来说， 分布式系统就是多个子系统分布在一台/多台机器的一个/多个 jvm 中;


# 分布式系统的特性and如何衡量

可扩展性 - 通过增加机器来应对数据量的增长，同时，当任务规模缩减的时候，可以撤掉一些多余的机器，达到动态伸缩的效果

可用性 - 任何时候都能对外提供服务

高性能 - 高并发，低延迟

一致性 - 为了提高可用性， 分布式系统会引入 replication (数据冗余， 即提供多个数据完全一致的节点)；一致性就是保证这些 重复节点的状态的一致； 一致性越强，对用户越友好，但会制约系统的可用性；一致性等级越低，用户就需要兼容数据不一致的情况，但系统的可用性、并发性很高很多。

# 涉及哪些技术规范

负载均衡: nginx

分布式缓存: redis

网络通信：每个应用手写 socket --> 通过 http(应用层协议) 提供 http 接口 --> `rpc` - 典型如 dubbo, grpc

分布式事务

服务注册发现 : zookeeper

消息队列: kafka、rabbitMQ、rocketMQ

分布式计算平台: Hadoop, storm, akka

分布式存储

日志：elk

# CAP 理论

## cap 介绍

https://mwhittaker.github.io/blog/an_illustrated_proof_of_the_cap_theorem/

![alt](Snipaste_2018-07-18_16-34-32.png)


`分布式系统一定要满足 P, 也就是在 网络分区容错的情况下, 剩下的两者无法同时满足, 这就是 cap定理` 



- Consistency 一致性 - 任何节点上的read 操作每次都能读取到最新数据

    如果要保证 "一致性", 势必无法保证 "可用性", 因为 数据在节点间的复制也是要耗时的, 这段时间内系统无法对外提供服务

    比如: 为了保证其他server的 "一致性", 当前server在完成写操作时候, 必须锁定 其他server的读写, 只有数据同步后, 才可放开, 这段时间内其他server不具有"可用性"

    放弃强一致性的话，则系统无法保证数据保持实时的一致性，等到数据达到最终一致性时，有个时间窗口，在时间窗口内，数据不是最新的。

- Availability 可用性 - 总是能够正常对外提供服务， 但是不保证响应的数据是最新的 

    假设一个节点挂，另一个备份节点要顶上, 也就是总能找到一个可用的数据副本

    放弃可用性的话，则在遇到网络分区或其他故障时，受影响的服务需要等待一定的时间，再此期间无法对外提供政策的服务，即不可用

- Partition tolerance 分区容错性 - 网络分区之间的通信可能失败, 即使故障时整个系统仍然能够对外提供服务

    大多数分布式系统都分布在多个子网络。每个子网络就叫做一个网络分区（partition）

    放弃分区容错性的话，则放弃了分布式，放弃了系统的可扩展性, 而分布式系统最常常发生的就是分区故障, 所以必须满足


以"write/read " 为例子🌰, 一个"非一致性"系统:

![alt](Snipaste_2018-07-18_16-43-02.png)

一个 "一致性"系统:

![alt](Snipaste_2018-07-18_16-43-42.png)



## 保证一致性的算法

### Raft 算法

用于替代 paxos 算法, paxos 太复杂了

https://www.cnblogs.com/xybaby/p/10124083.html

动图演示: https://raft.github.io/raftscope/index.html

Raft集群包含多个服务器，5个服务器是比较典型的，允许系统容忍两个故障。在任何给定时间，每个服务器都处于以下三种状态之一，领导者（Leader），追随者（Follower）或候选人（Candidate）。 这几个状态见可以相互转换

- Leader：处理所有客户端交互，日志复制等，一般一次只有一个Leader

- Follower：类似选民，完全被动

- Candidate：类似 follower 的 升级版，可以发起选举投票, 可能被选为一个新的 leader 




================选举Leader 心跳机制

Leader向所有Follower发送定期心跳以保持其leader权限

所有 node 以Follower的身份开始。 只要某个 node 从Leader或Candidate接收到有效的RPC请求，该node就会保持Follower状态

如果一个Follower在一段时间内 (称为选举超时时间) 没有接到来自 leader 任何通信, 从follower切换到candidate, 并发起选举. 若收到大多数赞成票, 升级为 leader 状态. 如果发现其他节点比自己更新 (更早成为 leader), 主动切换回 follower




================ 数据复制




### Paxos 算法



# BASE 理论

基于CAP定理演化而来, 核心观点是: 优先满足可用性, 牺牲一定的一致性, 保证最终一致性

- ba : Basically Available（基本可用）

    系统基本可用, 指分布式系统出现故障的时候允许损失一部分的可用性

- s: Soft state（软/柔性状态）

    即允许各个节点短时间内不同步

- e: Eventually consistent（最终一致性），

    数据最终是一致的，但是实时是不一致的

核心思想是即时无法做到强一致性，但每个应用都可以根据自身业务特点，采用适当的方式来使系统达到最终一致性

base 和 acid 区别:

- acid  - 是 关系型数据库中的事务特性, "一致性" > "可用性", 这里的一致性 指的是 "主外键约束的完整性, 不被破坏"

- base - 分布式系统中 "可用性" > "一致性", "一致性" 实际上是 "最终一致性";

- ACID BASE只是一个度的问题，并不是对立的两个极端

    分布式系统若遵循严格的一致性, 那就和单机系统一样 , 那就遵循单机事务的 acid

    若遵循柔性事务, 那就是遵循 base 理论



# 分布式事务

## 事务概念and选型

```
本地事务:  SqlSessionfactory   --》 一个数据库范围类事务管理.

分布式事务: 跨了多个数据库事务管理,在微服务架构每个服务都有自己数据库, 在分布式系统下, 将不同节点下的事务操作绑定在一起, 作为一个整体事务处理, 如电商下单场景, 库存和订单服务若分布在不同节点, 就涉及到分布式事务

刚性事务是指严格遵循ACID原则的事务, 例如单机环境下的数据库事务.

柔性事务是指遵循BASE理论的事务, 通常用在分布式环境中, 常见的实现方式有:

     ①基于 XA 协议的 2PC、3PC
     ②基于业务层的 TCC补偿型提交 
     ③基于异步消息的
     ④本地消息表+异步消息


TX-LCN LCN并不生产事务，LCN只是本地事务的协调工, 支持多种模式 https://www.cnblogs.com/SimpleWu/p/10922654.html

https://github.com/changmingxie/tcc-transaction 开源的TCC补偿性分布式事务框架

Seata 从两段提交演变而来, 提供了 AT、TCC、SAGA 和 XA 等事务模式

```

## 选型比较


2pc, tcc 具备强一致性, 基于异步消息的方案仅仅保证最终一致性

2PC的强一致性依赖于数据库，而TCC的强一致性依赖于应用层的Commit与cancel。因此, 实现起来, 2pc 更简单, tcc 需要手动在应用层编码, 但是也由更高的自由度.

## sharding jdbc

### 对比 概念

https://dbaplus.cn/news-11-1854-1.html
https://www.jianshu.com/p/bd61417ac63c

mycat 与之对比: 一个是jdbc proxy, 一个是db proxy; Mycat 是基于 Proxy，它复写了 MySQL 协议，将 Mycat Server 伪装成一个 MySQL 数据库，而 Sharding-JDBC 是基于 JDBC 接口的扩展，是以 jar 包的形式提供轻量级服务的


### 使用 shardingjdbc

https://github.com/apache/shardingsphere

## 基于 xa 协议的

XA 协议中分为两部分：事务管理器和本地资源管理器

其中本地资源管理器往往由数据库实现，比如 Oracle、MYSQL 这些数据库都实现了 XA 接口，而事务管理器则作为一个全局的调度者。

### 2PC

两阶段提交 (Two-phase Commit，2PC): 将分布式事务分为两阶段, 1. 准备: master 问各个 node , 是否准备好提交事务, 2. 提交: 若阶段 1 都返回 yes, 则 master 发送指令要所有 node 提交, 整体事务成功, 若不是都返回 yes , 则 整体事务失败, 成功的节点回滚

通过引入协调者（Coordinator）来协调整体事务 (node1, node2)

- 准备阶段 - coordinator 询问 node 准备好你的事务提交了吗, node 回答 yes/no

    这段参与者执行了事务，但是还未提交

- 提交阶段 - 如果事务在每个参与者上都执行成功，事务协调者发送通知让参与者提交事务；否则，协调者发送通知让参与者回滚事务; 

    参与者接收到协调者发来的通知后，才进行事务提交或者回滚

优点:

- 强一致性 (只要有一个节点失败, 整体都失败)

- 数据库原生支持, 对业务侵⼊很小，它最⼤的优势就是对使⽤⽅透明，用户可以像使⽤本地事务⼀样使⽤基于 XA 协议的分布式事务

但是问题不少:

- 同步阻塞 - 所有事务参与者在等待其它参与者响应的时候都处于同步阻塞状态，无法进行其它操作, 

    当调用链很长的时候，2PC的可用性是非常低的

- 单点问题 - 协调者在 2PC 中起到非常大的作用，发生故障将会造成很大影响。    

    特别是在阶段二发生故障，所有参与者会一直等待状态，无法完成其它操作。

- 数据不一致 - 在阶段二，如果协调者只发送了部分 Commit 通知消息，此时网络发生异常

    那么只有部分参与者接收到 Commit 消息，也就是说只有部分参与者提交了事务，使得系统数据不一致

### 3PC

三阶段提交, 从原来的两个阶段扩展为三个阶段，并且增加了超时机制: coordinator 和 node 相互发送消息后 若长时间没收到回应, 则做出某种处理; 

主要是为了解决两阶段提交协议的阻塞问题

## TCC 失败补偿模式

补偿事务, TCC是Try、Commit、Cancel的缩写, 对于每个事务, 都有 三个方法. 其核心思想是：针对每个操作，都要注册一个与其对应的确认和补偿（撤销）操作

本质就是在 应用层, 使用业务编码的方式, 保证一致性, 

对分布式事务中的各个资源进行分别锁定, 分别提交与释放, 例如, 假设有AB两个操作, 假设A操作耗时短, 那么A就能较快的完成自身的try-confirm-cancel流程, 释放资源. 无需等待B操作. 如果事后出现问题, 追加执行补偿性事务即可


分为三个阶段:

- try 阶段 - 对业务系统做检测及资源检查

    如: 转账, commit 前要首先冻结 双方账户

- commit 阶段 - 对业务系统做确认提交

    Try阶段执行成功并开始执行 Confirm阶段时，默认 Confirm阶段是不会出错的。即：只要Try成功，Confirm一定成功。

- cancel 阶段 - 在业务执行错误，需要回滚的状态下执行的业务取消，预留资源释放

    满足幂等, 可以支持重试




优点:

- Try操作可以灵活选择业务资源的锁定粒度，而不是锁住整个资源，提高了并发度

- 满足严格的一致性, 适用于 红包, 收付款业务

缺点:

- 代码很繁琐, 对应用的侵入性强。业务逻辑的每个分支都需要实现try、confirm、cancel三个操作

    confirm和cancel接口必须实现幂等

- 还是有数据一致性问题 - 第三步有可能失败, 造成回滚不成功

## 基于异步消息

### 只使用消息中间件

遵循  base 理论, 也就是 柔性事务, 通过将一系列同步的事务操作变为基于消息执行的异步操作, 避免了分布式事务中的同步阻塞操作的影响. 最终一致性

1. node1发送远程事务消息到MQ Server;

    保证本地事务的提交与消息发送这两个操作的原子性

1. MQ Server给予响应, 表明事务消息已成功到达MQ Server.

1. node1 Commit本地事务.

1. 若本地事务Commit成功, 则通知MQ Server允许对应事务消息被 node 2 消费; 若本地事务失败, 则通知MQ Server对应事务消息应被丢弃.

    若 node1 超时未对MQ Server作出本地事务执行状态的反馈, 那么需要MQ Server向 node1 主动回查事务状态, 以决定事务消息是否能被消费.

1. node2 从 mq server 消费被允许的消息

    如果MQ订阅方 node 2 执行远程事务成功, 则给予消费成功的ack, 那么MQ Server可以安全将事务消息移除;

    如果执行失败, MQ Server需要对消息重新投递, 直至消费成功.

消息中间件也需要支持 HAC 来确保事务消息不丢失. 还可能需要对消息中间件增加消息不重复, 不乱序等其它要求


best efforts

只使用消息中间件

事务失败, 则调用方 向队列发送一个失败消息, 各个 node 订阅这个 消息, 收到失败消息后, 回滚, 解锁...等操作

"最大努力"体现在: 调用方会以某个频率不断发送失败消息, 直到 各个 node 收到消息, 告诉调用发失败消息收到, 则不再发送失败消息

实现了最终一致性，不需要依赖本地数据库事务。

### 本地消息表 + mq

结合消息中间件和数据库表, 各系统本地的事务来实现分布式事务,  思路是来源于ebay

创建 本地消息表/操作流水表, 保证业务操作和流水插入在同一个事务中. 并且使用消息队列来保证最终一致性

流程: 

- 在某节点 node1 完成 写库后(子事务), 向本地消息表保存一条记录, 状态为 "发送中", 代表这个操作, 使用本地事务保证这条记录一定被写入

- node 1 同时向消息队列中投递一条消息数据, 同消息表中的数据

    消息恢复系统: 引入一个定时任务, 每隔一段时间去本地消息表中捞取状态为“发送中”的消息，然后重新投递到mq中间件中

- node2 订阅了消息, 开始消费 (幂等), 成功后, 反向向mq投递一条消费成功的消息到另外的 ack_queue 队列

- node 1 订阅 ack_queue, 开始消费这个反馈消息, 将本地消息表的消息状态改为“已发送”

- 新开一个服务, 定时轮询将本地消息表中的消息转发到 Kafka 等消息队列中，如果转发成功则将消息从本地消息表中删除 (or 不删除而使用 status 字段标注)

- node2 从 消息队列中读取一个消息，并执行消息中的操作, 回调通知 node1 成功则 删除 消息表记录 or 更新 消息表 status 字段; 失败则 node1 回滚

优点: 

- 避免了分布式事务，实现了最终一致性

缺点:

- 消息表会耦合到业务系统中, 开发量大


# 分布式锁

分布式系统下加锁, 

- 需要独立的外部存储空间

    保证两个节点都可以访问到

- 需要唯一标识

- 至少需要 2 种状态 (锁定 / 释放 状态)


有如下的解决方案:

- 借助数据库加锁 (行锁、version乐观锁)

    数据库表，字段为锁的ID（唯一标识），锁的状态（0表示没有被锁，1表示被锁）

    quartz集群架构中就有所使用

    问题太多, 一般不用

- 系统外缓存 如redis , 借助 setnx 指令

    问题: 为了防止主机宕机或网络断开之后的死锁，Redis没有ZK那种天然的实现方式，只能依赖设置超时时间来规避

    推荐 Redisson 这个 redis client, 提供许多分布式功能

- Zookeeper

    ZK中还有一种名为临时有序节点, 创建后会有一个编号依次增加
    
    而且由于是临时节点, client 断开就删除, 天然防止了死锁

    还用到了 watch机制 (唤醒等待的线程)

    过程是这样:

    1.  client create 临时有序 node (比如在目录 '/lock' 下创建 'node1')

    1. client 获取 /lock 下的所有 children node

    1. 比较 node1 和其他 node 的序号, 如果是最小的, 则获取 锁, 如果不是最小的, 则睡眠, zookeeper监测比 node1 小的前一个 node0, 若 node0 删除则唤醒睡眠线程, 即获取锁

    1. 释放锁, 即删除 node1


# 分布式 id 生成

使用数据库的自增ID，虽然简单，会带来ID重复的问题，并且单机版的ID自增，并且每次生成一个ID都会访问数据库一次，DB的压力也很大

- 基于 UUID, 不是自增, 不利于检索

- 为水平拓展的节点设置不同的起始 id 和相同的步长(步长为 节点数), 如共有三个节点, node1 的 id 会是 1, 4, 7..., node2 会是 2, 4, 8..., node3 是 3, 6, 9... 

    通过 sequence 设置

    这种方式缺点是 不好拓展.如果确定后期会扩容, 需要将步长设置大一点, 预留一些初始值给后续扩容使用

- 批量申请自增 id (号段模式): 一次性申请一批 可用 id, 快用完了再次申请, 始终保证内存中有可用的 id     https://github.com/didi/tinyid

    需要数据表配合: primary_key_table(id, biz_type, max_id, step, version), 
    
    biz_type 可选, 表示业务类型, 为不同业务做id隔离, max_id 表示当前可用 id 最大到多少,step 代表每次申请多少 id数量, 可以固定设置一个值, 一般为 1000 够了, 越大 qps 越高可根据业务类型决定大小; version 为乐观锁, 每次更新都加一, 保证并发更新的正确性

    流程: 查询 max_id 信息, 计算新的最大可用 id 为 new_max_id, 更新 new_max_id 到数据库, 同时 version + 1, 若更新成功, 则获取 id 成功, 为 (max_id, new_max_id], 若更新失败, 则号段可能被其他线程获取, 重试

- 中间件: redis 方式

    因为 redis 单线程的排他性, 保证的生成id 的唯一, 每次先+1再获取 (incr和increby )

    改进: 每次先+1000再获取, 拿到本地后, 0~1000 慢慢用, 用完了再获取 1001~2000, 一个用户服务宕机了，也顶多一小段userId没有用到

    手写工具类代码 
    TODO

- 雪花算法

    snowflake (雪花) 算法: 同时使用了时间戳、机器号、进程号以及随机数来保证唯一性。




# 并发请求数据冲突-超卖问题

比如电商的 "超卖问题": 并发的请求几乎同时到达, 因此页面展示可能来不及反应商品数量变化, 导致两个请求购买到了同一个商品

通过redis解决: 

- 下单成功后, 立即向 redis 中存储 商品编号 (可以设置过期时间)

- 每次下单前, 先到 redis 检查, 看看当前 want buy 的 商品是否存在于 redis, 若存在, 则下单失败, 否则下单成功

# 幂等问题

就是对接口的多次调用所产生的结果和调用一次是一致的

为什么需要: 分布式系统因为网络原因, 造成重试. 或者 用户重复点击提交

=============分析 and 解决方案:

- 在最前端控制

    比如: 页面进行防止重复点击

- 在系统设计层面控制, 每次请求过来, 先进行校验, 分场景进行强校验 or 弱校验

    - 涉及到金钱的进行`强校验`. 如: 本地数据库增加一个流水表 包括 (id, 订单号, 业务场景id), 借助本地事务保证校验一定完成.
    
        过程: 处理请求后, 在同个事务中增加一条流水, 以后再处理每个请求, 先到 通过 订单号, 业务场景id 到 流水表中查找, 查到则拦截, 查不到放行.

        另外, 硬盘中有持久化的数据心里有底:)

    - 其他不是很重要的场景做`弱校验`.如: 通过 redis +  sign 进行弱校验: 

        sign 是 通过请求参数+url 经过加密生成的字符串, 第一次请求完成后就保存到 redis, 后续若有第二次请求, 首先拿到  sign 和 redis 中的的比对, 若相同则证明是第二次访问了, 拒绝;

        是不是每个请求都需要在 redis 中缓存一个 sign, 那 redis 爆炸了怎么办? 不会, 我们会给每个 sign 设置一个过期时间, 这个时间和 请求认证拿到的 token 过期时间一致, 若 redis 中的签名过期了, 第二次请求也不会通过, 因为 认证 token 也过期了, 也会被拦截.

        如果不是分布式系统, 就不需要 redis ,token 直接放在jvm 即可, 如生成后放在 session 中

- 在接口设计时控制, 比如 将增量修改变为普通等量修改

    譬如说需求是：当用户点击赞同时，将答案的赞同数量+1。改为：当用户点击赞同时，确保答案赞同表中存在一条记录，用户、答案。赞同数量由答案赞同表统计出来//todo
    
- 在数据库层面控制, 比如 唯一索引.

    对于新增操作, 当然是对 业务id (比如邮箱) 设置唯一索引

# 限流

- Semaphore 控制并发数量

- 漏桶算法, 令牌桶算法控制访问速率

    Guava提供的限流工具类RateLimiter来实现控制速率，该类基于令牌桶算法来完成限流

- 分布式限流 redis (注解+aop)

- nginx 限流

# 负载均衡

ref: https://www.cnblogs.com/xybaby/p/7867735.html#_label_14

## 负载均衡算法

### 轮询

round-robin

各个节点逐个对外提供服务

没有考虑不同节点 的机器性能不同.比如, 也许同样数目的请求，高配的机器CPU才20%，低配的机器CPU已经80%了 

### 加权轮询

weight round-robin

在轮训算法的基础上，考虑到机器的差异性，分配给机器不同的权重，能者多劳

这个权重的分配依赖于请求的类型，比如计算密集型，那就考虑CPU、内存；如果是IO密集型，那就考虑磁盘性能

### 随机算法

random

随机选择一个节点服务，按照概率，只要请求数量足够多，那么也能达到绝对均衡的效果。而且实现简单很多

### 加权随机算法

weight random

在随机的时候引入不同节点的权重

### 哈希法hash

根据客户端的IP，或者请求的“Key”，计算出一个hash值，然后对节点数目取模。好处就是，同一个请求能够分配到同样的服务节点，这对于“有状态”的服务很有必要

只要hash结果足够分散，也是能做到绝对均衡的

哈希算法的缺陷也很明显，当节点的数目发生变化的时候，请求会大概率分配到其他的节点，引发一系列问题，比如sticky session。而且在某些情况，比如分布式存储，是绝对的不允许的

### 一致性哈希hash

http://www.zsythink.net/archives/1182

为了解决取余hash伸缩性差的问题, 一致性hash则利用hash环对其进行了改进。

每个节点 hash(ip), 按照顺序均匀分布在环形上作为虚拟节点(环形有刻度, 0 ~ 正无穷(0)), 接受到请求, hash(req_ip或者user_id)肯定会落到环形上的某个刻度, 顺时针找到最近的 node 处理这个请求

调整每个物理节点对应的虚拟节点数目，也就相当于每个物理节点有不同的权重

### 最少连接算法（least connection）

一般其他算法, 要么没有考虑到节点间的差异（轮询、随机、哈希），要么节点间的权重是静态分配的（加权轮训、加权随机、一致性hash）

# 分布式 session 一致性问题

sso (single sign on)

## 为什么需要 sso

### 单系统中的登陆是怎样的

web 应用采用 B/S 架构, http 作为通信协议, 是 stateless 的

client 首次请求 server, server 会创建一个 session, 将这个 session 的 id 返回给 client, 后续的 请求, client 都会带上这个 id, 这样后续的请求和第一个请求 就关联了

浏览器端使用 cookie 机制 来保存 session id, (cookie是浏览器用来存储少量数据的一种机制，数据以”key/value“形式存储，浏览器发送http请求时自动附带cookie信息)

如果 web 容器使用 tomcat, 浏览器中可以看到一个名为 "JSESSIONID" 的 cookie, 这就是 tomcat 中的会话机制维护的会话id

```java
//浏览器第一次请求服务器需要输入用户名与密码验证身份，服务器拿到用户名密码去数据库比对，正确的话说明当前持有这个会话的用户是合法用户，应该将这个会话标记为“已授权”
HttpSession session = request.getSession();
session.setAttribute("isLogin", true);

//再次访问时，tomcat在会话对象中查看登录状态
HttpSession session = request.getSession();
session.getAttribute("isLogin");
```

### 多系统中的登陆

单系统登陆核心是 cookie , 原样照搬到多系统登陆会碰到一个限制: cookie 的域（通常对应网站的域名）--> --> 浏览器发送http请求时会自动携带与该域匹配的cookie，而不是所有cookie

这对这个限制, 有个粗暴的解决方法: 将多个子系统的域名统一在一个顶级域名下, 如 "*.baidu.com", 然后将所有 cookie 的域设置为 "baidu.com"; 这样做理论可行, 但是会进一步带来更多限制 ==> 💊 子系统必须统一域名, 💊 子系统使用的技术需要统一, 不然 cookie 的 key  无法统一(比如 tomcat 的 为 JSESSIONID), 无法维持会话, 💊 cookie本身不安全.

单点登陆 ==> 单点登录全称Single Sign On（以下简称SSO），是指在多系统应用群中登录一个系统，便可在其他所有系统中得到授权而无需再次登录，包括单点登录与单点注销两部分


早先的架构是单一系统, 登陆只需要在唯一的系统中保存 session 即可. 随后架构演进为多系统架构, 在某一个子系统中保存 session 行不通了, 因为登陆进一个子系统, 然后进入其他子系统仍然需要登陆(因为其他子系统并没有保存 session ), 这明显很傻. 

单点登陆(SSO)由此而生: <span style="color:green">SSO 提供一个单独的认证中心, 其他子系统没有登陆入口, 只接受认证中心的授权(即令牌), 验证中心验证用户密码没问题则创建全局会话 , 然后创建授权令牌, 跳转到各个子系统, 子系统拿到令牌即拿到授权, 借此创建局部会话(这一步就和单系统登陆相同了).</span>

- hazelcast + spring-session 外部session解决方案
- redis + spring-session 方案
- nginx 做 ip_hash, 将请求路由到特定的 server, 请求的 session 位于集群的那个 节点 , 就将请求路由到那个节点, (容易产生单点故障问题)


单点登陆

[在线预览](https://www.draw.io/?lightbox=1&target=blank&highlight=0000ff&edit=_blank&layers=1&nav=1&title=sso-image.xml#R7V1tb6M6Fv41SLMfWvFmsD8maXLvlfaurnau7u5%2BpAlN0dBQETqd7q9f29hJODbBSUzKZKlGGmLAGHjOOc95sXGC2cuPX8rk9fn3YpXmju%2BufjjBg%2BP7vheF9D%2FW8lG3oCCuG9ZltqqbvH3D1%2By%2FqWh0Retbtkq3jQOrosir7LXZuCw2m3RZNdqSsizem4c9FXnzqq%2FJOlUavi6TXG39V7aqnkWr57r7Hb%2Bm2fpZXBojseMxWX5bl8XbRlzP8YMn%2FlfvfklkX%2BL47XOyKt4PmoK5E8zKoqjqrZcfszRnz1Y%2Btvq8Rcve3bjLdFOZnBA%2FJo%2BJtwwfl378FD66d37dw%2FckfxPPYvuxFd1uqw%2F5gOi4X9nm20v%2B9%2BwpzbMN%2FTV9TcvsJa3Sku7JRfMf%2B7bp%2B3NWpV9fkyU79Z1Ch7Y9Vy85%2FeXRTfo2q4SeUu5%2B53nyus0e%2BVVd2lKmy7dym31P%2F5lua9Cw1uKtYlea7cDAD2UvIV2JrnbP2eX9vmRLsZ0nj2k%2B3b21WZEX7PKbgt%2FQtiqLb6lspC%2FT5X%2B7PRIc7BJPWZ4fHLngf6yd3tUieclyJgt%2FpeUq2SSiWQDf88Vv3YWSPFtvaNuSvlH%2BENVXLF7P97Ss0h8HTeKV%2F5IW9AWUH%2FQQsfcuFvAT4hmT%2Buf7AdYl1J8PYB5J%2BUyEfK13Xe8xRjcEzAwhJzs9wJyCtwOUvBbZpuLXR1MHPQDYFWX1XKyLTZIfAm8PBvfWwdAq0cboCFEDHDskNNChggNbwIYfjlC4DArKazZBhykUAmIGhZ15s6onAp1pUvExmqYBaaOTTZPvNk0T1iFOAznP68U2qfpouy2cOXYmc2cS8o2Fgz1nHjpTuv3gzJEzpS3BCMubgmWIzmNMxPN6QCVSULnMM3ZzI%2BhuCXQRPk8XIhtUTAFdpKrCtKQ3M4LupkDnucACazlfX6gb%2Bf%2BV%2BX90Cv83pP87eFyChDEqMGgo7H5fAwvKULGKjXnkTJGDF5yTxw7GjIoT4kzwaKCGhszLgpf%2BmbTIc%2BM%2BsEhGPdVj9BKfCo8zo5ekn9C2SppHcFzRayfAaHk6t10FQ4z78Nq9WGO1sDONnWnANx6cyewLDyXNnInrzIlDVRsRCTiX27OZM603Hui%2Bvx3DEntK2TLJJ%2BK5PhZVVbzQHelmNWEpUtaWF8tvneAwfLHpap1%2BFUNJ88fifb5vmPIGukOO6tj73RZv5TJte4jidVVJuU7blIYUOzYkE5y4926MoqbeEJ5ImeZJRW12Y4Q6WIjO%2F2AifGCsvKatUrIk9c2Ks%2Fbgom8o%2BTg4TGiG1uvAsKmPQN4XHB%2B0jKvt%2BDACxzfzynSjHrHZUwibz6B4etqmlQNla%2FeezhS30TB%2Fpu6NfWCItTRN1b0SuXZ1r6%2FxGEYwXA0MGBQcIGQGhqgXUuapWJhz15F4fIMa5IlI7VDjzBxLz8FH8XKjxpYYmlG7NnOXPZbdEjObqZqdAHQkszayo5pEXGx87yL9dQZsHAONY0L55mTi4Nk%2BqxlMGPop6aTt85hFU6ZTnuF8cCSzHoo8qJA%2FFJG9xj6Uk%2BVb%2BX0XZEl%2FZNW%2F2fa9G0Xi93%2BYzr6PvUD8PlD3XJe3SlWnxFwoIkjqMEnHoKI0FRHkAg8JypolEUGQP0bH%2BaZyPOk4vuU%2BtCJoQ4BCNTz9j6JKFaGQscYN29kZRBRxQapIVRmqitcD65ynTxU7gXaVbdZ%2Fsn0Pd9EN0RALrMMDTlGgIx0a1hGgHlhHqLKOETADA4wfDgkw%2FgiYoQMmHJSGUQskR8AMDDBoUBpGrbYYATMwwMAasM8FjFp4OAJmYIBR6reuiRjVArG4wWTG%2FtUl0hTO84i3eP2F2OjTKz%2BYM%2B%2FeI%2FmT%2BfJ37r3rfVYIjo9CH0EA4QItAg4TXqEeARcGFeIAROxhuMw0qICjjo5aggpn%2BOWqQuIRXOyQOlA1ZaFcCql0u82KjS18DSVkJWHuHYLcvacvoA1vB1GuADeiXEGIjka5ujF6JL1KNVzcgMSdL4T9Qsx6LgKBH1hvZgpaz4UhpwAbofbUUNh%2BpgoYcuvIlBPEyMziyzCa3kd8WRdNPqL3kUMCB9eTZSYOCS%2BVSyF4n6LWtxQWlRzIMk%2BoolnK5kWWy9ugoxW%2FLhOpXVmTFfGBKv%2FsOLLnhvoAb5f07Huym%2FBQAelHuSCP1A5Ea7bNq24WDIAMklOHzA9tBetMnEQv1zgP4pVys6pJaktW45nsiz%2FVjHLyVhWS%2BqocV0%2BEm8w3dAxLfTuSs6jl1XUxRhuzPHWVvIjXPS34xsSZkGVRfMtSrkgWrPKJtk8eHIL3L%2Bu31Wjab820K7opjs7TTTGMuUATaMmwx8TVDrh1XEQ%2FruGYdW2lxKFdr9PFoz9nQaAulhYApvB8S46O92RJXIinH7GtlK3nK%2BA9MCT1ZG1e3IBJ0%2BTflm2PwUPWVEAHPdl2TxcOilhp85SwDeoBTObNZ8%2B9BEyc6Vz48dM9H5vx3TVj49SNqhymfih1cx08Z5qJ%2Bvvitdb%2BRchLVWYjObg1cqD4%2FQECfZzt9we4nyoxxY2XQzb2%2B%2BXIBkQQNPN1jxGE0fEfiuMPOHEIyw7PpgtKT9d1%2FD1dRHgsbHTOKWz8BBorS453pb9navU7DziPQT8kFoiRHG%2FrNBoYyLVMeVtmmDVDXDVPIkSuGePyCWZMDKiQhOo0M8m8EJuXRsWDhV8WbJudErIJ1oyz8YlskrNphIoJocemaNtR%2BXuXsMGBbIldQ7lLge5LtV8qMz6YahO4%2BB4kc03lBs6Y1HRlL3mnm6G1Bw5hGzj6oocZo%2FwPe2QytR5x6uEy%2Fc7ba1j3NmmyZwQOCE5BaA1Oalf24CQJ73HTrwBsNPefhzU4jyEIz6ShsP4%2FiPuJ8cJ5CXLApvMY5LhOvQ9bDMHXRXSVggnWgrjGRTyQ64pJ6UROiyMPo9QMSGoMJ5V3oi2Ei2D0JTUds83h8XJcp96HNalRQ8k6qYlYFBJznsKoRsClZsrXCY1ZUBPfXCDyqlKziyruYw6WBSn0z%2BQ5CgCRGcu5VJDkgI0FCXUc33IfHRFHqxEcbRGpZpmUbmm7mOoPLbz453O26dEPvcJqKABecEGunlZDkdcxXQ0FdVTkwdVQwmOpy06nSHkKnRHTK84c11bYqk4VFL0vOtvXo1PeFJDPF9jdlIK9pLoNSfVOkNTmKg1t1d9XX8sIGYeZTpdf1HKldgkGZwRdZ4QRlEJwRocUI7fzaQxKjnUlujwXz%2FJyUqADViyvEV1ZKME3OPEdiawFO9sbkUUeyGGfS2RR2E8yHBJTOWBTIivHdep9XJXI6rIxEWOxdWQFUwY7k7kX2spiknVqZvFFuJcTsot%2B9xzN%2Fj8XxJg0BWi3XINtwbS1UAu60kIt6MSFWtCJC7WgYwu1dOVPA%2FA%2B7IitriS9f4s4%2Bpr9%2BpqRS67EVvdXMmWrUdB1BmSrKCYXsVXN0xgUW9VlhnVpCpGdICz%2Bw6zqmKb4qcwjKPeJ%2FDNpqyLsqB%2FaCuyPHK9huY8c1al3MRzbKKuogFxO5w6JpZFE4vNo0wVPscesxptzV8w%2F0BByDjxxJH8ZLaahNFo2j7DKIzo3iQhX2VQ6srXKJtZfZzirbEqRGxcZ3smV3U%2FWdK3e1kR0bPjphwC6UmetfOJrNCOcD6f9XunPt4zwBVyCzdgLmlTCUkW7otBi2IepQoOWNIZzoWwpNAQHbKTQ7KosXbJ1LJe%2FIYrtA%2Fc6hpXwxmIBP4FquMLDqWIRAHc6Do5HlJTjOyJWbfcxIJaty7mafSiGnTb4KPEnGB3PjezwZh8q7Z4%2B6KKguusDLcB3xF0lcyCgFKOTpAAsXaA8hetO%2BjJfzR5xKfJHKzaselb3HqCRmIVqNXHOEHSFYU2frUQJIi2Dbh2bcgbuKt9rvRtbla2B%2BTfJxtK6gVkikHnDhit%2BnGyJPP11Wi0RmHiOO4rvEPg2ED5WvN1ZTKc8hStbIv2XaZtlNseK4%2FyRvV2RveErsTd8InsjJ7I3fBF7U57ClWVGl%2BzTlaaNdWmfFoyQOt1yOQw%2B99NesIyE9BSj03C2rgIX5Qzid5yhsDxiFgnUL%2FQCn4QVGdV87kitZVPr2JhBqydwL3hikDA%2FjAQ%2FhZ0bhbdDeMm5S8wpwms4AeJy4SUd3FVzRhd7VYU3Hpzw6ib9jhVtzk%2Fn4hHcVw0bdPL2VzJ18zzX6zoFenoEnVbEBn09zeO4LnPVfKJrLFMbkuWzlUOLmv16LtTT5kk08GkMF37u2lYWzYPi2cFC1RM6PiHfeivDSaRpvohmWK7my5XfH8v9ou9qy2ggB10C4rnYUg0I7elSObVri%2BjPsiiqw8PL5PX592KVsiP%2BBw%3D%3D)


有这样几个角色 : client/浏览器, sys1, sys2, sso认证中心

1. 用户访问系统1的受保护资源，系统1发现用户未登录，跳转至sso认证中心，并将自己的地址作为参数
1. sso认证中心发现用户未登录，将用户引导至登录页面
1. 用户输入用户名密码提交登录申请
1. sso认证中心校验用户信息，创建用户与sso认证中心之间的会话，称为全局会话，同时创建授权令牌
1. sso认证中心带着令牌跳转回最初的请求地址（系统1）
1. 系统1拿到令牌，去sso认证中心校验令牌是否有效
1. sso认证中心校验令牌，返回有效，注册系统1
1. 系统1使用该令牌创建与用户的会话，称为局部会话，返回受保护资源
1. 用户访问系统2的受保护资源
1. 系统2发现用户未登录，跳转至sso认证中心，并将自己的地址作为参数
1. sso认证中心发现用户已登录，跳转回系统2的地址，并附上令牌
1. 系统2拿到令牌，去sso认证中心校验令牌是否有效
1. sso认证中心校验令牌，返回有效，注册系统2
1. 系统2使用该令牌创建与用户的局部会话，返回受保护资源

用户登录成功之后，会与sso认证中心及各个子系统建立会话，用户与sso认证中心建立的会话称为全局会话，用户与各个子系统建立的会话称为局部会话，局部会话建立之后，用户访问子系统受保护资源将不再通过sso认证中心

单点注销

[在线预览(包括登陆和注销)](https://www.draw.io/?lightbox=1&target=blank&highlight=0000ff&edit=_blank&layers=1&nav=1&title=sso-image.xml#R7V1dc6s4Ev01VN19SIovAXq0HWf2YWdrajO1M%2FOIbZJQl5gsJjfJ%2FvqVQALTEkZgYXOz5CVYICGkPt1H3Y0wnNXLxy9Z%2BPr8a7qLEsM2dx%2BGc2fYtm15LvlHSz7LEuT4ZcFTFu%2FKIqsueIj%2FG7FCk5W%2Bxbvo0LgwT9Mkj1%2Bbhdt0v4%2B2eaMszLL0vXnZY5o07%2FoaPkVCwcM2TMTSP%2BJd%2FsxKLdOsT%2Fw9ip%2Be2a0DxE5swu33pyx927P7GbbzWPyVp19C3ha7%2FvAc7tL3oyJnbTirLE3z8ujlYxUldGz5sJX17lvOVv3Oon2uUsHfhJvQ2rqbre0%2Fuhvzxi5b%2BBEmb2wsDp8H1uwh%2F%2BQDRPr9Sg%2FfXpJ%2FxI9REu%2FJr%2BVrlMUvUR5l5EzCin%2Bry5bvz3EePbyGW1r1nYgOKXvOXxLyyyKHZDbzkFTJqt9JEr4e4k1xV5OUZNH2LTvEP6J%2FRYdSaGhp%2BpbTO60qYSgupZMQ7VhT1TibRbsv8ZYdJ%2BEmSpbVrK3SJKW336fFAx3yLP0e8UIymWbxV53hwkFv8RgnydGV98UfLSdPdR%2B%2BxAnFwr%2BjbBfuQ1bMBN%2By2W%2FZjcIkftqTsi2Z0WIQxSlm0%2FMjyvLo46iITfkvUUomIPskl7CzNz4TPwZPH5c%2F349knYv685GYWy4HaMgA9lS1XQsZOWBypihzvNEjoRME7khMXtN4nxf3R0sD3QG5S7P8OX1K92FyLHm1NJhfXRpaIa0sHi5qSEclCg3xEKUj0CAbtjuLwnmiIEyzinSoioKD1UShsm%2FnyII7y8KUZcEyPUtJGBwXny8MnoKNINzgR2Xxo%2F1uQZkgHZUkPBzoZB6TjWqyrb5TeDQn1UhHO4FQ9h%2Fno3FEknHkZVmUhDnhQE3WKxlbdoffKDDqafRMMI0YTM8hfcu2Eat1zB1BQ8hrNmQ5oKE8zJ6iXGiITEv4eXQZA25rhy2npcdtHasMFuxZLWtlH2rJq2ZhGINxZKxZFNCZNU%2BIJ%2FVmzTYQq0BmC6Ws2dHBmtFsDS9rDZ1e1tBxFKnRGCsokSkdDqmxDozF2li4xcG9EVjG2jWW5PjOWCNjSUqcWUVNTUrPUlEuGrqwN0%2FwiMFiGYgqa%2B0ZS2QE94VM%2BkYQUFEk9nwRzKL4pUTxprKPvc0lGkNF4tl8juhjCvqKx0Afk%2BWMIhyWN0vHNc0WbkqDbcnslkwaTi1%2Fh0uDL7FbgbH0jaVTHNwZi9W3gkytjIVprLFBlBtmkRKzsGgrY1ke3JFzfzslTHSY4m2YLNjAbtI8T1%2BaHoxNkm6%2Fd0qH4sxSX8UD60qUbNL3dV2wLArICd6rUxNcOgraBpHNV%2BkE6MAdcJ%2B0Cop5a%2FrQ7cC475lekaq%2FXAShN7vFK9LXmQEXkdyr0%2BbLcFr61Xa9C9w7IADYdH10joLbHIP08fEQ5QbE1pnOE4vri2O4FRwRW8UBwd2CrWEIBimDtIzgpIL%2BopjCimjRC41qycqbxWrQEKULOvL8cRyGN9DF6Z9w%2F00DA46EgBCzslgYwapevjsLKv3EtpDytU%2BXTctlsZS%2FM7gFnQoeRJE%2FhkhNkY5x0vTff8T5n%2FT41vQ89vsvSpJufcthv4%2F4VUGeWlHViZgzIYK4DuNa1xsIEWQCJgSxpgkiCJoJ77RZEa7HHde3PMdoHnhXjA%2F9M80jARTcqbCnJzu9BcwBQBSpiKE8fT2iw0n0mNMKpKl4%2F%2FQ7PXd3430h3t%2BKLXWaD%2BMyUo%2BpLICI2vE43GUqso5ZYCYmMLY7JYGxZ4GZusC4k9IwYlR4FpiJCQy6ooaxJBHdmfMP4%2Fwn82%2FG4fx8NV6tikELyqtiC1BlmC2gifK7cBVvnewWuJz3SheBb%2FOx3hvYLfymSwOvqcATacd4m8RR0bnCxUphQEDiio5Ww14xB%2BxyReFBDqjTaFVUcWmQkWApKFy51LEUFHUFUFEQWjRMeSa6GHxIz7PPP5m4Fj%2F%2BUtKwirA7EPnIIaCLwvs44d0lvWK%2FrosZonGBvg1ugSpVxQ0MGkiaakHOEHGVhA9rwcH0IPC%2BycWMLH2Du1oyqVr36JULk%2Br3orwU69HCBiNL4ITEyXG1iZPYlD5xskVvhcT0CwI2m%2FvryRp08TmuO0zQoGvM8V0lMTvXxcc7rOri4%2F3q%2Bxy6GIItOmiY0calrV4WYSFSggqNi4qwkMnCsphHjPDdjJoJoUYxrNopbcKbaGOhpiPeCq%2Fn%2Fer7HNpQI3qpZKgh9GRdJN95BdVwCtQsi1xR3wgwNUMzaoajpkpXsEye6KQZSK49kOcIAojUWM65QOIdVgYS6ri%2B5Tk6grxaw7i26OGTJgp1o%2B1sqs%2FwdJVkhsYKlL1s9ftzvB9xHXqBfCAgXr6a4To3H4jfRzUfCJn98oFct08%2BEFwUCaPQoiyuklRhK%2FlTBeh9k9m%2BERflTYBcH7CVP79GqtlAquJbjpIEpnKar4JeiBJVN1N%2F%2FKKWO7UjGNRwumq4HkQhqNGBYmR2jsakcCzLhfKou2xh1YB2FoZtSqBbBkvIMT0oiO9MZDXY2dGILOKu1XOJLIKvmY9EZHmHVYks71ff57gokZVFYzzKYkvPSkAY7IrHXkgp9UmWoZn7b2x5ucCV93tkb%2Fb%2FORB93ARQlcmkG5i6chjRhXIYUc8cRtQzhxGdymHsip86YD70wFb2guX4FnFea4671vRMfCG2Wt9Jla16TlcNyFaRj89iq5LRmBRblUWGZWEKFp3A1P9DreocpvipzCNI9%2FHsgbRVADsah7YC%2B8P7q5juw3vV9ymmYxt5FhXA5XJtYJ8bScS2yFjeFyF2nxyX3DUoNilwCw68MDh%2FmS2mIho1m0eY5eENDSLCF9CEhnS9gBbI7zOdF9A45OYX3itc6d1NqOvFhqZE%2B4rbHzhwKTUk79ixJZoRvl0r3b%2Fs53vD9gwucUMIv9OkEnxKdCs0H7ahqtCgJfXh7lK6FBqCHVZSaHpVlizYOqfLfyGKDXeU9GEmvDIs4DZY0JWlCRYOWE77zmmPknB9h8eq7TkmxLJlMVe1rVJotcl7ia9gdCzT08Obbai0R9rSRJDqri1KwNox6EqZAw4lH%2FVCATZPj0KnM0kvYJQ3ekAFiuzZik0rn9W8BdKI1Vy1Ej%2BnC5oKYE6frkAJwi2dbu2bUCPoSt9rfRpdma2O%2Bq5cc2rdxCwRiLwF7kiWyJLfp9USgZ2Ngo7kOwS%2BmxKcSt7uTKYTRuHClki%2BO2szzeZUcpw9s7cLsrfgQuwt6MnecE%2F2FpzF3oRRuDBmZME%2BWWranJd2NWcE1%2Bma02GCobvewTQSPJKPTsLZuhJchBrY7qghsDys5gms8g8aleFIaMGoZCcwMZdNzGOjBq18gfu%2BCAxiug7Dzk9h52bwdoAXKxLObvAqvgBxPnhxB3eV1OhiryJ4%2FcmBV%2FbS75zRZvx0SzwcjJXDBhd59Z1Ul3mWaXVVgSs9jPolscG1nmQ4LstcJbvXzWlqU7J8umJoXrNdy4R6Wj2IBj%2FRAD%2B9oCuKZkF4drBQsULHJuqtjzKdQJpks0DFdDVSy0voRn%2BbjBw95YWAiiWzgZx0CohlBppyQEhL5%2BJ0bFuk8LWVZvYYTxirv9XJE79siXVhwnEk4g3JXBWN2eZDuCdPa%2F76wGWU3V34OmhtI1DQNBF2cKaJUNnwXxTPy3wo9MYHImrDPVeU8y6hsFuKCRlDhEvmohO%2BHcHeHq3cApi%2BuRaY3w4R0S7pvmhPXOTn0UfeFKyMfiMsrD8eJiZKhm95yvdsNYTNWeU7uDa3bHUNLTmHNxbfaKqaBc45jrMOHYlA6fhmITfBs%2Bt0uhxUl%2FfFBt4Xc6j3BcYaRtoaFXmCL6VjWw1JjY7cL2QJ3pdTW6pexfviSbwvjEoSlbRvYM77z1uaMzjclAqO6lTLef2oT1b0k33Pxzv6no8YVyzvRDpZ3qyFuV5PCUu%2F%2FH1eHngANHIgfvgMehwHfT1dsvofyrgsiS4dj3GZdkMhYhO3KER9m5JeiF1ZkF1xF03%2FnUtN2NJo7MqTJQ0osyuV5JxpwNvWg28LAYBX%2B%2FN1bTCvg3J5EioMh7tJRIRdipoz0cBor1dzmpqzhrfTXFBZfhvfqbYwtm6LTx%2FUuxjfOoRasgJ9LOkySgAB94JlQx%2BbeqAKKoGxdl2Aq4iuLxgiF1bQ%2FS0oecAXrCxEvlFpqY7FxZfSSAgJxsITNBIeSSH5Cm8dzgrpugoJGCxn8NoNRM4tR5GV9FdIWN7ldoUEK5zaE3GIQpKwbYEMDdq5epAqgu%2BvQu3zEu92yQnvPkHSY%2FHXRyNt0%2F0%2B2uasZ0alPhpfEe%2F0vruO33wljO%2FCMFTMtS6XfUnMZlZn01Znuja1slyoGEdTZ13bWkF15pqa1ZkrUWeiP0eWYsc%2B0wNz7b66XnN%2Bcr2mMT4H1B5zrWtzFAnO%2BEYAuEfkeOqaDL4KbLlD9810sX8LdRlsS5%2FDyJe9tVVRII9vyjwg7%2BhLLcscvgNnrcSZ1r3Eskzi1PvqeD%2BOuTU9zNzhrCekXzo%2FrqY1XBHrQxkQTGkUW9KoNWRB%2FFlrCOEjDFxrrhtcTGsEErfbV9caU2cJNzBMbKGh6Z8C3IWW9ME9kPgFhVe7ZV%2Fs5N%2FFa%2F1i55eCuwcWs8gU83c0oZ38zFIawq9nNAtfn39NdxG94n8%3D)


sso认证中心一直监听全局会话的状态，一旦全局会话销毁，监听器将通知所有注册系统执行注销操作

1. 用户向系统1发起注销请求
1. 系统1根据用户与系统1建立的会话id拿到令牌，向sso认证中心发起注销请求
1. sso认证中心校验令牌有效，销毁全局会话，同时取出所有用此令牌注册的系统地址
1. sso认证中心向所有注册系统发起注销请求
1. 各注册系统接收sso认证中心的注销请求，销毁局部会话
1. sso认证中心引导用户至登录页面



## 解决方案

### session 复制同步

利:

- Tomcat 原生支持, 只需要修改配置文件, 无需改动代码


弊

- 复制操作会消耗机器额外性能

### 客户端存储 session

将 session 加密后存储在 cookie中, 每个 client 都存储自己的 session

利

- 服务端无需存储

弊

- cookie 有大小限制
- 每次 http 请求都得带上 session


### NGINX 中对 ip 进行hash 路由

反向代理层使用用户ip来做hash，以保证同一个ip的请求落在同一个web-server上

即Nginx 四层负载均衡方式，其实 Nginx 还可以做到七层负载均衡方式，也就是使用 Http 协议中的一些业务属性来做 Hash，常见的有 userId,loginId等等


### 后端集中 session

比如 session 集中存储到 redis, 可以借助  spring-session 集成

利

- 应用水平拓展方便

弊

- 需要对相应的代码做出修改，这样复杂度就变高
- 每次请求都需要调用一次 Redis ，这就增加一次网络的开销




# 怎么实现子系统间的通信 进程通信

- springcloud 支持 restful api 调用
- dubbo可以支持多种通讯协议
- 自己封装
    - socket 编程
    - netty 框架
- 使用rpc 协议规范, 具体实现如 grpc, thrift
- WebService 协议规范 -  语言无关的系统通信标准;  server端提供的 http 接口 发布生成 wsdl 文件, 和应用一起放入 http 服务器中发布, client 根据 wsdl 文件 生成辅助代码, 这个代码作用是将请求信息封装为标准的 soap 格式数据, 然后发送到服务端, server端反射调用相关类;
    jdk6 集成了 webservice (@WebService 注解)

# 灰度发布

将服务器集群分成若干部分，每天只发布一部分机器，观察运行稳定没有故障，第二天继续发布一部分机器，持续几天才把整个集群全部发布完毕，期间如果发现问题，只需要回滚已发布的一部分服务器即可

# 分布式架构之soa

soa: service oriented architecture 面向服务架构. 各个子系统 (子系统可以各不相同, 架构, 语言...) 间以统一的 "服务方式" 进行交互.

soa 并没有给出具体的实现方式， 目前业界有两套实现方式：去中心化 （spring cloud， dubbox） and 中心化 （esb）的方式


引入 soa 会带来新的问题:

- 网络延时, 尤其是多级服务调用.

    在每一级服务执行完需要检测是否超时, 如果超时, 就直接抛出 超时异常而不要继续下一级服务调用

- 各个服务负责人不同, 调试跟踪困难

- 系统的安全/监测, 流量控制

- 老系统改造移植

## 基于 sca 实现 soa

sca (service component architecture) 是几大公司 指定的 实现 soa 架构的具体实施规范

sca 涉及内容:

- 如何发布服务
- 服务符合调用
- 支持的通信方式

Tuscany 框架 基于 sca 规范， 可用来实现 soa 架构

## 基于 esb 实现中心化的 soa

和 sca 不同 （sca 是多个厂家联合指定的 soa 实现标准）， esb 只是一个概念 （check ： https://www.slideshare.net/MuraliMalli/mule-esb-demo）

esb核心内容如下： 主要思想就是基于消息中间件 （总线） 实现系统间交互

- 要有统一的消息格式
- 消息路由 - 总线根据传进来的消息决定调用哪个子系统
- 支持多种消息交互类型 - 比如 请求/响应 （适用于 同步请求）， 发布/订阅 （适用于 异步的消息广播）
- 支持多种网络协议 - 比如 tcp/ip, udp/ip, http
- 支持多种数据格式 - 消息中的数据格式可能不一致， 总线要能够在不同的数据格式间转换

Mule 是 esb 实现框架之一， 可以方便的连接各个系统 ， 基本概念check http://www.cnblogs.com/liangqihui/p/7905310.html

## 基于spring cloud 实现去中心化的soa

也就是微服务

# 微服务 micro service

## 为什么需要微服务


传统的web开发方式：所有的功能打包在一个war包，部署在一个容器

* 优点：开发简单，集中式管理不会重复开发，没有分布式的管理开销调用开销

* 缺点

    * 开发效率低：所有的开发在一个项目改代码，递交代码相互等待，代码冲突不断
    * 技术选型单一: 比如只能使用Java, 无法在一个系统中使用多种语言
    * 部署不灵活：任何小修改必须重新构建整个项目，这个过程往往很长
    * 扩展不灵活: 水平扩展(即前端加一个 load balancer)会带来资源浪费(如: moduleA, B 资源占用小, C占用多, 无法单独扩展C)

## 什么是微服务架构

- 将原来的单体app 根据业务划分为多个服务
- 每个服务可单独部署
- 通过暴露出来的api互相调用(http, rpc)
- 每个服务需要保证高可用(每个服务需要有候补节点)

优缺点:

* 优点：开发简单、服务独立无依赖、拓展方便
* 缺点：多服务运维难、通信成本、数据一致性、系统集成测试

![微服务架构图](Snipaste_2019-01-24_16-43-41.png)

一般架构是这样:

1. 负载均衡层
1. 网关(内部gw, 第三方gw)
1. 业务服务层 (如订单服务, 库存服务, 用户服务)
1. 支撑服务 (注册发现, 配置中心, 日志聚合, 监控告警)
1. 平台服务 PaaS( 发布系统, 镜像治理, 资源治理)
1. 基础设施 IaaS (计算, 网络 , 存储...)


## 如何搭建微服务架构

### 整体选型

https://www.infoq.cn/article/micro-service-technology-stack
https://zhuanlan.zhihu.com/p/94488453
https://blog.csdn.net/hardworking0323/article/details/81170961

https://blog.csdn.net/mulinsen77/article/details/84583716
https://www.cnblogs.com/waterystone/p/4920797.html
https://www.cnblogs.com/shijiaqi1066/p/3412346.html


### 微服务系统要素

<img src="Screenshot_1.png" width=50% />

1. 服务注册、发现、负载均衡，容错（服务注册中心->兼配置中心）
2. 服务间的通信（消息模式，RPC模式）
3. api网关（路由，监控，安全认证，日志，限流）
4. 微服务周边设施：日志中心，监控中心 ，文档自动生成

#### 客户端访问服务 API Gateway

[网关选型](https://blog.csdn.net/tianyaleixiaowu/article/details/78739661)

* 提供统一服务入口，让微服务对前台透明
* 聚合后台的服务，节省流量，提升性能
* 提供安全，过滤，流控等API管理功能


#### 服务间通信

* 同步调用

一致性强，但是性能稍差，如：  
REST（JAX-RS，Spring Boot）：容易实现、使用范围广    
RPC（Thrift, Dubbo）：传输协议更高效，安全可控

* 异步消息调用

数据一致性减弱，后台服务需要实现幂等性；  
(Kafka, Notify, MetaQ)


#### 服务注册、发现

* 在微服务架构中，一般每一个服务都是有多个拷贝，来做负载均衡  
* 服务发现基本都是通过zookeeper等类似技术做服务注册信息的分布式管理。

#### 服务挂了怎么办-熔断降级

* 重试机制
* 限流
* 熔断机制
* 负载均衡
* 降级（本地缓存）

#### 运维的压力怎么解决

docker

## 相关开源项目

* Spring Cloud：提供一组工具，构建分布式系统中的common patterns，如：配置管理，服务发现，网关

* jhipster：代码生成工具。可以生成整套微服务架构项目基础代码，后端使用spring（spring cloud ,spring boot等），前端使用AngularJS。

    * jhipster-registry：基于 Spring Cloud Netflix Eureka 和 Spring Cloud Config，实现服务注册、发现，配置管理等

    * jhipster-console：基于ELK，实现日志中心和监控中心。

* spring cloud alibaba + dubbo



备注：

注册中心-JHipster Registry

网关--JhipsterSampleGateway

分布式日志和全文检索--jhipster console  

Jenkins持续集成

sonarqube代码质量控制

mongoDB

分布式文件系统CEPH   API：http://docs.ceph.org.cn/radosgw/s3/java/

jhipster各组件启动顺序：  

1. 启动jhipster-registry --服务和网关依赖注册中心，如果不先启动注册中心，服务和网关启动不了。
2. 启动jhipster-console--如果使用这个控制台的话需要优先于具体服务启动
3. 启动具体服务--例如订单服务，商品服务。服务启动时会把自己注册到注册中心，方便别的服务或网关调用
4. 启动网关--这里的网关指jhipster生成的gateway类型的项目，是一个基于angular的前端