---
title: monitor-system-grafana-prometheus
tags: 
  - grafana
  - prometheus
  - influxdb
  - clickhouse
---

https://github.com/Kalasearch/grafana-tutorial https://kalasearch.cn/blog/grafana-with-prometheus-tutorial/
https://lingchu.me/post/158.html
https://www.cnblogs.com/williamjie/p/9448904.html

<!-- more -->

<!-- TOC -->

- [Spring Boot Actuator 整合 Prometheus](#spring-boot-actuator-整合-prometheus)
- [grafana](#grafana)
- [influxdb](#influxdb)
  - [时序数据库](#时序数据库)
    - [为什么要使用时序数据库](#为什么要使用时序数据库)
    - [时序数据的特征](#时序数据的特征)
    - [使用场景](#使用场景)
    - [开源方案](#开源方案)
  - [和 promethus 区别](#和-promethus-区别)
  - [数据模型](#数据模型)
  - [basic concept](#basic-concept)
    - [数据库](#数据库)
    - [Retention Policy（RP） 数据保留策略](#retention-policyrp-数据保留策略)
    - [Shard Group 数据切片组](#shard-group-数据切片组)
- [hbase](#hbase)

<!-- /TOC -->


# Spring Boot Actuator 整合 Prometheus

https://mp.weixin.qq.com/s/MWQImwMhcFglmHZ4lIcxVA

https://mp.weixin.qq.com/s/wkfIQc3IQD_6y7eCsGd_Rw AlertManager

https://mp.weixin.qq.com/s/40ULB9UWbXVA21MxqnjBxw Node Exporter

https://mp.weixin.qq.com/s/GImvM_F2XlMjO0a5xnJuFg

# grafana

ELK + Telegraf + Grafana
推荐 ELK，画图这块可以看 Grafana 看能不能满足，不行的话自己写也可以，ES 有 API, 我们实现的一些思路，可以参考 https://blog.ops-coffee.cn/elk

# influxdb

## 时序数据库

### 为什么要使用时序数据库

普通的关系型数据库不行吗?

用其他数据库也是可以用的，只是效率不同, 专门针对基于时间的大量高频数据优化的，其存储更小、查询更快


### 时序数据的特征

```
数据顺序追加

很少修改/删除, 只有增加/查询, 删除也是批量删除

冷数据需要定期归档

数据查询一般以一段时间为范围

业务操作不需要具备事务的能力
```

### 使用场景

用于 监控场景

比如: 物联网系统记录设备运行数据, 互联网应用记录用户使用日志, 广告点击日志, 电商的交易记录


### 开源方案

- OpenTSDB ,底层采用HBase作为存储

- Prometheus, 保存在内存数据库中，并且定时保存到硬盘上。需要远端存储来保证可靠和扩展性

- InfluxDB, 单机开源的时序数据库,由Go语言编写, 分布式需要商业化支持

- Timescale, 固定Schema，底层基于PG


## 和 promethus 区别

InfluxDB面向的是通用时序平台，包括日志监控等场景。而Prometheus更侧重于指标方案。两个系统之间有非常多的相似之处，包括采集，存储，报警，展示等等

Influxdata的组合有：telegraf+Influxdb+Kapacitor+Chronograf

Promethues的组合有：exporter+prometheus server+AlertManager+Grafana

## 数据模型

- 度量 Metric 或者 Measurement, 类似关系型数据库里的表（Table），代表一系列同类时序数据的集合

  例如为空气质量传感器建立一个 Table，存储所有传感器的监测数据。

- 标签 Tag：描述数据源的特征，通常不随时间变化

  (Tag 由 Tag Key、Tag Value 组成，两者均为 String 类型), 
  
  可类比加上索引的表字段 (可能多个字段都是 tag), 会作为记录的主键, Tags组合用来唯一标识 metric

- 时间戳 Timestamp：Timestamp代表数据产生的时间点，可以写入时指定，也可由系统自动生成；

  类比表字段, 插入一条记录, 变化一个值

- 量测值 Field：Field描述数据源的量测指标，通常随着时间不断变化，类比 MySQL 的 表字段

  例如传感器设备包含温度、湿度等Field；

  类比表字段

- 数据点Data Point: 数据源在某个时间产生的某个量测指标值（Field Value）称为一个数据点，

  类似 传统表的一行记录

  数据库查询、写入时按数据点数来作为统计指标；

- 时间线 Time Series ：数据源的某一个指标随时间变化，形成时间线，Metric + Tags + 某个Field 组合确定一条时间线



时序查询一般是这样: 通过 metric + tags 确定 查那个数据源的 series (时间线), 通过 field 确定查数据源的哪个属性的时间线


## basic concept


### 数据库

用户可以通过create database xxx来创建一个数据库

### Retention Policy（RP） 数据保留策略

核心作用有3个：指定数据的过期时间，指定数据副本数量以及指定ShardGroup Duration。

- RP是数据库级别而不是表级别的属性。这和很多数据库都不同。

- 每个数据库可以有多个数据保留策略，但只能有一个默认策略。

- 不同表可以根据保留策略规划在写入数据的时候指定RP进行写入, 如果没有指定任何RP，则使用默认的RP。




```sh

# 创建语法
# 其中retention_policy_name表示RP的名称，database_name表示数据库名称，duration表示TTL，n表示数据副本数。
# CREATE RETENTION POLICY ON <retention_policy_name> ON <database_name> DURATION <duration> REPLICATION <n> [SHARD DURATION <duration> ] [DEFAULT]
CREATE RETENTION POLICY "one_day_only" ON "water_database" DURATION 1d REPLICATION 1 SHARD DURATION 1h DEFAULT 



```


### Shard Group 数据切片组

实现了数据分区, 根据 timestamp, 每个Shard Group只存储指定时间段的数据.

> 只是一个逻辑概念, 里面包含了大量Shard，Shard才是InfluxDB中真正存储数据以及提供读写服务的概念
> 不同Shard Group对应的时间段不会重合。比如2017年9月份的数据落在Shard Group0上，2017年10月份的数据落在Shard Group1上。
> 每个Shard Group对应多长时间是通过Retention Policy中字段”SHARD DURATION”指定的，如果没有指定，也可以通过Retention Duration（数据过期时间）计算出来

为什么需要将数据按照时间分成一个一个Shard Group? 

-  将数据按照时间分割成小的粒度会使得数据过期实现非常简单，InfluxDB中数据过期删除的执行粒度就是Shard Group，系统会对每一个Shard Group判断是否过期，而不是一条一条记录判断。

-  实现了将数据按照时间分区的特性, 查询是效率高, 类似二分查找

落在一个Shard Group中的数据又是如何映射到哪个Shard上呢？

- InfluxDB是根据hash(Series)将时序数据映射到不同的Shard，而不是根据Measurement进行hash映射，这样会使得相同Series的数据肯定会存在同一个Shard中，但这样的映射策略会使得一个Shard中包含多个Measurement的数据



# hbase

https://zhuanlan.zhihu.com/p/37964096 对比选型

https://zhuanlan.zhihu.com/p/145551967 简介

https://zhuanlan.zhihu.com/p/65994704
