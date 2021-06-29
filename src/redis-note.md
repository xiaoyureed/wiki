---
title: Redis
tags:
  - redis
  - nosql
date: 2016-04-28 15:30:56
categories: java web
---


<div align="center">
redis 使用笔记; {% post_link 缓存概述 📚 缓存概述 %};
reference: [redis基础](https://juejin.im/post/5ad6e4066fb9a028d82c4b66), [redis集群原理及搭建与使用](https://juejin.im/post/5ad54d76f265da23970759d3)
《redis开发和运维》
</div>

<!--more-->

<!-- TOC -->

- [redis介绍](#redis介绍)
  - [redis 特性](#redis-特性)
  - [install](#install)
  - [使用场景](#使用场景)
- [redis命令](#redis命令)
  - [系统管理命令](#系统管理命令)
  - [数据操作命令](#数据操作命令)
  - [键管理](#键管理)
- [数据类型](#数据类型)
  - [streaming 类型](#streaming-类型)
  - [String-字符串](#string-字符串)
  - [List-列表](#list-列表)
  - [Set-集合](#set-集合)
  - [Hash-哈希](#hash-哈希)
  - [ZSet-有序集合](#zset-有序集合)
- [lua脚本](#lua脚本)
- [单线程架构](#单线程架构)
- [内存淘汰机制](#内存淘汰机制)
  - [三道保险保证 key 过期处理](#三道保险保证-key-过期处理)
  - [有哪些淘汰机制](#有哪些淘汰机制)
  - [内存回收算法](#内存回收算法)
  - [如何配置淘汰策略](#如何配置淘汰策略)
    - [通过配置文件](#通过配置文件)
    - [通过命令运行时修改](#通过命令运行时修改)
- [持久化](#持久化)
  - [备份选型](#备份选型)
  - [RDB-快照持久化](#rdb-快照持久化)
    - [RDB持久化概念](#rdb持久化概念)
    - [触发机制-如何开启](#触发机制-如何开启)
    - [RDB文件](#rdb文件)
  - [AOF-日志持久化](#aof-日志持久化)
    - [aof概念](#aof概念)
    - [写入原理](#写入原理)
    - [文件同步](#文件同步)
    - [重写机制](#重写机制)
    - [aof 损坏](#aof-损坏)
- [发布与订阅](#发布与订阅)
- [事务](#事务)
- [事件](#事件)
  - [文件事件](#文件事件)
  - [时间事件](#时间事件)
  - [事件的调度&执行](#事件的调度执行)
- [redis集群](#redis集群)
  - [连接过程](#连接过程)
  - [主从链](#主从链)
- [哨兵 Sentinel](#哨兵-sentinel)
- [分片](#分片)
- [一个简单的论坛系统](#一个简单的论坛系统)
  - [文章信息](#文章信息)
  - [点赞功能](#点赞功能)
  - [对文章进行排序](#对文章进行排序)
- [一致性哈希](#一致性哈希)
- [整合 spring boot2](#整合-spring-boot2)
  - [jedis](#jedis)
  - [lettuce](#lettuce)
  - [定义序列化器](#定义序列化器)
- [面试问题](#面试问题)
- [分布式锁](#分布式锁)
  - [实现原理](#实现原理)
    - [分布式锁需要满足的要求](#分布式锁需要满足的要求)
    - [加锁机制](#加锁机制)
    - [watch dog 自动延时机制](#watch-dog-自动延时机制)
    - [redisson 如何实现可重入锁](#redisson-如何实现可重入锁)
  - [redis 实现 分布式锁](#redis-实现-分布式锁)
  - [redisson 实现](#redisson-实现)

<!-- /TOC -->

# redis介绍

## redis 特性

全称: remote dictionary server

- 速度快

    - 因为是在内存中运行
    - c 实现
    - 单线程架构, 没有资源竞争. - 存储的键很多的redis, 不要执行 `keys*` 操作

- 基本数据结构是键值对, 但是 值可以支持多种数据类型

    - string, 在 string 基础上演变出 bitmaps(位图), HyperLogLog
    - hash
    - 集合
    - 有序集合
    - 列表

- 提供多种功能

    - 键可以设置过期时间 - 可以用来实现缓存
    - 发布订阅功能 - 可以实现消息队列
    - 支持 lua 脚本 - 可以利用 lua 创建新的 redis 命令
    - 简单的事务支持
    - 提供了 pipeline (流水线) 功能 - 这样客户端能够将一批命令一次性传到 redis, 减少了网络开销

- 持久化支持

    - RDB

    - AOF

- 主从复制 - 是分布式 redis 的基础

- 高可用and分布式

## install

Windows

cmd启动server端: `redis-server.exe redis.windows.conf `, redis.windows.conf不是必须, 如果省略则使用默认配置

cmd启动client端: `redis-cli.exe -h 127.0.0.1 -p 6379 `, 然后即可在这个端口进行后续操作.




ubuntu下卸载

rm -rf var/lib/redis/

rm -rf /var/log/redis

rm -rf /etc/redis/

rm -rf /usr/bin/redis-*

```sh
apt-get remove redis # 删除
apt-get remove --purge redis # 同时删除配置文件
find / -name redis # 找到残余文件 删除之


```

Ubuntu下安装

```sh
$sudo apt-get update
$sudo apt-get install redis-server

$ redis-server # 启动server

$ redis-cli # 启动client(redis 127.0.0.1:6379)

# 检测redis server是否启动
127.0.0.1:6379> ping
PONG
```

通过源码安装

```sh
wget http://download.redis.io/releases/redis-5.0.2.tar.gz # 下载 redis 
tar -zxv -f xxx.tar.gz # 解压
ln -s redis-x.x.x <link_name> # 建立快捷软链接 # 最佳实践， 利于未来版本升级
cd redis
make # 编译
make install # 安装 # 安装到了 /usr/local/bin, 这样就可以在任何目录下使用redis 命令了

```

## 使用场景

- 计数器

  - 可以对 String 进行自增自减运算，从而实现计数器功能

  - 例如对于网站访问量，如果使用 MySQL 数据库进行存储，那么每访问一次网站就要对磁盘进行读写操作。而对 Redis 这种内存型数据库的读写性能非常高，很适合存储这种频繁读写的计数量。

- 缓存

  将热点数据放到内存中，设置内存的最大使用量以及淘汰策略来保证缓存的命中率。

- 分布式 session

  在分布式场景下具有多个应用服务器，可以使用 Redis 来统一存储这些应用服务器的会话信息，使得某个应用服务器宕机时不会丢失会话信息，从而保证高可用。

- 分布式锁实现:  SETNX 命令, 还可以使用官方提供的 RedLock 分布式锁实现

  在分布式场景下，无法使用单机环境下的锁实现。当多个节点上的进程都需要获取同一个锁时，就需要使用分布式锁来进行同步。

  setnx key value，当key不存在时，将 key 的值设为 value ，返回1。若给定的 key 已经存在，则setnx不做任何动作，返回0。当setnx返回1时，表示获取锁，做完操作以后del key，表示释放锁，如果setnx返回0表示获取锁失败

- 分布式id生成

    因为 redis 单线程的排他性, 保证的生成id 的唯一, 每次先+1再获取

    改进: 每次先+1000再获取, 拿到本地后, 0~1000 慢慢用, 用完了再获取 1001~2000, 一个用户服务宕机了，也顶多一小段userId没有用到

- 消息队列

  List 是一个双向链表，可以通过 lpop 和 lpush 写入和读取消息。

  不过不规建使用, 最好使用 Kafka、RabbitMQ 等消息中间件。专业的人做专业的事

- feed 流

    使用的是 list 类型(有序性)

- 点赞/like功能

    利用 zet 的去重

- 共同好友/共同关注

    Set 可以实现交集、并集等操作

- 排行榜

    ZSet 可以实现更具score 排名, 而且天然去重


# redis命令

## 系统管理命令

位于`/usr/local/bin`

```sh
#         启动

redis-server # 默认方式启动
redis-server --port 6379 [...] # 指定配置项启动
redis-server <path_to_redis.conf> # 指定配置文件启动， 一般存到 /opt/redis/redis.conf

#         连接 停止

redis-cli -h <host> -p <port> # 默认 127.0.0.1：6379
redis-cli shutdown [nosave|save] # 关闭， 是否持久化
```

配置文件类似如下：

```conf
bind 127.0.0.1
port 6379
logfile xxx # 日志文件
dir xxx # redis 工作目录， 存放持久化文件
daemonize xxx # 是否以守护进程方式启动

```

## 数据操作命令

命令查询: http://redisdoc.com

记忆方式: https://my.oschina.net/u/3025423/blog/793649

```sh
#        全局

keys * # 所有键， 耗时
dbsize # 键总数， 不耗时
exist <key> # 是否存在
del key [key ...] # 删除
expire <key> <seconds> # 键过期删除
ttl <key> # 剩余过期时间， 0， -1， -2
type <key> # 值的类型

```

## 键管理

```sh
# 键重命名
# rename key newKey 键重命名, 如果 newkey 已经存在, 会被覆盖
# 为了防止被覆盖, renamenx 命令 : 表示如果 newKey 不存在才可rename 成功

# 随机挑选一个 键 randomkey

# 键的过期时间
# 对于散列表这种容器，只能为整个键设置过期时间（整个散列表），而不能为键里面的单个元素设置过期时间。
# 通过 set 在初始化时设置过期时间 or 通过 setex
set key value [ex seconds] [px milliseconds] [nx|xx]
# expire key seconds 秒级别过期; key 不存在, 返回0; 设置过期为负值, 等同于 del (立即删除)
# expireat key timestamp 秒级别时间戳后过期
# pexpire key milliseconds 毫秒级别过期
# pexpireat key milliseconds-timestamp 毫秒时间戳 ----- 最终底层都是调用这个命令
# ttl key 观察剩余时间; pttl 更精确(毫秒级别)
# persist key 取消过期时间

# 键的迁移 
# 将部分数据从一个redis迁移到另一个 redis
# 方法有三: move; dump+restore; migrate
# `move key db` 
# `dump key`在源redis上将数据序列化成 RDB格式 + `restore key ttl value`在目标redis上将数据复原, ttl 表示过期时间, tto=0表示没有过期时间
# migrate host port key|"" target-db timeout [copy] [replace] [key key...]

# 遍历键
# `keys pattern` 支持glob通配符 - 耗时
# 渐进式遍历 `scan cursor [match pattern] [count number]` 
# 

# 多数据库管理
# `select dbIndex` 切换数据库, 从0开始
# flushdb/flushall 清除/清除全部


```

# 数据类型

https://www.cnblogs.com/javazhiyin/p/11063944.html

键的类型只能为字符串，值支持的`五种数据类型`为：字符串(string)、列表(list)、集合(set)、有序集合(zset)、散列表(hash)。

![alt](Snipaste_2018-07-19_16-09-13.png)

![alt](Snipaste_2018-12-08_16-01-44.png)![alt](Snipaste_2018-12-08_16-03-03.png)

每种数据类型在redis 内部有自己的编码实现, 内部的编码实现有改进不会对外部有什么影响, 多种内部编码实现可以对应不同应用场景

## streaming 类型

https://lolico.me/2020/06/28/Using-stream-to-implement-message-queue-in-springboot/
TODO

Redis5新增了一个Stream的数据类型，这个类型作为消息队列来使用时弥补了List和Pub/Sub的不足并且提供了更强大的功能，比如ack机制以及消费者组等概念

## String-字符串

String数据结构是简单的key-value类型，value其实不仅可以是String，也可以是数字。 常规key-value缓存应用； 常规计数：微博数，粉丝数等。



```sh
# 常用命令: set,get,decr,incr,mget 等

## 设置值 , 过期时间
## `set key value [ex seconds] [px milliseconds] [nx|xx]`
## 设置秒/毫秒级别过期时间, 
## 键必须存在/不存在 才能设置成功, 用于添加/更新 (等价 setnx setxx)
## setnx 实现 分布式锁 (原理: redis的单线程模型, setnx 只有一个client 会设置成功)

> set hello world
OK

## 获取值
## getset key value 设置获取原值

> get hello
"world"

# 批量设置, 获取
127.0.0.1:6379> mset a 1 b 2 c 3
OK
127.0.0.1:6379> mget a b c
1) "1"
2) "2"
3) "3"

## 计数 
## 自增 `incr key`, 值不是整数, 返回错误, 值是整数则返回自增后的结果, 值不存在视作0,自增后返回1
## 自减 `decr key`
## incrby 和 decrby 自增自减指定数字

# 删除

> del hello
(integer) 1
> get hello
(null)

###########################################

# 不常用的命令

## 追加 `append key value` 向 string 尾部追加字符串
## 长度 `strlen key` 每个英文字符 1 byte, 中文 3 byte
## 设置 指定位置的 字符 `setrange key offset value` 偏移量从0开始
## 获得 部分字符串 `getrange key start end`
```

命令的时间复杂度

场景: 

- 缓存 - 业务对象(如: User) 需要序列化
- 计数 - 统计播放量 (真实的计数系统需要考虑: 防作弊, 数据持久化)
- 共享session - 分布式应用
- 限流 - 例如每分钟不能访问超过10次, 同一个ip地址不能在1秒内访问超过n次

## List-列表

![alt](Snipaste_2018-07-19_16-13-36.png)![alt](Snipaste_2018-12-09_13-37-01.png)

允许重复;

Redis list的实现为一个双向链表，即可以支持反向查找和遍历; 

场景: 

- 消息队列; lpush 和 brpop 结合可实现阻塞队列

- 文章列表

    每篇文章使用 hash 类型存储; 用户文章列表使用 list 类型存储

```sh
# 常用命令: lpush,rpush,lpop,rpop,lrange等

######## 添加 lpush rpush linsert ##########

# rpush r即right, 在List中从右向左插入元素 (一旦使用该命令，List就代表是一个列表而不是一个变量，对其使用get方法会报错)

> rpush list-key item
(integer) 1
> rpush list-key item2
(integer) 2
> rpush list-key item # 允许重复元素
(integer) 3

127.0.0.1:6379> rpush list c b a
(integer) 3
127.0.0.1:6379> lrange list 0 -1 # 遍历
1) "c"
2) "b"
3) "a"

# linsert key before|after pivot value 在 pivot 前/后 插入 value


############# 查找 lrange lindex llen ###########################

# lrange key start end 左右闭区间
# 索引从左到右 0 ~ (N-1) ; 从右到左 -1 ~ -N
> lrange list-key 0 -1     # index从 0 开始, -1 表示直到末尾
1) "item"
2) "item2"
3) "item"

# lindex key index 获取 index 处的元素
127.0.0.1:6379> lindex list -1 # 最后一个元素
"a"

# llen key 列表长度


########## 删除 lpop rpop lrem ltrim ################

# lpop l表示left, 返回并删除keyList中的第一个value值 (删除左边第一个)
# rpop key 从右边弹出一个

> lpop list-key
"item"

# lrem key count value 删除值为 value 的 元素
# count > 0, 从左到有删除最多 count个; count < 0, 从右到左删除最多 |count| 个; count = 0, 删除所有.
# ltrim key start end 取子列表


###############修改 lset##################

# lset key index value 修改index 处的元素

################## 阻塞式弹出 blpop brpop ###################

# blpop key [key...] timeout 等待直到有元素返回, 单位 s
# 列表为 nil, 等待 timeout秒后返回, 如果timeout=0, 一直等待下去; 如果在此期间有新的元素添加, 立即返回这个新元素
# 列表不为nil, 立即返回结果
```

## Set-集合

![alt](Snipaste_2018-07-19_16-47-33.png)

无序不可重复

set提供了判断某个成员是否在一个set集合内的重要接口，这个也是list所不能提供的

取交集, 并集

![alt](Snipaste_2018-12-09_19-07-10.png)

```sh

# 添加 sadd key element [element...]
# sadd s即为Set，Set add value的意思, 给keySet插入value，Set集合是无序的，没有头部和尾部

> sadd set-key item
(integer) 1
> sadd set-key item2
(integer) 1
> sadd set-key item3
(integer) 1
> sadd set-key item         # 重复, 不可插入
(integer) 0

# smembers key 集合中的所有元素
# s=Set，members即在Set集合中的所有元素,  返回keyset中所有的value值

> smembers set-key
1) "item"
2) "item2"
3) "item3"

# 判断是否存在 sismember key element
# s=Set，ismember即某个元素是否在Set集合的内存中

> sismember set-key item4
(integer) 0     # 不存在
> sismember set-key item
(integer) 1     # 存在

# srandmember key [count] 随机返回count个数的元素, 默认返回1个

# 移除 srem key element [element...]
# srem 即Set remove, 删除keySet中指定的value值

> srem set-key item2
(integer) 1
> srem set-key item2
(integer) 0

> smembers set-key
1) "item"
2) "item3"

# 元素个数 scard key

# spop key [count] 随机弹出一个元素

############ 集合间的操作 #############################

# 交集 sinter key [key...]

# 并集 sunion key [key...]

# 差集 sdiff key [key...]


```

场景:

- 标签 - sadd

    给用户添加标签 ; 给标签添加用户

    添加过程应该在同个事务中完成, 至于如何实现, 可通过 redis 事务, or 通过 lua 脚本

- 生成随机数, 如抽奖 - spop/srandmember

- 社交需求 - sadd + sinter

## Hash-哈希

![alt](Snipaste_2018-07-19_16-53-24.png)

适合用于存储对象; 举个例子🌰： 一个电商网站项目的首页就使用了redis的hash数据结构进行缓存

```sh
# 常用命令： hget,hset,hgetall 等。

# HSET key field value 将哈希表 key 中的域 field 的值设为 value

> hset hash-key sub-key1 value1
(integer) 1
> hset hash-key sub-key2 value2
(integer) 1
> hset hash-key sub-key1 value1     # 如果域 field 已经存在于哈希表中，旧值将被覆盖
(integer) 0

# hgetall 即 hash get all, 根据hash的key, 获取 field和field_value, 返回list, 长度为hash的2倍

> hgetall hash-key
1) "sub-key1"     # field
2) "value1"       # filed value
3) "sub-key2"
4) "value2"

# 删除

> hdel hash-key sub-key2
(integer) 1 # 返回结果为 成功删除的 field 个数
> hdel hash-key sub-key2
(integer) 0

# hget 获取指定filed的值

> hget hash-key sub-key1
"value1"

> hgetall hash-key
1) "sub-key1"
2) "value1"

# hlen key 计算field 个数

# 批量设置 批量获取 field value
# hmget key field [field...]
# hmset key field value [field value]

# hexists key field 判断field是否存在

# hkeys key 返回所有 field
# hvals key 返回所有 value
# hgetall key 返回 所有 field-value

# hstrlen key field  计算 field 长度

```

hash 这种类型和结构化数据库的表最类似, key 记录为 id, value就是一行数据. 也有不同之处: hash 每个 key 可以有不同的 field. 而关系型数据库一旦添加新的 "列", 每行数据都要赋值, 即使是 null; 关系型数据库方便进行关系查询, 而 redis 中的 hash 模拟 复杂查询就很麻烦

场景: 缓存. 相对于直接使用String 类型使业务对象系列化构建缓存, 使用 hash 构造缓存更直观, 更新操作更便捷

至此, 对于 缓存场景, 有 3种方案:

- 使用 string 类型, 每个 key-value 就是一个属性 eg: `set usre:1:name xy`, `set user:1:age 18` ...

    这种最简单, 内存占用最大, 因为 key 的冗余内容较多

- 使用 string 类型, 将一个业务对象序列化然后用一个 键值对保存

    内存占用不多, 但是序列化反序列化有时间开销, 尤其 更新 操作的时候, 很麻烦

- 使用 hash 类型, 每个 键值对保存一个业务对象,  属性 通过 field-value 对保存


## ZSet-有序集合

![alt](Snipaste_2018-07-19_17-03-19.png)

类似 set , `元素不可重复` (分数可以重复); 此外, sorted set增加了一个权重参数score，使得集合中的元素能够按score进行`有序排列`

场景: 在直播系统中，实时排行信息包含直播间在线用户列表，各种礼物排行榜

```sh

# 添加 zadd key score member [score member ...]
# 四个选项: nx: member必须不存在才可成功, 用于 添加; xx: member必须存在才可成功, 用于 修改; ch: 元素发生变化的个数 ; incr: 对 score 自增
# 728 是分数, score 值可以是整数值或双精度浮点数(如果某个 member 已经是有序集的成员，那么更新这个 member 的 score 值)
> zadd zset-key 728 member1
(integer) 1
> zadd zset-key 982 member0
(integer) 1
> zadd zset-key 982 member0     # member一样, score也一样, 无法插入, 无法更新score
(integer) 0

# 元素个数 zcard key

# 计算某个元素的分数 zscore key member 

# 计算某个成员排名 zrank/zrevrank key member 正数/倒数

# zrem key member [member...]

# 增加分数 zincrby key increment member 分数增加increment

# 获取指定排名范围的元素 zrange/zrevrange key start end [withscores] 从低到高/从高到低

> zrange zset-key 0 -1 withscores # 全范围
1) "member1"
2) "728"
3) "member0"
4) "982"

# 获取指定分数范围的元素 zrangbyscore/zrevrangebyscore key min max [withscores] [limit offset count]
# min max 支持 开闭区间 (小括号, 中括号), 正负无穷 (+-inf)
> zrangebyscore zset-key 0 800 withscores
1) "member1"
2) "728"

# 删除 zrem key member
# 删除指定排名范围内的所有元素 (升序) zremrangebyrank key start end
# 删除指定分数范围内的元素 (升序) zremrangebyscore key min max

> zrem zset-key member1
(integer) 1
> zrem zset-key member1
(integer) 0

# 返回指定分数范围元素个数 zcount key min max

############## 集合间操作 ################################

#交集 zinterstore
#并集 zunionstore
```


# lua脚本



# 单线程架构

不存在并发执行命令

- 纯内存访问
- 非阻塞io - epoll 实现io多路复用
- 单线程没有上下文切换


# 内存淘汰机制

可以设置内存最大使用量，当内存使用量超过时施行淘汰策略，具体有 6 种淘汰策略。

![alt](Snipaste_2018-07-20_12-08-58.png)

## 三道保险保证 key 过期处理

定期删除+惰性删除

定期删除: redis是每隔100ms随机抽取一些key来检查和删除

惰性删除: 获取某个key的时候，redis会检查一下 ，这个key如果设置了过期时间那么是否过期了？如果过期了此时就会删除

通过上述两种手段结合起来，保证过期的key一定会被干掉

但是实际上这还是有问题的，如果定期删除漏掉了很多过期key，然后你也没及时去查，也就没走惰性删除; 此时会走内存淘汰机制


## 有哪些淘汰机制

- noeviction(默认策略)：内存满了, 对于写请求不再提供服务，直接返回错误, 没人用

- allkeys-lru：从所有key中使用LRU算法进行淘汰 (常用)

- allkeys-random：从所有key中随机淘汰数据 (没人用)

- volatile-lru：从设置了过期时间的key中使用LRU算法进行淘汰 

- volatile-random：从设置了过期时间的key中随机淘汰

- volatile-ttl：在设置了过期时间的key中，根据key的过期时间进行淘汰，有更早过期时间的key优先移除

volatile-xx 的一般不用

## 内存回收算法

Redis中采用两种算法进行内存回收，引用计数算法以及LRU算法;

如果使用 Redis 来缓存数据时，要保证所有数据都是热点数据，可以将内存最大使用量设置为热点数据占用的内存量，然后启用 allkeys-lru 淘汰策略，将最近最少使用的数据淘汰。

作为内存数据库，出于对性能和内存消耗的考虑，Redis 的淘汰算法（LRU、TTL）实际实现上并非针对所有 key，而是抽样一小部分 key 从中选出被淘汰 key.

LRU(Least Recently Used)，即最近最少使用, 根据key的最近被访问的频率进行淘汰，很少被访问的优先被淘汰，被访问的多的则被留下来。 [Java 提供的 LinkedHashMap 可以实现](https://www.cnblogs.com/lzrabbit/p/3734850.html) //todo

## 如何配置淘汰策略

### 通过配置文件

通过在Redis安装目录下面的redis.conf

redis的配置文件不一定使用的是安装目录下面的redis.conf文件，启动redis服务的时候是可以传一个参数指定redis的配置文件的

```
//设置Redis最大占用内存大小为100M
maxmemory 100mb
// 淘汰策略
maxmemory-policy allkeys-lru
```

Redis使用的是近似LRU算法，它跟常规的LRU算法还不太一样。近似LRU算法通过随机采样法淘汰数据，每次随机出5（默认）个key，从里面淘汰掉最近最少使用的key。

```
// 修改采样数量
maxmemory-samples 10
```

### 通过命令运行时修改

Redis支持运行时通过命令动态修改内存大小

果不设置最大内存大小或者设置最大内存大小为0，在64位操作系统下不限制内存大小

```
//设置Redis最大占用内存大小为100M
127.0.0.1:6379> config set maxmemory 100mb
//获取设置的Redis能使用的最大内存大小
127.0.0.1:6379> config get maxmemory

config get maxmemory-policy
config set maxmemory-policy allkeys-lru
```

# 持久化

## 备份选型

Redis 的持久化机制有两种，第一种是快照（RDB），第二种是 AOF 日志。

快照是一次全量备份，AOF 日志是连续的增量备份。快照是内存数据的二进制序列化形式，在存储上非常紧凑，而 AOF 日志记录的是数据修改的指令文本, 有冗余。

RDB是通过Redis主进程fork子进程，让子进程执行磁盘 IO 操作来进行 RDB 持久化，AOF 日志存储的是 Redis 服务器的顺序指令序列，AOF 日志只记录对内存进行修改的指令记录。即RDB记录的是数据，AOF记录的是指令

不要仅仅使用 RDB，因为那样会导致你丢失很多数据，因为RDB是隔一段时间来备份数据

也不要仅仅使用 AOF，因为那样有两个问题，第一，速度慢; 第二，aof日志文件出bug 的几率大于 rdb快照的二进制文件

用RDB恢复内存状态会丢失很多数据，重放AOP日志又很慢。Redis4.0推出了混合持久化来解决这个问题。将 rdb 文件的内容和增量的 AOF 日志文件存在一起。这里的 AOF 日志不再是全量的日志，而是自持久化开始到持久化结束的这段时间发生的增量 AOF 日志，通常这部分 AOF 日志很小。于是在 Redis 重启的时候，可以先加载 rdb 的内容，然后再重放增量 AOF 日志就可以完全替代之前的 AOF 全量文件重放

## RDB-快照持久化

### RDB持久化概念

写操作到达一定密度, 触发快照, 频率可配置

优:

- rdb文件是经过压缩的二进制文件, 体积小

- redis加载 rdb文件速度快于 aof 的方式

缺点:

- rdb 持久化无法做到实时持久化

    利用快照进行备份。如果系统发生故障，将会丢失最后一次创建快照之后的数据

- rdb 文件格式在不同版本redis中有不同, 兼容性不好

### 触发机制-如何开启

自动触发, 手动触发

对于手动触发: `save` 阻塞 redis 直到 rdb 过程完成, 一般不用;  `bgsave` 使得redis进程创建新的进程执行 rdb过程, 基本没有阻塞

对于自动触发: 📌 `save m n` 在 m 秒内, 数据集有了 n 次修改, 自动触发 bgsave; 📌 `debug reload` 重新加载 redis; 📌 `shutdown`

bgsave 流程示意图:

![alt](Snipaste_2018-12-10_18-31-17.png)

### RDB文件

rdb文件存储在 dir 配置指定的目录下,文件名通过 dbfilename 配置指定

## AOF-日志持久化

### aof概念

redis的每个写操作都会被追加到一个日志文件中

将写命令添加到 AOF 文件（Append Only File）的末尾。 --------- 以独立日志文件的方式记录每次写命令, 解决了 RDB方式不能实时持久化的问题

rdb, aof同时存在时, redis重启时优先载入 aof 文件来恢复数据 (aof 比 rdb 记录的数据更全)

通过 `appendonly yes` 配置 开启 aof, aof文件名通过 appendfilename 配置设置, 默认为 appendonly.aof, 保存路径同rdb中的dir配置

编辑 /etc/redis/redis.conf, 

```sh
appendonly yes
```

### 写入原理

以`文本方式`写入`aof缓存区` , 然后由操作系统决定什么时候将该内容同步到硬盘，用户可以调用 file.flush() 方法请求操作系统尽快将缓冲区存储的数据同步到硬盘。可以看出`写入文件的数据不会立即同步到硬盘`上，在将写命令添加到 AOF 文件时，要根据需求来保证何时同步到硬盘上。

- 为什么使用文本格式? 📌 文本格式具有很好的兼容性; 📌 文本格式很容易处理追加操作; 📌 可读性, 直接修改处理;

- 为什么命令写入需要先加入"aof缓存区"? 📌 redis是单线程执行命令, 如果不使用"缓存区", 而是直接写入到硬盘, 则硬盘读写速度会成为瓶颈; 📌 redis 可使用多种不通过的 缓存区-硬盘 同步策略, 在性能和安全中做出平衡.

### 文件同步

有三个同步选项:

- always 每个写命令都同步 (always 选项会严重减低服务器的性能)
- everysec 每秒同步一次 (everysec 选项比较合适，可以保证系统奔溃时只会丢失一秒左右的数据，并且 Redis 每秒执行一次同步对服务器性能几乎没有任何影响)
- no 让操作系统来决定何时同步 (no 选项并不能给服务器性能带来多大的提升，而且也会增加系统奔溃时数据丢失的数量)

### 重写机制

随着服务器写请求的增多，AOF 文件会越来越大。Redis 提供了一种将 AOF 重写的特性，能够去除 AOF 文件中的冗余写命令。压缩文件体积.

重写的意义:

- 进程内超时命令不会再写入
- 无效命令不再写入, 只写入生成最终数据的命令
- 多条写命令可以合并

如何触发重写:

- 手动触发: `bgrewriteaof`
- 自动触发: 

### aof 损坏

加载损坏的aof 文件会启动失败; 这是可以先备份这个文件然后尝试通过 `redis-check-aof-fix` 修复

# 发布与订阅

订阅者订阅了频道之后，发布者向频道发送字符串消息会被所有订阅者接收到 (某个客户端使用 SUBSCRIBE 订阅一个频道，其它客户端可以使用 PUBLISH 向这个频道发送消息。)



发布与订阅模式和观察者模式有以下不同:

- 观察者模式中，观察者和主题都知道对方的存在；而在发布与订阅模式中，发布者与订阅者不知道对方的存在，它们之间通过频道进行通信。

- 观察者模式是同步的，当事件触发时，主题会去调用观察者的方法，然后等待方法返回；而发布与订阅模式是异步的，发布者向频道发送一个消息之后，就不需要关心订阅者何时去订阅这个消息。

  ![alt](Snipaste_2018-07-20_12-21-29.png)

# 事务

Redis 最简单的事务实现方式是使用 MULTI 和 EXEC 命令将事务操作包围起来。

redis事务在执行的中途遇到错误，不会回滚而是继续执行后续命令；（违反原子性） .那么redis为什么还要事务呢? - 事务中的多个命令被一次性发送给服务器，而不是一条一条发送，这种方式被称为流水线，它可以减少客户端与服务器之间的网络通信次数从而提升性能。

redis事务中的命令在执行exec命令前会被放入队列, 并不会实际执行, 也就不存在隔离级别的概念

watch key1 key2... : 类似于乐观锁, 如果某个事务提交时, 有其他client 改变了任意一个 key, 则使用 exec 执行事务时, 事务队列不会被执行;


# 事件

Redis 服务器是一个事件驱动程序。

## 文件事件

服务器通过套接字与客户端或者其它服务器进行通信，文件事件就是对套接字操作的抽象。

Redis 基于 Reactor 模式开发了自己的网络事件处理器，使用 I/O 多路复用程序来同时监听多个套接字，并将到达的事件传送给文件事件分派器，分派器会根据套接字产生的事件类型调用相应的事件处理器。

![alt](Snipaste_2018-07-20_12-25-16.png)

## 时间事件

服务器有一些操作需要在给定的时间点执行，时间事件是对这类定时操作的抽象。

时间事件又分为：定时事件, 周期事件

Redis 将所有时间事件都放在一个无序链表中，通过遍历整个链表查找出已到达的时间事件，并调用相应的事件处理器。

## 事件的调度&执行

# redis集群

通过使用 slaveof host port 命令来让一个服务器成为另一个服务器的从服务器。

一个从服务器只能有一个主服务器，并且不支持主主复制。

## 连接过程

1. 主服务器创建快照文件，发送给从服务器，并在发送期间使用缓冲区记录执行的写命令。快照文件发送完毕之后，开始向从服务器发送存储在缓冲区中的写命令；

1. 从服务器丢弃所有旧数据，载入主服务器发来的快照文件，之后从服务器开始接受主服务器发来的写命令；

1. 主服务器每执行一次写命令，就向从服务器发送相同的写命令。

## 主从链

随着负载不断上升，主服务器可能无法很快地更新所有从服务器，或者重新连接和重新同步从服务器将导致系统超载。为了解决这个问题，可以创建一个中间层来分担主服务器的复制工作。中间层的服务器是最上层服务器的从服务器，又是最下层服务器的主服务器。

![alt](Snipaste_2018-07-20_12-29-01.png)

# 哨兵 Sentinel

下线推举

Sentinel（哨兵）可以监听主服务器，并在主服务器进入下线状态时，自动从从服务器中选举出新的主服务器。

# 分片

分片是将数据划分为多个部分的方法，可以将数据存储到多台机器里面，也可以从多台机器里面获取数据，

> 假设有 4 个 Reids 实例 R0，R1，R2，R3，还有很多表示用户的键 user:1，user:2，... 等等，有不同的方式来选择一个指定的键存储在哪个实例中。最简单的方式是范围分片，例如用户 id 从 0~1000 的存储到实例 R0 中，用户 id 从 1001~2000 的存储到实例 R1 中，等等。但是这样需要维护一张映射范围表，维护操作代价很高。还有一种方式是哈希分片，使用 CRC32 哈希函数将键转换为一个数字，再对实例数量求模就能知道应该存储的实例。

根据执行分片的位置，可以分为三种分片方式：

- 客户端分片：客户端使用 [#一致性哈希](#一致性哈希) 等算法决定键应当分布到哪个节点。
- 代理分片：将客户端请求发送到代理上，由代理转发请求到正确的节点上。
- 服务器分片：Redis Cluster。


# 一个简单的论坛系统

该论坛系统功能如下:

- 可以发布文章；
- 可以对文章进行点赞；
- 在首页可以按文章的发布时间或者文章的点赞数进行排序显示。

## 文章信息

文章包括标题、作者、赞数等信息，在关系型数据库中很容易构建一张表来存储这些信息，在 Redis 中可以使用 HASH 来存储每种信息以及其对应的值的映射。

Redis 没有关系型数据库中的表这一概念来将同种类型的数据存放在一起，而是使用命名空间的方式来实现这一功能。键名的前面部分存储命名空间，后面部分的内容存储 ID，通常使用 : 来进行分隔。例如下面的 HASH 的键名为 article:92617，其中 article 为命名空间，ID 为 92617。

![alt](Snipaste_2018-07-20_12-35-52.png)

## 点赞功能

![alt](Snipaste_2018-07-20_12-39-13.png)

点赞、取消点赞是高频次的操作，若每次都读写数据库，大量的操作会影响数据库性能，所以需要做缓存

定时从 redis 中取出点赞数据持久化存入 db, 比如 2h

## 对文章进行排序

![alt](Snipaste_2018-07-20_12-39-35.png)


# 一致性哈希

//todo


# 整合 spring boot2

## jedis

项目地址: https://github.com/xetorthio/jedis

Jedis 实例是线程不安全的, 为什么?

- Jedis类中有RedisInputStream和RedisOutputStream两个属性，而发送命令和获取返回值都是使用这两个成员变量，显然，这很容易引发多线程问题。

可以通过JedisPool连接池去管理实例，在多线程情况下让每个线程有自己独立的jedis实例 来实现线程安全

## lettuce

https://www.cnblogs.com/throwable/p/11601538.html - 使用

spring boot2 默认使用

Lettuce 基于 netty, 线程安全, 大吞吐量, 

需要添加依赖 `common-pool2`, 用于提供连接池, [自定义对象连接池](https://blog.csdn.net/qq447995687/article/details/80233621) //todo


```yml
spring:
  application:
    name: redis-demo
  cache:
    type: redis
    redis:
        # 单独为不同的缓存可以配置不同的超时时间
      time-to-live: 20000 #默认缓存超时时间, ms
      cache-null-values: false #是否缓存空值
  redis:
    port: 6379
    host: localhost
    lettuce:
      pool:
        max-active: 8
        max-wait: -1
        max-idle: 8
        min-idle: 0
    timeout: 10000  #redis 连接超时时间ms
    database: 0
```



但是有个bug, 导致在高并发下,  netty 产生堆外内存溢出 OutOfDirectMemoryError; 这个 bug 是: 若不指定 netty堆外内存用量, 默认会采用 -Xmx 的值, 这个值是一定的, 但是堆外内存不会及时释放, 所以不管 -Xmx 设置多大, 总是会用完, OutOfDirectMemoryError, 推荐 jedis 替代 (https://github.com/seata/seata/issues/2164)

```xml

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
    <exclusions>
        <exclusion>
            <groupId>io.lettuce</groupId>
            <artifactId>lettuce-core</artifactId>
        </exclusion>
    </exclusions>
</dependency>

<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
</dependency>
```

## 定义序列化器

[GenericJackson2JsonRedisSerializer Jackson2JsonRedisSerializer 区别](https://blog.csdn.net/bai_bug/article/details/81222519) - 前者可以直接从redis 反序列化成对象, 但是占用内存高 (使用GenericJackson2JsonRedisSerializer序列化时，会保存序列化的对象的包名和类名，反序列化时以这个作为标示就可以反序列化成指定的对象)

# 面试问题

https://blog.csdn.net/varyall/article/details/81205183 //todo

# 分布式锁



## 实现原理

### 分布式锁需要满足的要求

- 互斥

    同一时刻只能有一个线程获得锁

- 防止死锁 (即要能够设置锁有效时间)

    比如有个线程获得锁的同时，还没有来得及去释放锁，就因为系统故障或者其它原因使它无法执行释放锁的命令,导致其它线程都无法获得锁

- 锁最好满足可重入

    同一个线程可以重复拿到同一个资源的锁。重入锁非常有利于资源的高效利用


### 加锁机制

线程去获取锁，获取成功: 执行lua脚本，保存数据到redis数据库。保存成功则表示获取锁成功

线程去获取锁，获取失败: 一直通过while循环尝试获取锁，获取成功后，执行lua脚本，保存数据到redis数据库。

为什么使用 lua:
如果你的业务逻辑复杂的话，通过封装在lua脚本中发送给redis，而且redis是单线程的，这样就保证这段复杂业务逻辑执行的原子性

### watch dog 自动延时机制

"看门狗" 用于这样的场景: 线程1 获得锁, 过了段时间, 业务还没有执行完，锁的过期时间就到了，线程1 还想持有锁的话，就会启动一个watch dog后台线程，不断的延长锁key的生存时间

默认这个看门狗线程是不启动的，还有就是这个看门狗启动后对整体性能也会有一定影响，所以不建议开启看门狗。


### redisson 如何实现可重入锁

Redis存储锁的数据类型是 Hash类型, Hash数据类型的 item 值包含了当前线程信息

`key: {item1: value1, item2: value2}`, key 为固定值 "redisson", item 构成为 uuid + ":" + current thread id, value1 为 当前锁获取次数



## redis 实现 分布式锁

要求:

- set ex 300 nx ... 设置过期时间和占位要是原子操作
- 执行完业务要删除锁
- 要删除自己的锁, 而不要删除别人的锁 (值存为 uuid or thread id)

伪代码

```java
Resp withRedisLock() {
    uuid = new uuid().tostring()
    sucess = redis.setNx("lock", uuid, 30s)
    try {
        if (success) {
            return biz.exec()
        } else {
            return withRedisLock()// 自旋
        } 
    }
    finally {
        // 查找, 删除必须是原子操作, 一下不符合, 必须用 lua 脚本
        // lockValue = redis.get("lock");
        // if (lockValue == uuid) {
        //     redis.delete("lock")
        // }

        deleteScript = "if redis.call('get', KEYS[1]) == ARGV[1] then return redis.call('del', KEYS[1]) else return 0 end"
        redis.exec(deleteScript)
    }
    
}

```


## redisson 实现

redis 分布式锁的官方推荐

watch dog (key 自动续期, 防止业务超时锁被删除了)


```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
<!-- 推荐使用 redisson starter -->
<dependency>
    <groupId>org.redisson</groupId>
    <artifactId>redisson</artifactId>
    <version>3.8.2</version>
    <optional>true</optional>
</dependency>   
```

springboot 中配置

```java

    @Bean(destroyMethod = "shutdown")
    public RedissonClient redisson(@Value("classpath:/redisson-config.yml") Resource configFile) throws IOException {
        Config config = Config.fromYAML(configFile.getInputStream());
        return Redisson.create(config);
    }

```
