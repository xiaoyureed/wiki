---
title: Postgres note
tags:
  - db
  - postgres
date: 2020-01-05 17:10:13
categories: java web
---

http://www.postgresqltutorial.com/# - postgresql tutorial

https://medium.com/quick-code/top-tutorials-to-learn-postgresql-database-for-beginners-99ff0deb9f84

http://www.ruanyifeng.com/blog/2013/12/getting_started_with_postgresql.html - 通俗的入门

https://github.com/PostgREST/postgrest - postgresql to rest api



<!--more-->

<!-- TOC -->

- [what is postgres](#what-is-postgres)
- [Why Postgres](#why-postgres)
- [how to use](#how-to-use)
    - [sql 示例](#sql-示例)
    - [数据类型](#数据类型)
        - [时间](#时间)
        - [array类型](#array类型)
        - [hstore 键值对类型](#hstore-键值对类型)
        - [json类型](#json类型)
        - [数字类型](#数字类型)
        - [序号类型](#序号类型)
        - [字符类型](#字符类型)
    - [不同于MySQL的特性](#不同于mysql的特性)
        - [账号系统](#账号系统)
        - [schema](#schema)
        - [sql查询](#sql查询)
        - [数据修改](#数据修改)
        - [表管理](#表管理)
        - [导入导出](#导入导出)
- [crontab 定时操作 postgresql](#crontab-定时操作-postgresql)
- [docker 中启动 postgres](#docker-中启动-postgres)

<!-- /TOC -->


# what is postgres

关系型数据库, 开源, 同类是 MySQL

Greenplum: 开源分布式数据库集群技术，基于PostgreSQL

# Why Postgres

或者说, 比MySQL好在哪里 (https://www.zhihu.com/question/20010554/answer/62628256)

- MySQL 的各种 text 字段有不同的限制, 要手动区分 small text, middle text, large text... Pg 没有这个限制, text 能支持各种大小.
- pg 可以设置 transform_null_equals 把 = null 翻译成 is null (按照 SQL 标准, 做 null 判断不能用 = null, 只能用 is null, 这是因为 null永远不等于null) - http://www.postgresqltutorial.com/postgresql-is-null/
-  MySQL 里需要 utf8mb4 才能显示 emoji 的坑, Pg 就没这个坑.
- MySQL 的事务隔离级别 repeatable read 并不能阻止常见的并发更新, 得加锁才可以, 但悲观锁会影响性能, 手动实现乐观锁又复杂. 而 Pg 的列里有`隐藏的乐观锁` version 字段, 默认的 repeatable read 级别就能保证并发更新的正确性, 并且又有乐观锁的性能
- MySQL 不支持多个表从同一个序列中取 id, 而 Pg 可以
- MySQL 不支持 OVER 子句, 而 Pg 支持. OVER 子句能简单的解决 "每组取 top 5" 的这类问题
- 子查询 (subquery) 性能都比 MySQL 好.
- 存储 array 和 json, 可以在 array 和 json 上建索引,  jsonb 的存储结构用来实现文档数据库存储, 不必引入mongodb了
- 自带全文检索, 不必引入 elasticsearch
- MySQL 处理树状回复的设计会很复杂, 而且需要写很多代码, 而 Pg 可以高效处理树结构
- postgres 打通所有类型的数据源, 使用sql语法去查询控制，像使用本地表一样. 这就不必引入 ETL了 (postgresql有丰富的fdw拓展插件，能打通sql server, sybase, oracle ,mysql，甚至能打通nosql 如redis, mongodb，还能打通各种文件比如日志文件)
- Postgresql 的PL/Python过程语言允许函数用Python语言编写

# how to use

## sql 示例

```sql
create table product
(
	id serial not null
		constraint product_pk
			primary key,
	name text default '' not null,
	price decimal default 0 not null,
	create_date timestamp default now() not null
);

comment on table product is 'product';

comment on column product.name is 'product name';

comment on column product.price is 'product price
';

comment on column product.create_date is 'create date';


```

## 数据类型

http://www.postgres.cn/docs/11/datatype-numeric.html

### 时间

- date 日期
- time [without time zone] 时间
- timestamp [without time zone], 日期时间, 无时区
- time/timestamp with time zone 有时区 or timestamptz
- interval 时间间隔

time、timestamp和interval接受一个可选的精度值 p，这个精度值声明在秒域中小数点之后保留的位数。缺省情况下，在精度上没有明确的边界。p允许的范围是从 0 到 6

时间输入输出最好遵循 ISO 8601, ISO 8601指定使用大写字母T来分隔日期和时间。PostgreSQL在输入上接受这种格式，但是在输出时它采用一个空格而不是T

### array类型

```sql
CREATE TABLE contacts (
id serial PRIMARY KEY,
name VARCHAR (100),
phones TEXT []
);

INSERT INTO contacts (name, phones)
VALUES
(
'John Doe',
ARRAY [ '(408)-589-5846',
'(408)-589-5555' ]
);

-- 一次插入两行
INSERT INTO contacts (name, phones)
VALUES
(
'Lily Bush',
'{"(408)-589-5841"}' -- 单引号包裹中括号
),
(
'William Gate',
'{"(408)-589-5842","(408)-589-58423"}'
);

SELECT
name,
phones [ 1 ]
FROM
contacts;

SELECT
name,
phones
FROM
contacts
WHERE
'(408)-589-5555' = ANY (phones);


```

### hstore 键值对类型

, ref http://www.postgresqltutorial.com/postgresql-hstore/

### json类型

```sql
CREATE TABLE orders (
ID serial NOT NULL PRIMARY KEY,
info json NOT NULL
);

INSERT INTO orders (info)
VALUES
(
'{ "customer": "Lily Bush", "items": {"product": "Diaper","qty": 24}}'
),
(
'{ "customer": "Josh William", "items": {"product": "Toy Car","qty": 1}}'
),
(
'{ "customer": "Mary Clark", "items": {"product": "Toy Train","qty": 2}}'
);

SELECT
info
FROM
orders;

SELECT
info -> 'customer' AS customer -- 获取json中某个key的值, 以json格式返回, 可以继续加箭头
FROM
orders;

SELECT
info ->> 'customer' AS customer -- 以字符串格式返回
FROM
orders;

SELECT
info -> 'items' ->> 'product' as product
FROM
orders
ORDER BY
product;

```

### 数字类型

- 整型: 常用的类型是integer/int，因为它提供了在范围、存储空间和性能之间的最佳平衡。一般只有在磁盘空间紧张的时候才使用 smallint类型。而只有在integer的范围不够的时候才使用bigint

    int2、int4和int8 是 smallint, integer, bigint 简写

- 小数: decimal和numeric是等效的, 

    一个numeric的precision（精度）是整个数中有效位的总数, numeric的scale（刻度）是小数部分的数字位数, `NUMERIC(precision, scale)`

### 序号类型

smallserial、serial和bigserial类型不是真正的类型，它们只是为了创建唯一标识符列而存在的方便符号

也有类似 int 的简写

```sql
CREATE TABLE tablename (
    colname SERIAL
);

等价于
CREATE SEQUENCE tablename_colname_seq;
CREATE TABLE tablename (
    colname integer NOT NULL DEFAULT nextval('tablename_colname_seq')
);
ALTER SEQUENCE tablename_colname_seq OWNED BY tablename.colname;

```

### 字符类型

- character varying(n) 变长 ,有限制 最多存储 n 个字符

- character(n) 定长, 空格填充

    没有长度声明词的character等效于character(1)

- text 无限变长

## 不同于MySQL的特性

### 账号系统

```bash
# 默认提供 db account: postgres/none, Linux account: postgres
su postgres # 切换user
psql # 登陆

```

```sql
\password postgres; -- 为postgres设置一个密码
create user dbuser with password 'dbuser'; -- 默认只有登陆权限
create database demodb owner dbuser;
grant all privileges on database demodb to dbuser; -- 赋予操作权限
\q -- 退出psql

```

设置免密登陆

```bash
# 退出后再次登陆
# psql [options] [dbname [username]]
psql --host=localhost --port=5432 --dbname=demodb --username=tiger --password=tiger # 报错, 不允许指定密码, 需要手动输入
# 怎么自动密码登陆呢? 定时任务可能会用到
# 方法1: 在Linux中创建和db用户同名的账户, 在shell中, psql <dbname> 即可
#           要导入数据: psql exampledb < exampledb.sql
# 方法2: 设置环境变量 export PGPASSWORD="password"
# 方法3: 在 home dir 中创建.pgpass文件, 写入 localhost:5432:mydbname:postgres:mypass, 文件权限为 600,
#       然后在 shell中 psql ... -w 即可

# 执行sql
psql -f <Sql_file> ...
psql -c "sql query" ...

```

准备数据

```bash
# 
docker cp <host_sql_location> <container>:<container_location> # 复制到docker
psql -c "create database dvdrental owner postgres"
psql -c "grant all privileges on database dvdrental to postgres"
pg_restore -U tiger -d demodb <xxx.tar>

```

### schema

```sql
create schema public; -- schema 把一个数据库分为了多个区域, 各个区域中可以存在同名的表
          -- schema public 是默认存在的, 创建表如果不指定 schema 则默认是 public
          -- 一般开发时不会写死 schema, 即不会指定schema, 通过 search_path 设置
drop schema public [cascade]; -- 删除空schema(前置删除)

revoke create on <schema_name> from public; -- 为schema赋予create权限, public表示所有用户
                -- 权限有多种: create(创建表)/usage(使用表)/...

show search_path ; -- 查看模式的搜索路径
-- 默认是这样的:
 search_path
--------------
 $user,public
 
-- 这表示, sql 中没有指定schema, 会这么确定schema: 首先查看是否有和当前用户同名的schema, 如果有则使用; 如果没有则使用默认的 public schema

SET search_path TO myschema,public; -- 修改默认的 search_path

```

schema的最佳实践:

- 仅仅使用 默认的public, 相当于不使用 schema 这个特性; 适用于 "只有一个用户或者数据库里只有几个合作用户的情形"
- 为每个用户创建一个模式，名字和用户相同

### sql查询

select distinct on (column1, ...) column1, column2 order by ...  按照 column1 分组, 返回每组的第一行, 需要和order by一起使用 

```sql
-- 理解 distinct on 需要从 distinct 看起
-- 查询 某一字段唯一的行
select distinct column1 from t1;
-- 查询 两字段组合在一起唯一的行
select distinct column1, column2 from  t1;
-- 问题来了: 查询仅仅某一个字段唯一的行, 但是需要 select 多列
-- 这时如果仅仅使用 distinct 就有歧义了, 需要使用 distinct on
select distinct on (column1) column1, column2 from t1; -- 没有排序, 结果不定

```

limit null 等价于省略 limit

limit n offset m 返回n行, 但是偏移m行后开始计算


fetch 通过游标抓取结果, 效果类似limit, 因为limit并不是 sql 标准中的语法, 所以引入fetch

ref: http://www.postgres.cn/docs/9.4/sql-fetch.html

```sql
-- 语法 FETCH { FIRST | NEXT } [ row_count ] { ROW | ROWS } ONLY
select ... order by xxx FETCH FIRST ROW ONLY; -- 返回第一行
                    -- 等同于 FETCH FIRST 1 ROW ONLY;
FETCH FIRST 5 ROW ONLY; -- 返回前5行

-- 和 offset 合用
OFFSET 5 ROWS FETCH FIRST 5 ROW ONLY; -- 获取排序后的 6-10 行

```

like 模糊查询, 等价于 "~~" (% - 一个或多个字符; 下划线 - 一个字符)

not like 等价于 "!~~"

ilike 大小写不敏感 , 等价 "~~*"


连接符 "||"

```sql
select first_name||' '|| last_name as full_name from customer order by full_name;
```

inner join

自连接

left join, right join

full [outer] join - 返回A表中的行, 这些行在B表中没有关联的行 

cross join

natural [inner/left/right] join (默认 inner) 无需指定连接条件, 将表中具有相同名称的列自动进行匹配


union 并集

intersect 并集

except 差集

grouping sets() - group by 的子句, 效果相当于对多个 group by 结果进行 union all; 最后的结果集列数为多个group by 中的最大的列数, 合并时,列数不够的会用 null 补全;

```sql
-- 一个 grouping set 就是一个包含多个列的集合, 用这些列来进行group by
-- In this syntax, you have four grouping sets (c1,c2), (c1), (c2), and ().
SELECT
    c1,
    c2,
    aggregate_function(c3)
FROM
    table_name
GROUP BY
    GROUPING SETS (
        (c1, c2),
        (c1),
        (c2),
        ()
);
```

cube(c1, c2, c3) 用来简写 grouping sets(...), 为指定的列产生所有可能的grouping set组合

```sql
CUBE(c1,c2,c3)
 -- 等价于
GROUPING SETS (
    (c1,c2,c3),
    (c1,c2),
    (c1,c3),
    (c2,c3),
    (c1),
    (c2),
    (c3),
    ()
)
```

rollup 是group by 的子句, 也是产生 grouping set, 只是不是产生全部的可能组合, 而是只产生少部分; 多个列间有层级关系; 常用来计算年月日

```sql
-- cube 和 rollup 区别:

CUBE (c1,c2,c3)
-- 等价于
(c1, c2, c3)
(c1, c2)
(c2, c3)
(c1,c3)
(c1)
(c2)
(c3)
()

ROLLUP(c1,c2,c3)
-- 等价于
(c1, c2, c3)
(c1, c2)
(c1)
()
```

### 数据修改

update... from ... 根据其他表的条件进行更新

```sql
UPDATE A
SET A.c1 = expresion
FROM B
WHERE A.c2 = B.c2;

```

insert into ... on conflict target action 插入or更新

```sql
-- target: 列名/约束名/where子句
-- action: DO NOTHING  或者  DO UPDATE SET column_1 = value_1, .. WHERE condition

```

### 表管理

字段类型

http://www.postgresqltutorial.com/postgresql-data-types/

create table

```sql
CREATE TABLE table_name (
column_name TYPE column_constraint,
table_constraint table_constraint
) INHERITS existing_table_name; -- 继承所有字段


CREATE TABLE new_table_name [(columns...)]
AS some_query; -- 用查询到的数据填充新建的表

CREATE TABLE IF NOT EXISTS new_table_name -- 不存在才创建
AS query;

CREATE TEMPORARY/TEMP TABLE temp_table -- 临时表, 在一次会话中有效

```

### 导入导出

http://www.postgresqltutorial.com/#


# crontab 定时操作 postgresql

```sh
# crontab -e
# * * * * * /root/xy/crontab.sh
# service cron restart

# psql "host=192.168.0.123 port =5432 user = treece password =1123 dbname=amt" -f ./deal_str.sql

#!/bin/bash

step = 20 # period seconds
sql_location = '/root/xy/update.sql'
host = '10.59.97.227'
port = 5432
db_name = 'sat'
user_name = 'pimpopr'

for((i = 0; i < 60; i = (i + step)));do
        #$(echo "test-test" >> /root/xy/test.txt)
        psql "host=${host} port=${port} dbname=${db_name} user=${user} password="
        sleep ${step}
done
exit 0
```

# docker 中启动 postgres

```sh
mkdir -p $HOME/docker/volumes/postgres

docker run -d --rm --name postgresql -e POSTGRES_USER=dbuser -e POSTGRES_DB=testdb  -e POSTGRES_PASSWORD=password -p 5432:5432 -v $HOME/docker/volumes/postgres:/var/lib/postgresql/data  postgres


```

我是在 wsl2 中运行的 docker, 使用 navicat 可以正常使用 localhost 连接 .使用 inteliJ 系列, 需要 查出 wsl2 的 eth0 地址, 不能直接用 localhost 连接
