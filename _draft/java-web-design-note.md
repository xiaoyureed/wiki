---
title: java-web-design-note
tags: [java, web, design]
categories: java web
---

https://my.oschina.net/huangyong/blog/158380

<!--more-->

<!-- TOC -->

- [Action](#action)
- [Action分发机制](#action分发机制)
- [entity实体](#entity实体)
- [技术选型](#技术选型)
- [Java热插拔](#java热插拔)

<!-- /TOC -->

# Action

类比 controller

# Action分发机制

依靠唯一的 Servlet : DispatcherServlet

1. 从 ServletRequest 中得到 request url, 遍历路由库找到对应的 Action (包括 Action 类与 Action 方法)

1. 解析 url 中的占位符, 得到 Action 方法参数

1. 反射创建 Action, 执行对应方法, 得到返回值 (进一步转为 json/xml, 根据注解判断)

# entity实体

- 定义一个顶级 Bean 类 实现 序列化接口, 提供 hashcode, equals, tostring, toJson, toXml 方法, 使用了 Apache Commons 的  lang 包来实现; (无论是否 entity 类, 都可继承这个类复用); 

- 执行 SQL 语句，将 ResultSet 映射为 Map(尤其是执行多表连接的时候，查询出来的列来自于不同的表，而且数量很大的情况下，就没有必要再写一个 Entity 去映射了，直接拿到一个 Map，最后将其转为 JSON 给前端)

# 技术选型

JUint：用于单元测试。
Apache Commons：用于提供最基础的支持，例如：Lang、Collections、BeanUtils、DBCP、DbUtils 等。
Log4J：用于记录日志。
Jackson：用于处理 JSON 数据。
CGLib：用于动态代理。

# Java热插拔

https://blog.csdn.net/tonyhuang_google_com/article/details/49702291 通过 Java spi
 https://blog.csdn.net/javierhui111/article/details/3830833 通过 osgi
 https://segmentfault.com/q/1010000005736656 通过spring依赖注入
