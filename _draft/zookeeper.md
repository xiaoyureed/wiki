---
title: zookeeper
tags: [zookeeper]
categories: distributed system
---

<!--more-->

# zk是什么

# 为什么使用zk

- 注册中心
- 配置中心

# 怎么用zk

## 命令行使用 zk client

```sh
# 进入命令行客户端
docker run --link zookeeper-x:zk -it --rm zookeeper zkCli.sh -server zk
ls /
get /
create -e /k1 v1 # 创建临时节点
```

## 作为微服务注册中心

spring cloud 提供了 zookeeper client, 配置 connect-string (格式 host:port )即可. 会在 zk 中创建临时节点, 位于 /services/(application-name)/(uuid)

相比 eureka 保证 ap, zk 优先保证 cp, 只要 client 断开和 server的心跳, zk 马上就会删除对应的临时节点

```sh
docker run -d --name zookeeper-x -p 2181:2181 zookeeper
docker logs -f --tail=10 zookeeper-x
```
