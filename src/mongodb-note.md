---
title: mongodb-note
tags:
  - mongodb
  - nosql
  - db
categories: java web
date: 2020-08-31 22:19:28
---


<div align="center">
https://university.mongodb.com/ 官方课程

https://mongoing.com/ 中文社区
https://docs.mongoing.com/ 文档

https://www.mongodb.com/cloud 免费版云服务

https://zhuanlan.zhihu.com/p/24377636
https://zhuanlan.zhihu.com/p/26483750
https://www.zhihu.com/question/20059632/answer/14981332
https://www.zhihu.com/question/19882468/answer/18329680
</div>

<!--more-->

<!-- TOC -->

- [what is mongodb](#what-is-mongodb)
  - [特性](#特性)
  - [和 MySQL 概念对比](#和-mysql-概念对比)
  - [和 es 对比](#和-es-对比)
  - [rdbms 和 nosql](#rdbms-和-nosql)
- [why mongodb](#why-mongodb)
  - [特点](#特点)
  - [使用场景](#使用场景)
- [how](#how)
  - [安装](#安装)
    - [docker](#docker)
    - [docker 安装](#docker-安装)
    - [压缩包安装](#压缩包安装)
  - [使用](#使用)
    - [和 spring boot 集成](#和-spring-boot-集成)
    - [使用 命令行](#使用-命令行)
- [索引](#索引)
  - [索引介绍](#索引介绍)
  - [在命令行中操作索引](#在命令行中操作索引)
  - [索引评估优化](#索引评估优化)
- [分片集群 Shard Cluster](#分片集群-shard-cluster)
  - [架构图](#架构图)
  - [分片机制](#分片机制)
  - [副本集](#副本集)
  - [mongo 中的 cap 支持](#mongo-中的-cap-支持)

<!-- /TOC -->

# what is mongodb

## 特性

基于分布式文件存储的数据库。由C++语言编写

nosql (非关系数据库)

文档型 - mongo 本质是 key-value 存储, key需要唯一, value会以json的形式存储(json 的 field 可以不同), 而关系型通过一条条记录(具有相同 column)来存储数据

高性能计算，提供基于内存的快速数据查询

容易扩展，利用数据分片可以实现分布式存储, 副本集可以实现高可用集群

丰富的功能集，支持二级索引、强大的聚合管道功能，为开发者量身定做的功能，如数据自动老化、固定集合

存储方式为虚拟内存+持久化存储，Mongo将数据写入内存中，再由虚拟内存管理器将其持久化到硬盘中，因此写操作会比关系型数据库快很多


## 和 MySQL 概念对比

- 数据库 (databases): 和 MySQL类似, 一台mongodb服务器可以包含多个数据库

- 集合 (collections): 类似MySQL中的表, 可以包含多个文档, 这些文档有相关性, 结构可以不同

    不同之处就在于集合的结构(schema)是动态的，不需要预先声明一个严格的表结构。
    
    更重要的是，默认情况下 MongoDB 并不会对写入的数据做任何schema的校验。

- 文档 (documents):  就是一个 json, 多个 field 组成, 是 mongo 独有的概念,类似 MySQL的 row

- 字段 (field ): json 中的某个 key, 类似 MySQL的 column

    支持嵌套的文档、数组

    MongoDB中字段的类型是固定的、区分大小写、并且文档中的字段也是有序的

- _id 主键:  默认使用一个_id 字段来保证文档的唯一性

    若 insert 时不指定, 会自动生成 id, 由客户端(Driver)生成

- reference 引用: 类似 MySQL的外键

    但是没有任何强制约束, 只是用于查询使用

- view 视图: 视图是基于表/集合之上进行动态查询的一层对象，可以是虚拟的，也可以是物理的(物化视图), 类似 MySQL

- index 索引: 与SQL 的索引相同

- `$lookup`: 聚合操作符，可以用于实现类似 SQL-join 连接的功能

- transaction 事务，从 MongoDB 4.0 版本开始

- aggregation 聚合: 类似 MySQL 的分组操作

    MongoDB 提供了强大的聚合计算框架，group by 是其中的一类聚合操作

- mapreduce: 

- Projection (投影): 类似传统的SQL语法中，可以限定返回的字段


## 和 es 对比

https://www.zhihu.com/question/274485040/answer/1242910052

## rdbms 和 nosql

关系型 vs. 非关系型

- rdbms 数据都是结构化的 (有严格的行/列约束), nosql 没有预定义的约束

- rdbms 支持 sql

    mongodb 在 client 使用sql: mongobooster、studio3t

- rdbms 支持事务 , nosql 不支持

    nosql 放弃事务，追求高性能、高可扩展

    最初 mongo 仅仅支持 单文档的事务, 4.0后, mongodb 支持多文档的事务 (事务中的操作可以跨副本集, 跨分片)

    在事务的隔离性上，MongoDB 支持快照(snapshot)的隔离级别，可以避免脏读、不可重复读和幻读。

- rdbms 有严格的一致性, nosql 仅仅最终一致性



# why mongodb

## 特点

可以和关系型数据库(MySQL)对比着看:

优点:

- 没有schema约束, 可以迅速开发, 不必定义字段名, 字段类型等等

- 速度更快, mongo存储方式是 先写内存, 然后刷入磁盘

    用于缓存, 不过一般不这么干, redis 就够了

- MongoDB自带复制、分片等高可用方案

- 支持二维空间/地理位置索引, 可以存储经纬度

- GridFS, 将文件存储在MongoDB集合中

- 文档可以嵌套, 解决了 MySQL 中多表 join损失性能的问题


缺点: 

- 没有数据完整性检查, 现在已经有了 (Mongoose 可以解决一切约束缺失问题)

    MongoDB 支持模式验证

- 没有强制的外键约束

- 事务支持 (MongoDB 目前只支持单文档事务，需要复杂事务支持的场景暂时不适合)

- 开源协议问题 (AGPLv3切换到SSPL)

## 使用场景

1. 适合 日志系统, 爬虫爬取的原始数据

1. 适合数据规模巨大的系统 (自带分片, 复制功能, 分布式, 高可用实现方便)

1. o2o app, 用于地理位置存储 (查询附近的人, 距离、或者订餐系统、配送系统等)

1. 作为文件存储系统

1. 解决 MySQL 多表 join 的性能问题

# how

## 安装

### docker

### docker 安装

https://www.jianshu.com/p/576a156f6877
https://www.cnblogs.com/yunquan/p/11174265.html

```sh
# https://hub.docker.com/_/mongo
# 由于添加了 env params, 往 admin 数据库 的 system.users 集合中 插入了记录, 开启了 auth
docker run -d -p 27017:27017 --name mongodb -e MONGO_INITDB_ROOT_USERNAME=mongoadmin -e MONGO_INITDB_ROOT_PASSWORD=mongoadmin mongo

# check -> http://localhost:8081/
docker run --restart=always --name mongo-express --link mongodb:mongodb -d -p 8081:8081 -e ME_CONFIG_OPTIONS_EDITORTHEME="3024-night" -e ME_CONFIG_BASICAUTH_USERNAME="mongoexpress" -e ME_CONFIG_BASICAUTH_PASSWORD="mongoexpress" -e ME_CONFIG_MONGODB_ADMINUSERNAME="mongoadmin" -e ME_CONFIG_MONGODB_ADMINPASSWORD="mongoadmin" -e ME_CONFIG_MONGODB_SERVER="mongodb" mongo-express

```

### 压缩包安装

下载 zip 包, 新建文件夹 `data` 用来存储数据; 新建 `daemon_start.sh` 作为server启动脚本:

```sh
cd D:\\program_green\\mongodb\\mongodb-win32-x86_64-2008plus-ssl-4.0.11\\bin
./mongod.exe --dbpath D:\\program_green\\mongodb\\data
```

新建 `client_start.sh` 为client脚本:

```sh
cd D:\\program_green\\mongodb\\mongodb-win32-x86_64-2008plus-ssl-4.0.11\\bin
./mongo localhost:27017
```

mongoDB默认占用端口为27017

默认mongo启动是以test用户, 即 `mongo localhost:27017/test`, 可以以别的用户启动 `mongo localhost:27017/admin`


数据建模时的区别可以参考: https://www.yiibai.com/mongodb/mongodb_data_modeling.html


## 使用

### 和 spring boot 集成

spring boot data mongodb

```sh
use demo
#需要创建非 admin 的 user, 否则 spring boot 会报认证异常
db.createUser({"user": "tiger", "pwd": "tiger", "roles": [{"role": "readWrite", "db": "demo"}]})
```

spring boot 配置: ...uri: `mongodb://tiger:tiger@172.24.208.103:27017/demo`



### 使用 命令行

```sh
# 进入
# 远程连接使用 mongo username:password@ip:port/dbname
# 不带 username 也可进入, 但是不是同一个系统
mongo --username mongoadmin --password
    使用 -u -p 亦可

# 当前db
db
# 查看所有
show dbs  

# 使用/创建数据库, 默认是使用 test 数据库
use DATABASE_NAME
# 数据库名不存在自动创建新数据库


# 添加 user
db.createUser(
     {
       user: "wenqiang",
       pwd: "1438438",
       roles: ["readWrite"]
     }
)
#or
db.createUser(
     {
       user: "wenqiang",
       pwd: "1438438",
       roles: [{
           "role":"readWrite",
           "db": "demo-db"
       }]
     }
)
# 创建管理员，赋予root权限
db.createUser({user:"admin",pwd:"xiiasdfiik34",roles:[{role: 'root', db: 'admin'}]})
# 删除 user
db.dropUser('username')

# 登录认证新user
db.auth("mutianwei", "123568")

# 显示表(集合)
show collections

# 查询当前 db 的集合
db.getCollectionNames()

# 创建 collections
db.createCollection("logs")

# create document
db.logs.insert( { name: "wangwenqiang", age: 3} )

# 查询 所有 document
db.logs.find([json参数])

db.集合名称.find().limit(条数)

# 统计条数
db.集合名称.count([json 参数])

# 更新 document
# age > 25 的 user 的 status 设置为 c
db.users.update(
    { age: { $gt: 25 } },
    { $set: { status: 'C' } },
    { multi: true }
)
db.book.update(
   {"_id" : ObjectId("5c61301c15338f68639e6802")},
   {"$inc": {"viewCount": 3} }
)

# 删除 document
db.book.remove({"_id":
     ObjectId("5c612b2f15338f68639e67d5")})
# 删除集合中全部数据
db.集合名称.remove({})



# projection (只查询显示指定字段, 1 表示显示, 0 表示不显示, 默认 0)
db.book.find({"author": "James"}, 
    {"_id": 1, "title": 1, "author": 1})

# 分页
db.book.find({})
    .sort({"viewCount" : -1})
    .skip(10).limit(5)


```

# 索引

## 索引介绍

使用 wiredTiger 作为默认的引擎。

使用了 B树的结构 (mysql  用的是 b+ 树) - https://zhuanlan.zhihu.com/p/107228878 //todo

============= 索引分类

除了普通索引之外，MongoDB 支持的类型还包括：

- 哈希(HASH)索引，哈希是另一种快速检索的数据结构，MongoDB 的 HASH 类型分片键会使用哈希索引。
- 地理空间索引，用于支持快速的地理空间查询，如寻找附近1公里的商家。

    其球面（Spherical）和平面（Flat）两种模式，

- 文本索引，用于支持快速的全文检索
- 模糊索引(Wildcard Index)，一种基于匹配规则的灵活式索引，在4.2版本开始引入。

唯一索引、

二级索引、

TTL索引(通过创建TTL索引，实现自动删除过期记录的功能, 比如保存日志)

MongoDB集合在创建时默认就基于_id字段创建了唯一索引，数据插入时会检查_id字段的唯一性

在内存足够的情况下，索引会被加载到Cache中，如果执行的查询是索引覆盖的（Query Optimization），其性能甚至可以媲美Redis等内存数据库等

## 在命令行中操作索引

```sh
# 为集合声明一个普通的索引：
# 数字 1 代表升序，如果是降序则是 -1
db.book.ensureIndex({author: 1})

# 复合式(compound)的索引 (对 type, published 添加联合索引)，如下：
# 只有对于复合式索引时，索引键的顺序才变得有意义
db.book.ensureIndex({type: 1, published: 1})

# 如果索引的字段是数组类型，该索引就自动成为数组(multikey)索引：
# MongoDB 可以在复合索引上包含数组的字段，但最多只能包含一个
db.book.ensureIndex({tags: 1})

```

可以通过一些参数化选项来为索引赋予一定的特性，包括：

- unique=true，表示一个唯一性索引

- expireAfterSeconds=3600，表示这是一个TTL索引，并且数据将在1小时后老化

- sparse=true，表示稀疏的索引，仅索引非空(non-null)字段的文档

- partialFilterExpression: { rating: { $gt: 5 }，条件式索引，即满足计算条件的文档才进行索引


## 索引评估优化

https://docs.mongodb.com/manual/reference/explain-results/index.html

使用 explain() 命令可以用于查询计划分析

hint() 强制查询走某个索引

```sh
db.test.explain().find( { a : 5 } )

{
  "queryPlanner" : {
    ...
    # 通过 winningPlan 可以知道是否高效: 
    # 未能命中索引的结果，会显示COLLSCAN
    # 命中索引的结果，使用IXSCAN
    #出现了内存排序，显示为 SORT
    "winningPlan" : { 
      "stage" : "FETCH",
      "inputStage" : {
        "stage" : "IXSCAN",
        "keyPattern" : {
            "a" : 5
        },
        "indexName" : "a_1",
        "isMultiKey" : false,
        "direction" : "forward",
        "indexBounds" : {"a" : ["[5.0, 5.0]"]}
        }
    }},
   ...
}
```

# 分片集群 Shard Cluster

## 架构图

```
                        +-------------------------------+
                        |        application            |
                        +--------+------+-------+-------+
                                 |      |       |
                                 v      v       v
+-----------------------------------------------------------------------------------------------------+
|            +------------+   +------------+   +------------+                                         |
|            |    mongos  |   |   mongos   |   |   mongos   |             +-------------------------+ |
|            +------------+   +------------+   +------------+        <--+ |                         | |
|                                                                         | +---------------------+ | |
|                        +       +      +       +      +                  | |   config primary    | | |
|                        |       |      |       |      |                  | |                     | | |
|                        v       v      v       v      v                  | |                     | | |
|                                                                         | +---------------------+ | |
|          +--------------------------+  +-----------------------------+  |                         | |
|          |                          |  |                             |  |  +--------------------+ | |
|          |                          |  |                             |  |  |                    | | |
|          |   +------------------+   |  |      +-----------------+    |  |  |  config secondary  | | |
|          |   |    primary       |   |  |      |  primary        |    |  |  |                    | | |
|          |   |                  |   |  |      |                 |    |  |  +--------------------+ | |
|          |   +------------------+   |  |      +-----------------+    |  |                         | |
|          |                          |  |                             |  |  +--------------------+ | |
|          |  +-----+      +-----+    |  |    +-----+     +-----+      |  |  |                    | | |
|          |  |     |      |     |    |  |    |     |     |     |      +->+  |  config secondary  | | |
|          |  | secondary  |     |    |  |    secondary   |     |      |  |  |                    | | |
|          |  |     |      |     |    |  |    |     |     |     |      |  |  +--------------------+ | |
|          |  |     |      secondary  |  |    |     |     |secondary   |  |                         | |
|          |  |     |      |     |    |  |    |     |     |     |      |  |                         | |
|          |  |     |      |     |    |  |    |     |     |     |      |  |                         | |
|          |  |     |      |     |    |  |    |     |     |     |      |  |                         | |
|          |  +-----+      +-----+    |  |    +-----+     +-----+      |  |                         | |
|          |                          |  |                             |  |                         | |
|          |                          |  |                             |  +-------------------------+ |
|          +--------------------------+  +-----------------------------+        config server replSet |
|               shard0                             shard1                                             |
|                                                                                                     |
|                                                                                                     |
|                                                                                                     |
+-----------------------------------------------------------------------------------------------------+

```

- 数据分片 shards 

    存储真正的集群数据, 可以是一个单独的 Mongod实例，也可以是一个副本集

    生产环境下, Shard一般是一个 Replica Set，以防止该数据片的单点故障

    对于分片集合(sharded collection)来说，每个分片上都存储了集合的一部分数据(按照分片键切分)，如果集合没有分片，那么该集合的数据都存储在数据库的 Primary Shard中

- 配置服务器 config server 

    保存集群的元数据（metadata），包含各个Shard的路由规则，配置服务器由一个副本集(ReplicaSet)组成

- 查询路由 mongos

    Mongos是 Sharded Cluster 的访问入口，其本身并不持久化数据 。
    
    Mongos启动后，会从 Config Server 加载元数据，开始提供服务，并将用户的请求正确路由到对应的Shard。
    
    Sharding 集群可以部署多个 Mongos 以分担客户端请求的压力

## 分片机制

基于分片切分后的数据块称为 chunk，一个分片后的集合会包含多个 chunk，每个 chunk 位于哪个分片(Shard) 则记录在 Config Server(配置服务器)上。

Mongos 在操作分片集合时，会自动根据分片键找到对应的 chunk，并向该 chunk 所在的分片发起操作请求。




===============数据如何切分:




- 范围分片: 按照分片字段从小到大, 分成多个范围段, 每段就是一个 shard

    如: 集合根据x字段 (整型) 来分片，x的取值范围为[minKey, maxKey], 将整个取值范围划分为多个chunk，每个chunk（默认配置为64MB）包含其中一小段的数据

    能很好的满足范围查询的需求, 如 想查询x的值在[-30, 10]之间的所有文档，这时 Mongos 直接能将请求路由到 Chunk2

    范围分片的缺点在于，如果 ShardKey 有明显递增（或者递减）趋势，则新插入的文档多会分布到同一个chunk，无法扩展写的能力，比如使用_id作为 ShardKey，而MongoDB自动生成的id高位是时间戳，是持续递增的。

- 哈希分片: 根据用户的 ShardKey 先计算出hash值（64bit整型），再根据hash值按照范围分片的策略将文档分布到不同的 chunk。

    由于 hash值的计算是随机的，因此 Hash 分片具有很好的离散性，可以将数据随机分发到不同的 chunk 上

    但不能高效的服务范围查询，所有的范围查询要查询多个 chunk 才能找出满足条件的文档。





================如何保证每个 shard 上,  chunk 都是均衡的呢


- 全预分配，chunk 的数量和 shard 都是预先定义好的，比如 10个shard，存储1000个chunk，那么每个shard 分别拥有100个chunk。

- 非预分配

    这种情况则比较复杂，一般当一个 chunk 太大时会产生分裂(split)，不断分裂的结果会导致不均衡；
    
    或者动态扩容增加分片时，也会出现不均衡的状态。 这种不均衡的状态由集群均衡器进行检测，一旦发现了不均衡则执行 chunk数据的搬迁达到均衡。

    数据均衡器运行于 Primary Config Server(配置服务器的主节点)上

    MongoDB 的数据迁移对集群性能存在一定影响，这点无法避免，目前的规避手段只能是将均衡窗口对齐到业务闲时段。




============如何判断 chunk 是否均衡




对于数据的不均衡是根据两个分片上的 Chunk 个数差异来判定的, 有规定的阈值表,(规定了对于多大规模的 shard, chunk 个数差异不得大于多少)


## 副本集

副本集可以作为 Shard Cluster 中的一个Shard(片)之外，对于规模较小的业务来说，也可以使用一个单副本集的方式进行部署, 之后随业务量变大缓慢过度到 shard cluster (https://docs.mongodb.com/manual/tutorial/convert-replica-set-to-replicated-shard-cluster/)

MongoDB 的副本集采取了一主多从的结构，即一个Primary Node + N* Secondary Node的方式，数据从主节点写入，并复制到多个备节点。


利用副本集，我们可以实现：：

- 数据库高可用，

    主节点宕机后，由备节点自动选举成为新的主节点；

- 读写分离，

    读请求可以分流到备节点，减轻主节点的单点压力。

    读写分离只能增加集群"读"的能力，对于写负载非常高的情况却无能为力。对此需求，使用分片集群并增加分片，或者提升数据库节点的磁盘IO、CPU能力可以取得一定效果。




=====================选举


通过 Raft 算法来完成主节点的选举，这个环节在初始化的时候会自动完成，如下面的命令：

```sh
config = {
    _id : "my_replica_set",
    members : [
        {_id : 0, host : "rs1.example.net:27017"},
        {_id : 1, host : "rs2.example.net:27017"},
        {_id : 2, host : "rs3.example.net:27017"},
  ]
}
# initiate 命令用于实现副本集的初始化：
rs.initiate(config)

# 在选举完成后，通过 isMaster()命令就可以看到选举的结果
db.isMaster()

{
    "hosts" : [
    "192.168.100.1:27030",
    "192.168.100.2:27030",
    "192.168.100.3:27030"
    ],
    "setName" : "myReplSet",
    "setVersion" : 1,
    "ismaster" : true,
    "secondary" : false,
    "primary" : "192.168.100.1:27030",
    "me" : "192.168.100.1:27030",
    "electionId" : ObjectId("7fffffff0000000000000001"),
    "ok" : 1
}
```

主节点的选举需要满足"大多数"原则, 为了避免出现平票的情况，副本集的部署一般采用是基数个节点，比如3个




=================心跳 failover


副本集中的每个节点上都会定时向其他节点发送心跳，以此来感知其他节点的变化，比如是否失效、或者角色发生了变化。

默认情况下，节点会每2秒向其他节点发出心跳，这其中包括了主节点。 如果备节点在10秒内没有收到主节点的响应就会主动发起选举

利用心跳，MongoDB 副本集实现了自动故障转移的功能 failover (即 primary node 挂了, 自动选举某个 slave node 作为新的 primary node )




=================主备节点的数据复制 oplog


主节点和备节点的数据是通过日志(oplog)复制来实现的，这很类似于 mysql 的 binlog

在每一个副本集的节点中，都会存在一个名为local.oplog.rs的特殊集合。 当 Primary 上的写操作完成后，会向该集合中写入一条oplog; 而 Secondary 则持续从 Primary 拉取新的 oplog 并在本地进行回放以达到同步的目的

- oplog 必须保证有序，通过 optime 来保证。
- oplog 必须包含能够进行数据回放的完整信息。
- oplog 必须是幂等的，即多次回放同一条日志产生的结果相同。
- oplog 集合是固定大小的，为了避免对空间占用太大，旧的 oplog 记录会被滚动式的清理

```sh
{
"ts" : Timestamp(1446011584, 2),
"h" : NumberLong("1687359108795812092"),
"v" : 2,
"op" : "i",
"ns" : "test.nosql",
"o" : { "_id" : ObjectId("563062c0b085733f34ab4129"), "name" : "mongodb", "score" : "100" }
}
# ts 操作的 optime，该字段不仅仅包含了操作的时间戳(timestamp)，还包含一个自增的计数器值。
# h 操作的全局唯一表示
# v oplog 的版本信息
# op 操作类型，比如 i=insert,u=update..
# ns 操作集合，形式为 database.collection
# o 指具体的操作内容，对于一个 insert 操作，则包含了整个文档的内容
```


## mongo 中的 cap 支持

参考 https://docs.mongodb.com/manual/applications/replication/

mongo 的 shard cluster 需要 在 一致性 和 可用性 两者权衡, 所以允许客户端为其操作设定一定的级别或者偏好，包括这些配置项：

- read preference

    读取偏好，可指定读主节点、读备节点，或者是优先读主、优先读备、取最近的节点

    设置为 primary，此时读写都在主节点上。 这保证了数据的一致性，但一旦主节点宕机会导致失败(可用性降低)

    设置为 secondaryPrefered，此时写主，优先读备，可用性提高了，但数据存在延迟(出现不一致)

- write concern

    写关注，指定写入结果达到什么状态时才返回，可以为无应答(none)、应答(ack)，或者是大多数节点完成了数据复制等等

    将读写关注都设置为 majority(意思是 大多数)，一致性提升了，但可用性也同时降低了(节点失效会导致大多数写失败)

- read concern

    读关注，指定读取的数据版本处于怎样的状态，可以为读本地、读大多数节点写入，或者是线性读(linearizable)等等。