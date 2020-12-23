---
title: Distributed log collection
categories: distributed system
---

logstash+kibana+elasticsearch+redis 开发强大的日志分析平台

log.io: nodejs 开发的实时日志收集系统

Graylog: 易用、功能丰富的日志管理系统， 部署、维护、搜索比 ELK 简单

https://elasticsearch.cn/ es 中文网

https://kalasearch.cn/blog/chapter3-elastic-search-and-lucene/ 卡拉搜索上的一篇教程


<!--more-->

<!-- TOC -->

- [ElasticSearch](#elasticsearch)
  - [概念](#概念)
  - [使用场景](#使用场景)
  - [启动](#启动)
  - [中文分词插件](#中文分词插件)
  - [使用](#使用)
    - [meta info api](#meta-info-api)
    - [index api](#index-api)
    - [document api](#document-api)
    - [mapping api](#mapping-api)
    - [search api](#search-api)
    - [数据类型](#数据类型)
  - [Java client](#java-client)
- [logstash](#logstash)
- [filebeat](#filebeat)
- [kibana](#kibana)

<!-- /TOC -->

## ElasticSearch

https://www.elastic.co/guide/en/elasticsearch/reference/index.html

https://github.com/medcl 中文大牛
https://elasticsearch.cn/ 中文社区

### 概念

https://zhuanlan.zhihu.com/p/37964096 综合对比

Elastic 本质上是一个分布式数据库，允许多台服务器协同工作，每台服务器可以运行多个 Elastic 实例

ES 需要在创建字段前要预先建立 Mapping，Mapping 中包含每个字段的类型信息，ES 需要根据 Mapping 为字段建立合适的索引。由于这个 Mapping 的存在，ES 中的字段一但建立就不能再修改类型了

- index, 类似 MySQL 的 db

- type 类似 table (最新版移除了)

- document 类似 row

  同一个 Index 里面的 Document，不要求有相同的结构（scheme），但是最好保持相同，这样有利于提高搜索效率

- 数据类型

  https://www.cnblogs.com/betterwgo/p/11571275.html


### 使用场景

- 搜索
- 地理空间数据处理分析可视化
- 日志处理分析


### 启动

ref: https://www.jianshu.com/p/d8b0c736070f

```sh
mkdir -p /mydata/es/config
mkdir -p /mydata/es/data
mkdir -p /mydata/es/plugins
echo "network.host: 0.0.0.0" >> /mydata/es/config/elasticsearch.yml
chmod -R 777 es/
# 单点启动
docker run -d --name es -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms64m -Xmx128m" -v ~/docker_data/es/config/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml -v ~/docker_data/es/data:/usr/share/elasticsearch/data -v ~/docker_data/es/plugins:/usr/share/elasticsearch/plugins elasticsearch:7.9.2

# 不映射目录版本
#docker run -d --name es -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -v es:/usr/share/elasticsearch/data  elasticsearch:7.9.2


# kibana
# localhost:5601

docker run -d --name kibana --link es:es-link -e ELASTICSEARCH_HOSTS=http://es-link:9200 -p 5601:5601 kibana:7.9.1

```

### 中文分词插件

```sh
# 若没有映射 plugins 目录到 container 外部， 则需要如下：

# 中文 分词插件 https://github.com/medcl/elasticsearch-analysis-ik/
docker exec -it es bash
./bin/elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v6.3.0/elasticsearch-analysis-ik-6.3.0.zip # replace 6.3.0
docker restart es

# 若映射了 plugins 目录：
# 先到 release 页面下载 zip 文件
# 然后解压到 plugins 目录, 文件夹命名不要修改
# 重启 es


# 自定义拓展词库
# 修改插件包下的 config/IKAnalyzer.cgf.xml
# 使用 nginx 托管 ik_fenci.txt 文件
```

### 使用

#### meta info api

```sh
# 基本信息 , 可以带 ?v 表示带表头
/_cat/nodes  # 所有节点
curl -X GET 'http://localhost:9200/_cat/health'   # 健康信息 
curl -X GET 'http://localhost:9200/_cat/master'   # 主节点
curl -X GET 'http://localhost:9200/_cat/indices'  # 索引信息


```

#### index api

```sh

# 新建 Index, 
# 名为 weather
curl -X PUT 'localhost:9200/weather'
# {"acknowledged":true,"shards_acknowledged":true,"index":"weather"}% # 成功

# 删除 index
curl -X DELETE 'localhost:9200/weather'

# 查看当前节点所有 index
# v 为 verbose 详细信息
curl -X GET 'http://localhost:9200/_cat/indices?v'
# health status index   uuid                   pri rep docs.count docs.deleted store.size pri.store.size
# yellow open   weather 11BWkxZDSHicWIcg8UMF1A   1   1          0            0       208b           208b

# 查看单一 index 情况
curl -X GET 'http://localhost:9200/_cat/indices/weather?v'


```

#### document api

```sh
# 新建 index, document 
# 也可更新 (不是增量更新, 必须指定全部的数据), 必须指定 ID, 这个 ID (_id) 和 json 中的 id 不同
PUT /weather/_doc/2
{
  "name": "aa"
}

# 新建 index, document, 
# 不可更新
# 必须指定 path 中的 id
PUT /people/_create/1
{
  "name": "bb"
}

# 新建
# 带 path id 为 修改/新建, 不带则一定是新建
POST /account/_doc  # 不支持 _create
{
  "name": "act"
}

# 查询
curl -X GET "localhost:9200/customer/_doc/1?pretty"


# 乐观锁 ?if_seq_no=xxx&if_primary_term=1


# 更新 (增量更新, 不必指定全量数据)
POST /users/_update/1
{
  "doc": {
    "name": "updated name"
  }
}

# 删除
DELETE /users/_doc/1



# 批量 bulk



```

#### mapping api

```sh

# specific a mapping while create an index
# 可以直接插入 doc, 不创建 mapping, 这样 es 会默认根据 doc 字段类型创建 mapping

# 验证分词, 最新版不支持 type 了 (考虑 去掉 person {...})
#
# 首先新建一个名称为accounts的 Index，里面有一个名称为person的 Type。person有三个字段
# analyzer是字段文本的分词器，search_analyzer是搜索词的分词器。ik_max_word分词器是插件ik提供的
curl -X PUT 'localhost:9200/accounts' -H 'Content-Type:application/json' -d '
{
  "mappings": {
    "person": {
      "properties": {
        "user": {
          "type": "text",
          "analyzer": "ik_max_word",
          "search_analyzer": "ik_max_word"
          // 相关性得分, 默认 1.0
          //"boost": 1.0
        },
        "title": {
          "type": "text",
          "analyzer": "ik_max_word",
          "search_analyzer": "ik_max_word"
        },
        "desc": {
          "type": "text",
          "analyzer": "ik_max_word",
          "search_analyzer": "ik_max_word"
        }
      }
    }
  }
}'
# 最新版这样创建 index 和分词约束: 分步
curl -XPUT http://localhost:9200/accounts
curl -X PUT 'localhost:9200/accounts/_mapping' -H 'Content-Type:application/json' -d '
{
    "properties": {
        "user": {
            "type": "text",
            "analyzer": "ik_max_word",
            "search_analyzer": "ik_max_word"
        },
        "title": {
            "type": "text",
            "analyzer": "ik_max_word",
            "search_analyzer": "ik_max_word"
        },
        "desc": {
            "type": "text",
            "analyzer": "ik_max_word",
            "search_analyzer": "ik_max_word"
        }
    }
}'

# 插入
# 也可以用 put 但是 url 必须要有 id， 一般用于修改
curl -XPOST http://localhost:9200/accounts/_create/1 -H 'Content-Type:application/json' -d '
{
  "user": "张三",
  "title": "工程师",
  "desc": "数据库管理"
}'
curl -XPOST http://localhost:9200/accounts/_create/2 -H 'Content-Type:application/json' -d '
{
  "user": "李四",
  "title": "运维程序员",
  "desc": "开发"
}'

# 查看 mapping
GET /users/_mapping

```

#### search api

两种方式

- get 请求 + url 参数
- get 请求+body 参数


方式 1: 

```sh

# search specific index
GET /users/_search 
GET /users/_search?q=* # 同上
GET /users/_search?q=*&sort=id:asc # 排序


# search for multiple index
GET /users,account/_search # 不带空格



```

方式 2: 

```sh

# search all data
curl -XGET "http://es-link:9200/_search" -H 'Content-Type: application/json' -d'{  "query": {    "match_all": {}  }}'

# 排序
GET /users/_search
{
  "query": {
    "match_all": {}
  }, 
  "sort": [
    {
      "id": {
        "order": "desc"
      }
    }
  ]
}


```

#### 数据类型

https://www.cnblogs.com/betterwgo/p/11571275.html

- nested , 防止 数组内的对象扁平化存储, 检索时出 bug

### Java client



https://github.com/spring-projects/spring-data-elasticsearch 版本兼容问题

所以推荐 https://www.elastic.co/guide/en/elasticsearch/client/java-rest/7.9/index.html

## logstash

收集日志, 跑在 jvm 中, 

## filebeat

因为logstash是jvm跑的，资源消耗比较大，所以后来作者又用golang写了一个, 功能较少但是资源消耗也小

logstash 和filebeat都具有日志收集功能，filebeat更轻量，占用资源更少，但logstash 具有filter功能，能过滤分析日志。一般结构都是filebeat采集日志，然后发送到消息队列，redis，kafaka。然后logstash去获取，利用filter功能过滤分析，然后存储到elasticsearch中

## kibana