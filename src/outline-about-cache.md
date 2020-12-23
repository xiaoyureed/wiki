---
title: 缓存概述
date: 2017-08-17 14:51:45
tags: [cache, outline]
categories: java web
---

<div align="center">
ehchach, memcache, redis...
https://www.tianmaying.com/tutorial/spring-web-ehcache
web缓存体系: http://clsn.io/clsn/lx1088.html
https://sourcegraph.com/github.com/qiujiayu/AutoLoadCache 缓存框架
</div>
<!--more-->

<!-- TOC -->

- [为什么要使用缓存](#为什么要使用缓存)
- [缓存的特征](#缓存的特征)
  - [命中率](#命中率)
  - [最大元素（最大空间）](#最大元素最大空间)
  - [清空策略](#清空策略)
  - [缓存介质](#缓存介质)
- [分类 应用场景](#分类-应用场景)
  - [本地缓存（local cache）](#本地缓存local-cache)
    - [编程直接实现缓存](#编程直接实现缓存)
      - [成员变量or局部变量实现](#成员变量or局部变量实现)
      - [静态变量实现](#静态变量实现)
    - [Ehcache](#ehcache)
    - [Guava Cache](#guava-cache)
  - [分布式缓存（remote cache）](#分布式缓存remote-cache)
    - [memcache缓存](#memcache缓存)
    - [Redis缓存](#redis缓存)
  - [缓存方案选型](#缓存方案选型)
- [缓存更新的策略](#缓存更新的策略)
- [缓存带来的问题](#缓存带来的问题)
  - [缓存一致性问题](#缓存一致性问题)
    - [指定过期时间](#指定过期时间)
    - [删除缓存](#删除缓存)
    - [主动更新缓存](#主动更新缓存)
    - [使用 canal 中间件](#使用-canal-中间件)
  - [雪崩问题](#雪崩问题)
  - [缓存穿透](#缓存穿透)
  - [缓存击穿问题](#缓存击穿问题)
  - [缓存并发竞争问题](#缓存并发竞争问题)
- [静态资源缓存-图片缓存](#静态资源缓存-图片缓存)

<!-- /TOC -->

# 为什么要使用缓存

高性能(请求执行耗时更短)

高并发(1s内可以接收更多请求)


# 缓存的特征

## 命中率

命中率=返回正确结果数/请求缓存次数，命中率越高，表明缓存的使用率越高。

## 最大元素（最大空间）

缓存中可以存放的最大元素的数量，一旦超过，那么将会触发缓存启动清空策略

## 清空策略

缓存的存储空间有限制，当缓存空间被用满时，如何保证在稳定服务的同时有效提升命中率？这就由缓存清空策略来处理

* FIFO(first in first out)

    先进先出；满足数据实效性，优先保证最新数据可用

* LFU(less frequently used)

    最少使用；无论是否过期，清除使用次数较少的元素，缓存的元素有一个hit (命中次数)属性，hit 值最小的将会被清出缓存。，满足高频数据有效性

* LRU(least recently used)ehcache 默认值

    最近最少使用；无论是否过期，缓存的元素有一个时间戳，当缓存容量满了，而又需要腾出地方来缓存新的元素的时候，那么现有缓存元素中时间戳离当前时间最远的元素将被清出缓存，优先保证热点数据有效性  

此外还有一些：  
根据过期时间判断，清理过期时间最长的元素；  
根据过期时间判断，清理最近要过期的元素；  
随机清理；  
根据关键字（或元素内容）长短清理等。  

## 缓存介质

内存、硬盘(文件系统)、数据库（redis等nosql数据库）


# 分类 应用场景

根据缓存与应用的耦合度可分为local Cache、remote Cache  

* local cache

直接存在于应用中的缓存组件；  
优点：直接与应用存在于同一个进程，请求缓存速度快，没有过多的网络开销  
缺点：由于与应用耦合，多个程序无法共享缓存；每个程序需要维护自己的单独缓存，浪费内存  
适用于： 单应用不需要集群，或集群下各节点不许互相通信

* remote cache

与应用分离的缓存组件；
优点：与程序解耦，多个程序可共享缓存  
缺点： 网络开销

根据缓存位置不同可以分为客户端缓存和服务器缓存

* 客户端缓存（缓存在用户的客户端，例如浏览器中）
* 服务器缓存（缓存在服务器中，可以缓存在内存中，也可以缓存在文件里，并且还可以进一步地区分为本地缓存和分布式缓存两种）

## 本地缓存（local cache）

### 编程直接实现缓存

例如使用hashmap存储键值对.

#### 成员变量or局部变量实现

```java
 public void UseLocalCache(){
     //一个本地的缓存变量,只作用域类内部，类间无法共享缓存
     Map<String, Object> localCacheStoreMap = new HashMap<String, Object>();

    List<Object> infosList = this.getInfoList();
    for(Object item:infosList){
        if(localCacheStoreMap.containsKey(item)){ //缓存命中 使用缓存数据
            // todo
        } else { // 缓存未命中  IO获取数据，结果存入缓存
            Object valueObject = this.getInfoFromDB();
            localCacheStoreMap.put(valueObject.toString(), valueObject);

        }
    }
}
//示例
private List<Object> getInfoList(){
    return new ArrayList<Object>();
}
//示例数据库IO获取
private Object getInfoFromDB(){
    return new Object();
}
```

#### 静态变量实现

```java 
//O2O业务中常用的城市基础基本信息判断，通过静态变量一次获取缓存内存中，减少频繁的I/O读取，静态变量实现类间可共享，进程内可共享，缓存的实时性稍差
public class CityUtils {
      private static final HttpClient httpClient = ServerHolder.createClientWithPool(); 
      private static Map<Integer, String> cityIdNameMap = new HashMap<Integer, String>();
      private static Map<Integer, String> districtIdNameMap = new HashMap<Integer, String>();

  static {
    HttpGet get = new HttpGet("http://gis-in.sankuai.com/api/location/city/all");
    BaseAuthorizationUtils.generateAuthAndDateHeader(get,
            BaseAuthorizationUtils.CLIENT_TO_REQUEST_MDC,
            BaseAuthorizationUtils.SECRET_TO_REQUEST_MDC);
    try {
        String resultStr = httpClient.execute(get, new BasicResponseHandler());
        JSONObject resultJo = new JSONObject(resultStr);
        JSONArray dataJa = resultJo.getJSONArray("data");
        for (int i = 0; i < dataJa.length(); i++) {
            JSONObject itemJo = dataJa.getJSONObject(i);
            cityIdNameMap.put(itemJo.getInt("id"), itemJo.getString("name"));
        }
    } catch (Exception e) {
        throw new RuntimeException("Init City List Error!", e);
    }
}
    static {
    HttpGet get = new HttpGet("http://gis-in.sankuai.com/api/location/district/all");
    BaseAuthorizationUtils.generateAuthAndDateHeader(get,
            BaseAuthorizationUtils.CLIENT_TO_REQUEST_MDC,
            BaseAuthorizationUtils.SECRET_TO_REQUEST_MDC);
    try {
        String resultStr = httpClient.execute(get, new BasicResponseHandler());
        JSONObject resultJo = new JSONObject(resultStr);
        JSONArray dataJa = resultJo.getJSONArray("data");
        for (int i = 0; i < dataJa.length(); i++) {
            JSONObject itemJo = dataJa.getJSONObject(i);
            districtIdNameMap.put(itemJo.getInt("id"), itemJo.getString("name"));
        }
    } catch (Exception e) {
        throw new RuntimeException("Init District List Error!", e);
    }
}

    public static String getCityName(int cityId) {
      String name = cityIdNameMap.get(cityId);
      if (name == null) {
        name = "未知";
      }
       return name;
     }

    public static String getDistrictName(int districtId) {
      String name = districtIdNameMap.get(districtId);
       if (name == null) {
         name = "未知";
        }
       return name;
     }
   }
```

静态变量实现类间可共享，进程内可共享，缓存的实时性稍差  
为了解决本地缓存数据的实时性问题，目前大量使用的是结合ZooKeeper的自动发现机制，实时变更本地静态变量缓存  

> 这类缓存实现，优点是能直接在heap区内读写，最快也最方便；缺点同样是受heap区域影响，缓存的数据量非常有限，同时缓存时间受GC影响。主要满足单机场景下的小数据量缓存需求，同时对缓存数据的变更无需太敏感感知，如上一般配置管理、基础静态数据等场景。

### Ehcache

具有如下特点：

* 纯Java开源缓存框架

* 一个非常轻量级的缓存实现, 只依赖slf4j, Hibernate就使用了他

* 两级缓存介质，cache.flush()才能确保数据不会丢失

* 快速，针对大型高并发系统场景，Ehcache的多线程机制有相应的优化改善。

* 具有缓存(CacheEvenListener)和缓存管理器(CacheManagerListener)的侦听接口，能更简单方便的进行缓存实例的监控管理(做一些统计或数据一致性广播挺好用的)

* 支持多缓存管理器实例，以及一个实例的多个缓存区域。


核心定义：  

* cache manager：缓存管理器

* cache： 缓存管理器中可放置若干

* element： 单条缓存数据的组成单位

* system of record（SOR）：可以取到真实数据的组件，可以是真正的业务逻辑、外部接口调用、存放真实数据的数据库等，缓存就是从SOR中读取或者写入到SOR中去的。


```xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache>
    <!-- 
        磁盘存储:将缓存中暂时不使用的对象,转移到硬盘,类似于Windows系统的虚拟内存
        path:指定在硬盘上存储对象的路径
     -->    
    <diskStore path="d:/ehcache/"></diskStore>
    
    <!-- 默认缓存配置 -->
    <!-- 
    必须:
        * defaultCache:默认的缓存配置信息,如果不加特殊说明,则所有对象按照此配置项处理
        * maxElementsInMemory:设置了缓存的上限,最多存储多少个记录对象
        * eternal:代表对象是否永不过期(如果为true，则缓存的数据始终有效，如果为false那么还要根据timeToIdleSeconds，timeToLiveSeconds判断。)
        * overflowToDisk:如果内存中数据超过内存限制，是否要缓存到磁盘上
    可选:
        * maxElementsOnDisk：在磁盘上缓存的element的最大数目, 达到后old data会刷到磁盘，默认值为0，表示不限制
        * timeToIdleSeconds:最大的发呆时间(多长时间没有被访问就会失效), 默认0, 永不失效
        * timeToLiveSeconds:最大的存活时间, 默认0, 同上
        * diskSpoolBufferSizeMB： DiskStore使用的磁盘大小，默认值30MB。每个cache使用各自的DiskStore
        * memoryStoreEvictionPolicy： 如果内存中数据超过内存限制，向磁盘缓存时的策略。默认值LRU，可选FIFO、LFU
     -->
    <defaultCache
        maxElementsInMemory="10000"
        eternal="false"
        timeToIdleSeconds="120"
        timeToLiveSeconds="120"
        overflowToDisk="true"
    />
    
    <!-- User缓存配置 -->
    <!-- 
        cache:为指定名称的对象进行缓存的特殊配置
        name:指定对象的完整名(如果有别名亦可    )
     -->
    <cache 
        name="User" 
        maxElementsInMemory="10000" 
        eternal="false"
        timeToIdleSeconds="300" 
        timeToLiveSeconds="600" 
        overflowToDisk="true" 
    />

</ehcache>
```

### Guava Cache

```sh
# waiting
```

## 分布式缓存（remote cache）

### memcache缓存

[ref](https://blog.csdn.net/jationxiaozi/article/details/8509732)

### Redis缓存

[ref](https://blog.csdn.net/jationxiaozi/article/details/8509732)


## 缓存方案选型

[ref](https://www.zhihu.com/question/24504292/answer/86908460), [ref](https://www.zhihu.com/question/19645807/answer/12605611), [更多memcached和redis区别](https://www.zhihu.com/question/19829601)

[memcache 和 redis 区别](https://www.imooc.com/article/23549)

* ehcache直接在jvm虚拟机中缓存，速度快, 但是缓存共享麻烦, 大型分布式系统不选用

* redis 数据类型多 (String list，set，zset(sort set)，hash), meme 仅仅支持 string/二进制

* redis 支持持久化 (rdb快照, aof日志), binlog, mem 不支持持久化

* redis 原生支持集群部署, mem 支持不好 (需要通过客户端来实现往集群中分片写入数据)

* Redis使用单线程的IO复用模型。Memcached是多线程, 可以高效利用多核，非阻塞IO复用的网络模型.

    redis是单线程模型, 只使用cpu单核, mem 可使用多核, 所以在缓存小数据的时候, redis更快(没有线程切换,资源竞争), 缓存100k以上的大数据时mem更快


# 缓存更新的策略

[ref](https://coolshell.cn/articles/17416.html)


# 缓存带来的问题

常用三连: 查数据库枷锁防止缓存击穿, 缓存空对象/布隆过滤器防止穿透, 过期时间加随机值/逻辑过期删除防止雪崩

## 缓存一致性问题

一致性问题: 写库操作后, 数据库和缓存数据不一致了

有如下几种解决方案, 开发成本依次升高:

### 指定过期时间

缓存指定过期时间 , 容许某段时间的数据不一致

### 删除缓存

先写 db, 成功后, 再删除缓存/让缓存失效

但是也有问题, 问题描述: 写库时, 先更新数据库, 再删除缓存, 如果删除缓存失败了, 那数据库和缓存数据就不一致了 

- 解决: 利用消息中间件对缓存删除进行失败重试

- 解决: 改为先删除缓存然后更新数据库, 这样如果数据库更新失败, 数据库中是旧数据, 缓存中是空的, 数据不会不一致;

    但是在高并发下, 还是有可能问题: 先删除缓存, 正准备更新数据库, 这时又有一个查询请求进来了, 没有缓存, 查询数据库, 得到旧数据并写入缓存结束返回, 这时之前那个更新请求更新数据库完成, 缓存和数据库数据不一致了

    解决: 简单点的: "延时重复删除" - 在更新数据库前后各删除一次缓存, 第二次删除前等待一段时间(比如200ms); 第二次删除是为了删除写库前有读库操作造成缓存脏数据, 这段延时就是一个读库操作的耗时, 目的是为了让脏数据完全写入缓存, 以便我们刚好能够删除; (休眠和第二次删除可另起一个线程,这样可以提高qps)

- 解决: 还可以 利用内存queue , 将读请求/写请求 串行化, 对同一个 id 的读写串行化, 写库完成前, 无法读库 (严格的一致性): 创建几个jvm内存queue, 比如20个， 每个queue对应一个worker线程; 所有请求均匀分发给各个 queue, 比如按照 id 取模, 然后让对应的线程处理

    这样的话, 更新数据时根据唯一标识(比如商品id),得到hashcode,然后对queue个数取模, 均匀的放入queue中; 
        
    读取数据时，如果发现数据不再缓存中， 将进行【重新读取数据+添加缓存】的操作，根据唯一标识hash路由后加入同一个jvm内存队列；这样worker串行拿到对应操作，然后依次执行，这样的话，一个更新数据的操作，先删除缓存， 然后更新数据库， 还没有完成的话，后面的读请求会在queue中积压， 等待数据库更新操作完成；

    这里有个优化点：一个队列中，其实对同个id的多个更新缓存请求串在一起是没意义的，因此可以做过滤，如果发现队列中已经有一个更新缓存的请求了，那么就不用再放个更新请求操作进去了，直接等待前面的更新操作请求完成即可

    如果请求还在等待时间范围内，不断轮询发现可以取到值了，那么就直接返回；如果请求等待的时间超过一定时长，那么这一次直接从数据库中读取当前的旧值。


- 还可以 : 利用MySQL的binlog异步更新数据到缓存; 推荐



还有一个出现几率很小的问题:

> 比如，一个是读操作，但是没有命中缓存，然后就到数据库中取数据，此时来了一个写操作，写完数据库后，让缓存失效，然后，之前的那个读操作再把老的数据放进去，所以，会造成脏数据
> 顺序是这样的: thread 1读库 (缓存没有) -> thread2写库 -> thread2让缓存失效 -> thread1 设置缓存
> 因为这种情况要求一个完整的写库操作在一个读库操作周期内完成, 这显然不大可能, 因为写库大概率慢于读库

### 主动更新缓存

数据库更新后, 缓存立即更新, 适用于对数据一致性要求高的场景, 但是开发成本最高, 需要我们自己完成缓存的更新

一致性最好, 但是如果主动更新出现问题, 会出现不一致问题, 而且可能很久都不会再更新缓存, 所以`一般结合超时剔除` (最佳实践)


可能的问题: 如果对缓存的操作是更新而不是删除, 那么高并发下, 两个并发的写库操作会导致不一致, 一个可能的请求执行顺序是: thread1更新缓存, thread2更新缓存, thread2更新db, thread1更新db. 此时 缓存和db数据不一致了

这时因为 在高并发下, 更新db 和 更新缓存不是原子操作 (通过加锁可以避免)

解决: 删除缓存, 而不是更新缓存 

也有懒加载的思想; 还有这个原因: 缓存数据可能是通过负载计算后得出的, 更新缓存太复杂, 还是直接删除简单方便


### 使用 canal 中间件

阿里开源, 订阅 MySQL 的 binglog来更新缓存, 业务代码中就无需关注缓存一致性相关的问题了, 直接把canal 当成 MySQL 的从服务器, 查询即可

此外 canal 还能解决数据异构问题 (使用 canal 订阅 多个数据源, 聚合数据, 进行分析处理)


## 雪崩问题

所有请求瞬间越过缓存直接奔向数据库, 导致数据库也挂, 可能原因:

- 缓存层挂了

    - 缓存层高可用 - 多个缓存节点(redis 集群, 主从 + 哨兵)

    - 使用依赖隔离组件为后端限流如队列, 降级如 Hystrix

    - redis持久化，快速恢复缓存数据

- 项目刚上线, redis 中没有任何数据, 瞬间大量请求过来, 造成雪崩

    - 缓存预热 - 上线前, 预先将热点数据放入redis

- 缓存大量的 key 采用了相同的过期时间, 某一时刻集中失效

    - 为每个key 的过期时间加上一个随机数字

    - 使用 "逻辑过期" 策略, 即设置key永不过期, 而将业务要求的过期时间存储到value (到期时间: now() + expire), 每次读取value, 都评估一下是否过期 (到期时间 == now() ?), 如果过期, 则手动删除key(这样, 永远不会查不到key)



## 缓存穿透

查询一条不存在的空记录, 每次请求都会穿透缓存层到达数据库 (并且查不到数据,也没有写入缓存), 失去了 "缓存保护后端数据库" 的意义

造成这个问题的原因:

- 业务代码本身的问题
- 恶意攻击, 爬虫... 造成的大量空命中

怎么解决:

- 对于恶意请求, 预先过滤 , 如限制频率

- 缓存空对象 - 当第一次穿透缓存到达数据库, 查询为空, 就将这个空对象缓存到 redis, 之后针对这条数据的访问就不会穿透缓存而是直接返回空对象

    这种方法占用更多的 redis 存储空间, 需要为 null 缓存对象设置较短的过期时间

    数据一致性问题, 可利用消息队列清除 null 对象

- 布隆过滤器拦截 - 适用于 数据命中不高, 数据变化不大的场景 //todo

## 缓存击穿问题

大量请求同时访问某个 key (高频热点 key), 而这个 key 正好到了失效时间, 请求 直接打到 db

访问数据库时加锁, 大量请求只让一个去  db, 查到后存入缓存, 其他请求等待直到获取锁 先去 Redis 查, 到后直接返回, 伪代码:

```java
resp = queryRedis(key)
if (resp == null) {
    synchronized(this) {
        resp = queryRedis(key)
        if (resp == null) {
            resp = queryDB(key)
            setRedis(key)
        }
    }
}
return resp;
```

另一个思路是对即将过期的数据主动刷新，做法可以有很多，比如起一个线程轮询数据

## 缓存并发竞争问题

多个 client 对某个 key 操作, 如果:

- 对这个 key 的操作不要求顺序: 使用分布式锁, 抢到锁的client就进行操作

- 对这个 key 的操作要求顺序: 在 set value 时, value 中包含一个时间戳(标识顺序), 每个client 在 set 时要保证自己的时间晚于该时间戳


# 静态资源缓存-图片缓存

图片这些是静态资源吧，静态资源推荐的话是使用CDN缓存或者Nginx本地代理缓存和控制客户端浏览器的缓存时常以此来达到一个缓存的作用。



