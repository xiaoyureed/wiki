---
title: Big Data Introduce
tags: [data]
categories: outline
---


https://knownsec-fed.com/2018-08-31-jian-shu-da-shu-ju-shi-shi-chu-li-kuang-jia/

廖雪峰 大数据 开课吧
<!--more-->

# 概念介绍

## 实时计算 vs 离线计算



- 流式/实时计算 , 表示那些实时或者低延时的流数据处理过程, 比如实时ETL、实时监控等，延时一般都在毫秒级;

  - 比较流行的实时框架有Spark Streaming与Flink, 数据存储在Druid中。

  - Spark Streaming属于微批处理，是一种把流当作一种批的设计思想，具有非常高的吞吐量但延时也较高，这使得Streaming的场景也得到了一定的限制；Flink则是事件驱动的流处理引擎，是一种把批当作一种有限的流的设计思想，具有高吞吐，低延时，高性能的特点

- 离线计算: Kylin, 数据存储在HBase中

## OLTP Vs OLAP

OLTP（On-Line Transaction Processing），可称为在线事务处理，一般应用于在线业务交易系统，比如银行交易、订单交易等。OLTP的主要特点是能够支持频繁的在线操作（增删改），以及快速的访问查询。主要用来进行事务处理，满足ACID原则。就是我们经常说的关系数据库，增删查改就是我们经常应用的东西, 典型如关系型数据库 MySQL

OLAP（On-Line Analytical Processing），可称为在线分析处理，较多的应用在数据仓库领域，支持复杂查询的数据分析，侧重于为业务提供决策支持, 读取较多，更新较少. 目前常见是的实时OLAP场景，比如Druid（Apache Druid，不同于阿里Druid）、ClickHouse, doris等存储组件

## 行式存储 Vs 列式存储

行式存储（Row-based）: 常见的关系型数据库比如MySQL、Oracle; 总的来说，行存有利于写，但缺不利于读，因为行存是把同一条数据存放在相同位置，这样增删改比较高效，但是查询时会增加io的消耗, 一般应用于OLTP场景;

列式存储（Column-based）:常见的列存包括Parquet、Arrow, 列存有利于读，但不利于写, 最大特点是能够减少不必要的io消耗, 更适应于OLAP场景

## Hadoop家族

HDFS、MapReduce、HBase, 海量存储和分析的标配


# spark





# storm


# Presto 

分布式大数据SQL查询引擎
