---
title: saas-software-as-a-service
tags:
---


https://github.com/zuihou/lamp-cloud
TODO


<!-- more -->

# 概念

一种通过 Internet 提供软件的模式，厂商将应用软件统一部署在自己的服务器上，客户可以根据自己实际需求，通过互联网向厂商定购所需的应用软件服务，按定购的服务多少和时间长短向厂商支付费用

related roles:

- saas service provider
- tenant: the company that have purchased the service, 每个租户的数据隔离, 系统独立
- user: employee of tenant

# 设计

## 数据隔离方案

- 独立 db for every single tanent,
  安全性最好, 数据恢复最容易，但成本较高
- 共享 db, 但每个 tanent 一个 schema,
  数据恢复比较困难，因为恢复数据库将牵涉到其他租户的数据 如果需要跨租户统计数据，存在一定困难
- 租户共享同一个数据库、同一个 Schema, 表中增加 TenantID 多租户的数据字段;
  维护和购置成本最低, 数据备份和恢复最困难，需要逐表逐条备份和还原

# springboot 动态数据源切换实现 saas

