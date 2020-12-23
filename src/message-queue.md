---
title: Message Queue
tags:
  - mq
date: 2019-04-13 19:23:02
categories: distributed system
---

<div align="center">
https://www.zhihu.com/question/43557507
https://www.jianshu.com/p/79ca08116d57
消息队列（Message Queue）是一种应用间的通信方式, 是一种应用间的异步协作机制
rabbitmq：https://www.rabbitmq.com
</div>

<!--more-->

<!-- TOC -->

- [总结](#总结)
- [mq 是什么](#mq-是什么)
- [为什么使用mq-优缺点](#为什么使用mq-优缺点)
  - [带来的好处](#带来的好处)
  - [引入mq带来的问题-怎么解决](#引入mq带来的问题-怎么解决)
    - [造成整个系统可用性不好](#造成整个系统可用性不好)
    - [解决消息传递可靠性](#解决消息传递可靠性)
      - [如何防止重复消费](#如何防止重复消费)
      - [如何防止消息丢失](#如何防止消息丢失)
      - [如何保证消息顺序](#如何保证消息顺序)
      - [如何防止消息的大量积压](#如何防止消息的大量积压)
      - [如何防止消息过期失效](#如何防止消息过期失效)
      - [如何保证分布式最终一致性](#如何保证分布式最终一致性)
- [有哪些产品可供选择-选型](#有哪些产品可供选择-选型)
- [RabbitMQ](#rabbitmq)
  - [AMQP协议](#amqp协议)
    - [概念](#概念)
    - [工作过程](#工作过程)
    - [消息确认](#消息确认)
  - [基本介绍](#基本介绍)
  - [几种消息模型](#几种消息模型)
    - [hello-world](#hello-world)
    - [worker-queue](#worker-queue)
    - [发布订阅](#发布订阅)
    - [rpc](#rpc)
  - [使用 rabbit](#使用-rabbit)
    - [消息的序列化](#消息的序列化)
    - [发送](#发送)
    - [接收](#接收)
    - [持久化](#持久化)
    - [消息确认机制-手动确认-回调](#消息确认机制-手动确认-回调)
      - [消息发送确认](#消息发送确认)
      - [消息消费确认](#消息消费确认)
    - [使用 amqpAdmin 集中配置 rabbitmq](#使用-amqpadmin-集中配置-rabbitmq)
- [kafka](#kafka)
  - [kafka 简介](#kafka-简介)
- [即时通讯 MQTT协议](#即时通讯-mqtt协议)

<!-- /TOC -->

# 总结

```java
场景: 解耦, 削峰, 异步(提前返回)
模型: producer, consumer, queue(exchange, queue)
    exchange type: fanout, direct, topic, headers
        topic: *号, #号
    binding: 绑定exchange和queue, 设置 bingdingKey,发送数据时需要同时发送bindingKey
特性: 消息确认(msg ack), 持久化, 公平发布(fair dispatch)
    msg ack: 每个consumer 消费完消息, 会发送一个回执给 rabbit, rabbit然后才能删除队列中的消息, 否则 rabbit 会尝试发送这个消息给其他 consumer
    持久化: 
    轮询机制
带来的问题: 额外的组件引入复杂性; 暂时的不一致性(最终会一致)

```

# mq 是什么

消息队列：在分布式系统中传递数据

涉及三个概念 -- producer，consumer，mq中间件；

# 为什么使用mq-优缺点

使用的大前提: 生产者不需要从消费者处获得反馈/容许短暂的不一致性, 其实就是容许异步

## 带来的好处

- 系统解耦

  上游系统将数据发送到 mq, 下游系统无论有多少个, 只需要订阅自己感兴趣的主题即可.

  案例: 系统a的数据需要主动发送给下游系统b消费, 那么a 直接调用b的接口就好了, 现在需求更改: 又有系统c, d需要这个数据 (也就是一个 producer 有多个 consumer), 那么系统a需要修改, 加入调用c, d 接口的代码; 现在引入  mq, 上有系统a发送数据到 mq, 下游系统bcd需要数据, 直接订阅感兴趣的 topic 即可.

- 异步调用 (提早返回)

  一个请求过来，会执行一系列动作，如果这些操作都是没有返回结果的那么可以直接都放入消息队列执行,请求立刻返回
  
  如果这些动作可以中间一切分为两类，一类是有返回结果，二类是没有返回结果，引入mq后，一类操作执行完直接返回， 二类操作进入mq中慢慢执行，这样整个请求速度就加快了；
  
  比如：外卖订单的例子，执行链条中可分为两部分，一是创建订单，通知饭店接单（执行完直接返回）， 二是找到骑手（耗时操作，放到mq中执行）；

- 承载激增的大流量, 慢慢消化 (流量错峰)

  系统的请求不均匀，流量高的时候，请求先进入mq，在流量低时慢慢消化处理 ，用有限的机器承载高并发请求


## 引入mq带来的问题-怎么解决


### 造成整个系统可用性不好

引入 mq, 会造成整个系统 `可用性不好` （因为 mq 中间件服务器可能会挂）

那么怎么做高可用?

- 做集群 -  以rabbitmq为例, 集群有两种模式 

  署多个rabbitmq节点,消息数据只存在于一个节点,但是元数据存在于每个节点, 消费者如果连接到一个空节点,这个节点会根据元数据到queue所在节点同步queue数据; 实际上,还是没法保证高可用,因为queue数据还是只存在于一个节点, 这样做只是提高了吞吐量

  镜像集群模式(可以保证高可用,没法保证可拓展): 集群中的每个节点都有queue数据, 写消息时会自动把消息同步到每个节点; 怎么开启:RabbitMQ 有很好的管理控制台，就是在后台新增一个策略，这个策略是镜像集群模式的策略，指定的时候是可以要求数据同步到所有节点的，也可以要求同步到指定数量的节点

- 使用分布式消息队列 Kafka 

  Kafka 一个最基本的架构认识：由多个 broker 组成，每个 broker 是一个节点；你创建一个 topic，这个 topic 可以划分为多个 partition，每个 partition 可以存在于不同的 broker 上，每个 partition 就放一部分数据。

  Kafka 0.8 以后，提供了 HA (高可用)机制, 每个 partition 的数据都会同步到其它机器上，形成自己的多个 replica 副本。所有 replica 会选举一个 leader 出来(一个挂掉了在选举一个)，那么生产和消费都跟这个 leader 打交道，然后其他 replica 就是 follower。
  
  写的时候，leader 会负责把数据同步到所有 follower 上去(一旦所有 follower 同步好数据了，就会发送 ack 给 leader，leader 收到所有 follower 的 ack 之后，就会返回写成功的消息给生产者)，读的时候就直接读 leader 上的数据即可
  
  (只能读写leader, 要是你可以随意读写每个 follower, 就有数据一致性问题), 只有当一个消息已经被所有 follower 都同步成功返回 ack 的时候，这个消息才会被消费者读到。


### 解决消息传递可靠性

消息`可靠性不好` （消息丢失，消息重复/幂等性， 大量消息积压, 消息过期失效, 怎么保证消息顺序性）

#### 如何防止重复消费

什么时候出现: 

- 为了确保消息被成功消费, 开启 "手动确认", 如果消息已被成功处理，但在消息确认过程中出现问题，那么在消费端重启后，消息会重新被消费。

- 发送端为了保证消息会成功投递，一般会设定重试。如果消息发送至RabbitMQ之后，在RabbitMQ回复已成功接收消息过程中出现异常，那么发送端会重新发送该消息，从而造成消息重复发送

防止消息重复/保证幂等性 - 消息重复不可怕, 重要的是要保证消息处理的幂等性

- Kafka消息重复的表现: Kafka 实际上有个 offset 的概念，就是每个消息写进去，都有一个 offset，代表消息的序号，然后 consumer 消费了数据之后，每隔一段时间（定时定期），会把自己消费过的消息的 offset 提交一下(此时消息可能还没有正式开始处理)，表示“我已经消费过了，下次我要是重启啥的，你就让我继续从上次消费到的 offset 来继续消费吧”; --- 如果消费端突然崩掉, 此时还没有将已经消费的消息的offset提交, 重启后就会重复消费消息

- 怎么解决呢,需要根据具体业务来看:

  - 比如你拿个数据要写库，你先根据主键查一下，如果这数据都有了，你就别插入了，update 一下

  - 比如你是写 Redis，那没问题了，反正每次都是 set，天然幂等性。

  - 生产者发送每条数据的时候，里面加一个全局唯一的 id, 消费到了之后，先根据这个 id 去比如 Redis 里查一下，如果没查到, 证明没有消费过，处理，然后这个 id 写 Redis。如果消费过了，就别处理了

#### 如何防止消息丢失

防止消息丢失 - 消息可能在三个点丢失 -- 消费者,mq,生产者 ;  RabbitMQ 和 Kafka 的机制不同

- 对于rabbitmq:

  - 生产者将数据发送到 RabbitMQ 的时候，可能数据就在半路给搞丢了

    - 利用事务(同步模式的,吞吐量下降,不好用) - 发送数据之前,开启 RabbitMQ 事务channel.txSelect, 如果消息没有成功被 RabbitMQ 接收到，那么生产者会收到异常报错，此时就可以回滚事务channel.txRollback,如果收到了消息，那么可以提交事务channel.txCommit

    - 通过 开启 confirm 模式(异步模式的, 不会阻塞) - 如果写入了 RabbitMQ 中，RabbitMQ 会给你回传一个 ack 消息，告诉你说这个消息 ok 了。如果 RabbitMQ 没能处理这个消息，会回调你的一个 nack 接口，告诉你这个消息接收失败，你可以重试

  - rabbitmq弄丢消息 - 开启 RabbitMQ 的持久化. 
  
    第一步,创建 queue 的时候将其设置为持久化,这样就可以保证 RabbitMQ 持久化 queue 的元数据，但是它是不会持久化 queue 里的数据的。
    
    第二个是发送消息的时候将消息的 deliveryMode 设置为 2 ,就是将消息设置为持久化的，此时 RabbitMQ 就会将消息持久化到磁盘上去

  - 消费者弄丢消息 - RabbitMQ 提供的 ack 机制, 必须关闭 RabbitMQ 的自动 ack, 然后每次你自己代码里确保处理完的时候，再在程序里 ack一下, 如果消费者没有处理完就挂了,rabbitmq没有收到ack,会将这个消息发送给其他消费者处理;

- 对于Kafka:

  - 消费端丢失消息 - 正常来说, kafka是每隔一段实践就提交一次offset, 如果消费端接收到消息还没处理, 碰到kafka刚好自动提交了offset, 处理消息时消费端挂掉了,就造成了消息丢失; 这和 rabbitmq的消费端(ack机制)类似; 此时只要关闭offset, 处理完手动提交offset

  - kafka弄丢消息 
  
    - 某个broker挂了, 然后回重新选举 partition 的 leader,要是此时其他broker中的 follower 刚好还有些数据没有同步, 然后选举某个 follower 成 leader 之后,数据就丢失了

    - 如何解决呢: (保证在 leader 所在 broker 发生故障，进行 leader 切换时，数据不会丢失。)

      - 给 topic 设置 replication.factor 参数：这个值必须大于 1，要求每个 partition 必须有至少 2 个副本
      - 在 Kafka 服务端设置 min.insync.replicas 参数：这个值必须大于 1，这个是要求一个 leader 至少感知到有至少一个 follower 还跟自己保持联系，没掉队，这样才能确保 leader 挂了还有一个 follower 吧
      - 在 producer 端设置 acks=all：这个是要求每条数据，必须是写入所有 replica 之后，才能认为是写成功了。
      - 在 producer 端设置 retries=MAX（很大很大很大的一个值，无限次重试的意思）：这个是要求一旦写入失败，就无限重试，卡在这里了。

  - 生产者丢失数据 - 设置 `acks=all`, 和 `retries=MAX`后就可以保证没有数据丢失了;


#### 如何保证消息顺序

保证消息顺序性, 没有顺序会造成什么问题? 

比如两个数据库通过 binglog 同步数据, binglog 中有增加,修改,删除三个操作, 如果消费者读取的顺序变了,同步数据就错了; rabbit和kafka机制不同分开来说

- 对于 rabbitmq:

  问题: 一个queue, 多个consumer, msg1,msg2,msg3 顺序进入queue, 结果某个消费者先得到msg2, 那么顺序就变了;

  解决方法1: rabbit中将一个 queue, 替换为多个 queue, 每个consumer对应一个queue, 一组有序的消息一起被发送到一个queue, 被同一个consumer消费; 
  
  方法2: 或者只有一个queue, 和一个consumer, 在consumer内部维护内存队列做排队, 分发给不同的worker处理;

- 对于Kafka //todo

    问题:

    方法: 写 N 个内存 queue，具有相同 key 的数据都到同一个内存 queue；然后对于 N 个线程，每个线程分别消费一个内存 queue 即可，这样就能保证顺序性

#### 如何防止消息的大量积压

消息大量积压的问题: consumer出现问题, 不消费了; -- 临时紧急扩容消息队列

- 先修复consumer的问题, 这是肯定要做了, 然后将所有的consumer停止;
- 新建容量更大的消息队列, 写一个临时的分发数据的consumer, 将消息转入到大容量的队列中
- 然后将修复好的consumer部署到10倍数量的机器上, 启动, 消费临时队列中的消息(相当于将queue资源, consumer资源都临时扩大了)
- 等消费完积压的消息后, 恢复原先的queue和consumer架构;

#### 如何防止消息过期失效

消息过期失效 - 比如 rabbitmq 中可以设置消息过期时间, 积压过了这个时间, 消息回直接丢失; 怎么解决: 重新导入, 在半夜流量小的时候, 写临时程序, 在消费者中将丢失的消息查出来,重新发送到queue中(比如在订单服务中查找丢失的1000个订单, 重新导入到queue中给库存服务消费;)

#### 如何保证分布式最终一致性

分布式最终一致性问题 (正常来说mq会保证最终一致性,但是如果生产者系统处理成功直接返回成功, 某个消费者系统出现异常,最终数据一致性就无法保证 -- 需要引入分布式事务)


# 有哪些产品可供选择-选型

ActiveMQ, RabbitMQ，RocketMQ，Kafka，Redis

选择哪个好呢

- ActiveMQ - Java实现，出现的最早，性能没有后来的mq好；用的较少;

- RabbitMQ - erlang实现，性能好， 后台管理界面；社区活跃；

- rocket - 可以说是kafka的变种, java版本实现, 阿里开源; rocket和kafka有类似无限堆积的能力；支持分布式事务

- redis:  常常用于缓存，消息队列不是主要功能

  - redis 消息推送（基于分布式 pub/sub）多用于实时性较高的消息推送，并不保证可靠; 其他的mq和kafka保证可靠但有一些延迟

  - redis 发布订阅除了表示不同的 topic 外，并不支持分组

- Kafka - 真正的分布式消息队列，主要用于日志采集，实时数据计算（spark streaming，storm一起用）

  - kafka中发布一个东西，多个订阅者可以分组，同一个组里只有一个订阅者会收到该消息，这样可以用作负载均衡

  - 实时log分析, 应付大并发



# RabbitMQ

## AMQP协议

### 概念

https://blog.yoodb.com/yoodb/article/detail/1347

AMQP（高级消息队列协议）是一个网络协议。它支持符合要求的客户端应用（application）和消息中间件代理（messaging middleware broker）之间进行通信。

- Server: 中间件本体, 接收 client 连接

- virtual host: 一个 virtual host 表示一个交换器、消息队列的集合. 

  权限控制的最小粒度是Virtual Host

  每个 vhost 本质上就是一个 mini 版的 RabbitMQ 服务器，拥有自己的队列、交换器、绑定和权限机制

  连接时必须指定, RabbitMQ 默认的 vhost 是 / 

- Exchange: 接受生产者发送的消息，并根据Binding规则将消息路由给服务器中的队列。 就是一个路由表

  ExchangeType决定了Exchange路由消息的行为，例如，在RabbitMQ中，ExchangeType有direct、Fanout和Topic三种，不同类型的Exchange路由的行为是不一样的

- Binding: 用于 Exchange与Message Queue 的关联

  Exchange在与多个Message Queue发生Binding后会生成一张路由表，路由表中存储着Message Queue所需消息的限制条件即Binding Key。当Exchange收到Message时会解析其Header得到Routing Key，Exchange根据Routing Key与Exchange Type将Message路由到Message Queue。Binding Key由Consumer在Binding Exchange与Message Queue时指定，而Routing Key由Producer发送Message时指定，两者的匹配方式由Exchange Type决定

  Exchange 和Queue的绑定可以是多对多的关系


- Message Queue：消息队列，用于存储还未被消费者消费的消息

- broker 各种各样的 exchange 和 queue 合在一起成为 broker

- Message: 由Header和Body组成，

  Header是由生产者添加的各种属性的集合，包括Message是否被持久化、由哪个Message Queue接受、优先级是多少等。
  
  而Body是真正需要传输的APP数据

-Channel:信道，仅仅创建了客户端到Broker之间的连接后，客户端还是不能发送消息的。需要为每一个Connection创建Channel，AMQP协议规定只有通过Channel才能执行AMQP的命令。

  一个Connection可以包含多个Channel。因为对于操作系统来说建立和销毁 TCP 都是非常昂贵的开销，所以引入了信道的概念，以复用一条 TCP 连接。

### 工作过程

消息（message）被发布者（publisher）发送给交换机（exchange）, 

然后交换机将收到的消息根据路由规则分发给绑定的队列（queue）。

最后AMQP代理会将消息投递给订阅了此队列的消费者，或者消费者按照需求自行获取

### 消息确认

接收消息的应用也有可能在处理消息的时候失败。基于此原因，AMQP模块包含了一个消息确认（message acknowledgements）的概念：当一个消息从队列中投递给消费者后（consumer），消费者会通知一下消息代理（broker）

当“消息确认”被启用的时候，消息代理不会完全将消息从队列中删除，直到它收到来自消费者的确认回执（acknowledgement）


## 基本介绍


由 Erlang 语言开发的 AMQP 的开源实现, 可集群, 有 ui 界面, 有插件机制

保证消息可靠: RabbitMQ 使用一些机制来保证可靠性

- msg ack `消息确认(message acknowledgement)`: 有多个消费者的情况下, 一个consumer处理消息时挂了, 这个消息没有 "消息确认" 发给 rabbitmq, 队列中的消息不会删除, 会尝试发送给另外的consumer处理

- 持久化:message, queue 持久化, 即使 rabbitmq 重启, 队列, 消息也不会丢失

- 公平发布: 一个consumer 同时只能接受一个消息, 发送给 rabbitmq 一个消息回执后才会被发送下一个消息, 避免了一个consumer特别忙碌, 一个consumer特别闲的问题


其他特性:

- 虚拟主机(vhost): 支持将一个 rabbit 实例划分成多个 虚拟实例, 分别给不同的系统用, 起到隔离的效果, 一个系统对应的虚拟主机崩了, 不影响剩下的虚拟主机

- 集群: 是镜像集群, 只能提高吞吐量, 无法保证可无限拓展


安装:

https://blog.csdn.net/qq_22638399/article/details/81704372

erlang 命令行退出：https://blog.csdn.net/cnxieyang/article/details/52710967

WSL 中有bug ， 坑爹货

docker + rabbitmq：https://www.jianshu.com/p/14ffe0f3db94


## 几种消息模型

RabbitMQ提供了6种消息模型，但是第6种其实是RPC，并不是MQ; 3、4、5这三种都属于订阅模型，只不过进行路由的方式不同。

### hello-world

```
+------------------+                                                                  +---------------------+
|                  |                                                                  |                     |
|                  |              +------+----+-----+-----+-----+-----+               |                     |
|                  |              |      |    |     |     |     |     |               |                     |
|    publisher     |              | queue|    |     |     |     |     |               |       consumer      |
|                  |              |      |    |     |     |     |     |               |                     |
|                  +---------->   |      |    |     |     |     |     +-------------->+                     |
|                  |              |      |    |     |     |     |     |               |                     |
|                  |              |      |    |     |     |     |     |               |                     |
|                  |              +------+----+-----+-----+-----+-----+               |                     |
|                  |                                                                  +---------------------+
+------------------+


```

### worker-queue

让多个消费者绑定到一个队列，共同消费队列中的消息。队列中的消息一旦消
费，就会消失，因此消息不会重复消费

```
+--------------+         +------+----+-----+        +-------------+
|              |         |      |    |     |        |             |
               |         | queue|    |     |        | consumer    |
|   publisher  |         |      |    |     |        |             |
|              +----->   |      |    |     | -------+             |
|              |         |      |    |     |        +-------------+
|              |         |      |    |     |        +--------------+
+--------------+         +------+----+-+---+        |              |
                                       |            |    consumer  |
                                       +------------+              |
                                                    |              |
                                                    +--------------+


```

### 发布订阅

可以分三类 (实际上对应了三种Exchange类型)




=================广播订阅: 对应 exchange type 为 fanout

- provider 向 exchange 发送消息

- exchange 将消息发送给所有 queue (消息被广播)

  exchange 和所有 queue 绑定

- consumer 从 queue 拿到消息 

  若 两个 consumer 从同一个 queue 拿去消息, 消息不会重复

  若两个 consumer 从不同 queue 拿消息, 会重复 (也就是消息被广播了)


```
                                        +------+
                                        |      |
                                        +------+       +---------+
                                        |      |       |         |
                            +-----------+      +-------+   consumer
                            |           +------+       |         |
+-------------+             |           |      |       +---------+
|             |     +-------+-+         +------+
|             +-----+exchange |
|    publisher|     |         |        +------+
|             |     +-------+-+        |      |
+-------------+             |          +------+      +------------+
                            +----------+      +------+            |
                     type: fanout      +------+      |  consumer  |
                                       +------+      |            |
                                                     +------------+

```


=======================路由订阅: exchange type 为 Direct

- provider 向 exchange 发送消息, 必须指定消息的routing key

- exchange 接收消息, 然后把消息递交给 与routing key完全匹配的队列

  queue 指定接收哪些 routing key

  如 某个消息 msg1 (routing key = xxx), msg2 (key = yyy), msg3 (key = zzz), queue1 (指定 routing key = xxx), queue2 (key = xxx, yyy), 那么 queue1 的consumer会接收到 msg1, queue2 的consumer 会接收到 msg1, msg2. 

- 各个 consumer 从 自己的queue拿去消息



=============================topic 主题订阅: exchange type = topic. 是路由订阅的升级版, routing key 支持通配符

- #：匹配一个或多个词 (注意不是字母)
- *：匹配不多不少恰好 1 个词


### rpc



## 使用 rabbit

springboot整合

引入 `spring-boot-starter-amqp` (符合 amqp 协议的实现都可以用)

### 消息的序列化

RabbitMQ 的序列化是指 Message 的 body 属性，即我们真正需要传输的内容，

RabbitMQ 抽象出一个 MessageConvert 接口处理消息的序列化, 当调用了 convertAndSend 方法时会使用 MessageConvert 进行消息的序列化，其实现有 : 

- SimpleMessageConverter（默认）

  对于要发送的消息体 body 为 byte[] 时不进行处理，
  
  如果是 String 则转成字节数组,
  
  如果是 Java 对象，则使用 jdk 序列化将消息转成字节数组，转出来的结果较大，含class类名，类相应方法等信息。因此性能较差;此时就要考虑使用类似 Jackson2JsonMessageConverter 等序列化形式以此提高性能

- Jackson2JsonMessageConverter ;

要自定义需要如下配置:

```java
@Configuration
public class MyAMQPConfig {
    @Bean
    public MessageConverter messageConverter(){
          return new Jackson2JsonMessageConverter();
    }
}
```

或者:

```java
@Configuration
public class RabbitMQConfig {

    @Bean
    public RabbitListenerContainerFactory<?> rabbitListenerContainerFactory(ConnectionFactory connectionFactory){
        SimpleRabbitListenerContainerFactory factory = new SimpleRabbitListenerContainerFactory();
        factory.setConnectionFactory(connectionFactory);
        factory.setMessageConverter(new MessageConverter() {
            @Override
            public Message toMessage(Object object, MessageProperties messageProperties) throws MessageConversionException {
                return null;
            }

            @Override
            public Object fromMessage(Message message) throws MessageConversionException {
                try(ObjectInputStream ois = new ObjectInputStream(new ByteArrayInputStream(message.getBody()))){
                    return (User)ois.readObject();
                }catch (Exception e){
                    e.printStackTrace();
                    return null;
                }
            }
        });

        return factory;
    }

}
```

### 发送

使用默认的 exchange

```java
// 需要注册一个 queue, 否则消息只会到 exchange, 不会存入 queue
@Configuration
public class RabbitConfig {
    @Bean
    public Queue queueHello() {
        return new Queue("hello_queue");
    }

}

// 然后使用:

@Autowired
private final AmqpTemplate template;

/**
* 发送到默认 exchange, 不会发送到 queue
*
*/
public void sendMessage(String msg) {
  // 注意MessageBuilder 不是带泛型的那个
    template.send(MessageBuilder.withBody(msg.getBytes(StandardCharsets.UTF_8)).build());
}

/**
发送到 默认 exchange, 然后转发到 queue (queue name  = hello_queue)

也就是说, 默认 exchange 不用 binding , 发送时, routing key 指定为 queue name 即可被 该 queue 收到
*/
public void send1(String msg) {
  // 自动转换为 Message 后发送
    this.template.convertAndSend("hello_queue", msg);
    System.out.println("sender: 发送了 " + msg);
}

public void sendWithTopic(String msg, String routingKey) {
  // 发送给指定的 exchange
    this.template.convertAndSend("exchange", routingKey, msg);
}
```

自己指定 exchange:

```java
@Configuration
public class TopicExchangeConfig {
    @Bean
    public Queue queue1() {
        return new Queue("queue_1");
    }

    @Bean
    public Queue queue2() {
        return new Queue("queue_2");  // 需要指定 name
    }

    @Bean
    public TopicExchange exchange() {
        return new TopicExchange("exchange");  // 需要指定 name
    }

    // binding exchange and queue with routing key
    @Bean
    public Binding bindingQueue1(Queue queue1, TopicExchange exchange) {
        return BindingBuilder.bind(queue1).to(exchange).with("topic.message");
    }

    @Bean
    public Binding bindingQueue2(Queue queue2, TopicExchange exchange) {
        return BindingBuilder.bind(queue2).to(exchange).with("topic.#");
    }
}

// 使用

/**
  * 带 exchange 和 routing key 发送
  */
public void sendWithTopic(String msg, String routingKey) {
    this.template.convertAndSend("exchange", routingKey, msg);
}
```

### 接收

========================单独使用 @RabbitListener 注解来指定某方法作为消息消费的方法

```java
@Component
public class Receiver2 {
    @RabbitListener(queues = "queue_2")
    public void process(String msg) {
        System.out.println("receiver2: " + msg);
    }

    @RabbitListener(queues = {"hello_queue"})
    public void processHelloQueue(String msg) {
        System.out.println(">>> receive msg from hello_queue: " + msg);
    }
}
```

========================@RabbitListener 和 @RabbitHandler 搭配使用

(@RabbitListener 标注在类上面表示当有收到消息的时候，就交给 @RabbitHandler 的方法处理，具体使用哪个方法处理，根据 MessageConverter 转换后的参数类型)

此时需要有一个默认的处理方法 @RabbitHandler(isDefault=true)，默认是false。不设置一个true，消费mq消息的时候会出现“Listener method ‘no match’ threw exception”异常。

```java
// 为 consumer 指定 queue
// 监听
@Component
@RabbitListener(queues = "consumer_queue")
public class Receiver {

    @RabbitHandler
    public void processMessage1(String message) {
        System.out.println(message);
    }

    @RabbitHandler
    public void processMessage2(byte[] message) {
        System.out.println(new String(message));
    }
}
```

===============使用 使用 @Payload 和 @Headers 注解可以消息中的 body 与 headers 信息, 不使用的话默认方法参数就是 body

```java
@RabbitListener(queues = "debug")
public void processMessage1(@Payload String body, @Headers Map<String,Object> headers) {
    System.out.println("body："+body);
    System.out.println("Headers："+headers);
}

// 获取单个 header属性

@RabbitListener(queues = "debug")
public void processMessage1(@Payload String body, @Header String token) {
    System.out.println("body："+body);
    System.out.println("token："+token);
}
```


===========================甚至 可以在 consumer 端 通过 rabbitListener 设置 binding:

```java
@RabbitListener(bindings = @QueueBinding(
        exchange = @Exchange(value = "topic.exchange",durable = "true",type = "topic"),
        value = @Queue(value = "consumer_queue",durable = "true"),
        key = "key.#"
))
public void processMessage1(Message message) {
    System.out.println(message);
}
```

### 持久化

为什么需要持久化? 中间件服务器异常重启, 数据会丢失

持久化会造成性能损耗。可以通过`设置消息过期时间`、`降低发送消息大小`等其他方式来尽可能的降低MQ性能的降低。

分为如下三个: (均可通过 构造函数的 durable 指定, 默认都是 true)

- queue 的持久化

  通过构造函数设置

- exchange 的持久化

  通过构造函数设置

- message 的持久化 (必须在 queue 的持久化基础上才有意义)

  底层就是 AMQP.BasicProperties的属性deliveryMode设置为2

  发送消息时, 通过消息后处理设置持久化`convertAndSend(.... , new MessagePostProcessor() {message.getMessageProperties().setDeliveryMode(MessageDeliveryMode.PERSISTENT);})`





都设置持久化之后就能保证数据不会被丢失吗？当然不是，多个方面:

- 在消费者端, 如果消息的自动确认为true，那么在消息被接收以后，RabbitMQ就会删除该消息，假如消费端此时宕机，那么消息就会丢失。

  因此需要将消息设置为手动确认。即消费完成后再发送确认

- 在rabbitmq服务端，如果消息正确被发送，但是rabbitmq未来得及持久化，没有将数据写入磁盘，服务异常而导致数据丢失，

  解决方案，可以通过rabbitmq集群的方式实现消息中间件的高可用性


### 消息确认机制-手动确认-回调

确认机制有两种:

#### 消息发送确认

分为两步

- 是否到达 exchange 的确认; 

  实现 RabbitTemplate.ConfirmCallBack接口，消息发送到交换器Exchange后触发回调 (实际是发送到 broker 出发回调, 而 broker 包含 exchange 和 queue, 所以可以认为 exchange 出发回调)

  为 rabbitTemplate 配置 @PostConstruct `rabbitTemplate.setConfirmCallback(new ConfirmCallBackHandler())`

  设置 `spring.rabbitmq.publisher-confirms = true`

- 是否到达 queue 的确认 (比如根据发送消息时指定的routingKey找不到队列时会触发)

  实现 RabbitTemplate.ReturnCallback 接口

  `abbitTemplate.setReturnCallback(new ReturnCallBackHandler());`

  `spring.rabbitmq.publisher-returns = true`


若没有 ack, 生产者这边业务这样控制: 比如生产者每次发消息之前先把消息保存到本地，如果收到ack就把这个消息给删除，没有收到就隔一段时间重试，最多重试个3次，还是没收到就把这个消息登记起来后续处理，不再发送了

```java
@configuration
class config {
  @autowired
  RabbitmqTemplate tt;


  @PostConstruct
  public void initRabbitTemplate() {
    //RabbitTemplate.ConfirmCallBack
    tt.setConfirmCallback(() -> {
      // ...
    })
  }
}
```


####  消息消费确认

就是 消息是否被 consumer 消费

有三种模式: AcknowledgeMode.NONE/AUTO/MANUAL, 默认是 auto, consumer 接收到消息会自动确认, 不管消息是否完全被消费完成, broker 都会删除这条消息,任务消费完成了

`spring.rabbitmq.listener.simple.acknowledge-mode = manual` 手动确认

如果是自定义的监听容器, 配置 `SimpleRabbitListenerContainerFactory` 即可

消费者成功确认:

```java
@Component
@RabbitListener(queues = "confirm_queue_B")
public class Customer {
    @RabbitHandler
    public void process(Message message, Channel channel){
        System.out.println("ReceiverA："+new String(message.getBody()));
        // delivertag 就是一个数字, 在 channel 内每收到一条消息就自增
        // 参数 2: 是否批量确认消息; 一般为 false, 每条消息都分别签收确认
        channel.basicAck(message.getMessageProperties().getDeliveryTag(),true);
}

```

消费者失败确认:

```java
Component
@RabbitListener(queues = "confirm_queue_B")
public class Customer {
    @RabbitHandler
    public void processJsonMessage(@Payload String body, @Header(AmqpHeaders.DELIVERY_TAG) long deliveryTag, Message message,Channel channel){      
        System.out.println("ReceiverA："+new String(message.getBody()));
        // 参数 2: 是否批量拒绝
        // 参数 3: 是否重新加入 queue 等待消费
        channel.basicNack(deliveryTag,true,true);
}

// 还可以一次只拒绝一条消息
@Component
@RabbitListener(queues = "confirm_queue_B")
public class Customer {
     public void processJsonMessage(@Payload String body, @Header(AmqpHeaders.DELIVERY_TAG) long deliveryTag, Message message,Channel channel){  
        System.out.println("ReceiverA："+new String(message.getBody()));
        channel.basicReject(deliveryTag,true);
}

// channel.basicNack 与 channel.basicReject 的区别在于basicNack可以批量拒绝多条消息，而basicReject一次只能拒绝一条消息
```

消息手动拒绝中如果requeue为true会重新放入队列，但是如果消费者在处理过程中一直抛出异常，会导致入队-》拒绝-》入队的循环，该怎么处理呢？======== 先成功确认，然后通过**channel.basicPublish()**重新发布这个消息。重新发布的消息网上说会放到队列后面，进而不会影响已经进入队列的消息处理。

```java
@Component
@RabbitListener(queues = "confirm_queue_B")
public class AckTempalte {
    enum Action{
        ACCEPT, // 处理成功
        RETRY, // 可以重试的错误
        REJECT, // 无需重试的错误
    }
    @RabbitHandler
    public void processJsonUser(Message message, Channel channel){
        Action action=Action.ACCEPT;
        long tag=message.getMessageProperties().getDeliveryTag();
        try{
            System.out.println( message.getMessageProperties().getConsumerTag());
            String message1 = new String(message.getBody(), "UTF-8");
            System.out.println("获取消息'" + message1 + "'");

        }catch (Exception e){
            // 根据异常种类决定是ACCEPT、RETRY还是 REJECT
            action = Action.RETRY;
            e.printStackTrace();

        }finally {
            try {
                // 通过finally块来保证Ack/Nack会且只会执行一次
                if (action == Action.ACCEPT) {
                    channel.basicAck(tag, true);
                    
                }
                // 重试
                else if (action == Action.RETRY) {
                    channel.basicNack(tag, false, true);// 最后一个 true 表示 重新入 queue
                    Thread.sleep(2000L);
                } 
                // 拒绝消息也相当于主动删除mq队列的消息
                else {
                    channel.basicNack(tag, false, false);
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}

```

### 使用 amqpAdmin 集中配置 rabbitmq

# kafka

## kafka 简介

分布式消息中间件

高吞吐量

producer, consumer, topic 

推荐现在大家用Pulsar，Kafka的超级赛亚人形态，更加简洁更好用


适用于对消费次序或时间没有强一致性需要的场景




# 即时通讯 MQTT协议

https://www.jianshu.com/p/e132261456b5
TODO