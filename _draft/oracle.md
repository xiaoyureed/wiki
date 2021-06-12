---
title: Oracle 备忘
categories: java web
---


<!--more-->

<!-- TOC -->

- [basic intro](#basic-intro)
  - [basic concept](#basic-concept)
  - [结构组成](#结构组成)
  - [11g 和 12c 区别](#11g-和-12c-区别)
  - [Oracle 在 windows 下的服务](#oracle-在-windows-下的服务)
- [how to use](#how-to-use)
  - [登录断开](#登录断开)
  - [用户 和 权限](#用户-和-权限)
  - [特有的 SQL 语法](#特有的-sql-语法)
  - [数据类型](#数据类型)
    - [number](#number)
    - [char](#char)
    - [varchar2](#varchar2)
    - [clob](#clob)
    - [blob](#blob)
    - [date](#date)
    - [TIMESTAMP](#timestamp)
  - [序列](#序列)
  - [管理表](#管理表)
    - [创建表](#创建表)
    - [修改表结构](#修改表结构)
    - [删除表](#删除表)
  - [管理数据](#管理数据)
    - [插入数据](#插入数据)
    - [删除数据](#删除数据)
  - [查询数据](#查询数据)
    - [内联查询](#内联查询)
    - [左连接](#左连接)
    - [右连接](#右连接)
    - [伪列](#伪列)

<!-- /TOC -->

# basic intro


Oracle数据库主要有四个版本，如下所示 －－ 企业版： 它是最强大和最安全的版本。 它提供所有功能，包括卓越的性能和安全性。
－ 标准版： 它为不需要企业版强大软件包，提供用户基本功能。
－ 易捷版(XE)： 它是轻量级，免费且有限的Windows和Linux版本。
－ Oracle Lite： 专为移动设备而设计。

oracle12c与11g 区别总结:

## basic concept

Oracle数据库服务器由一个数据库和至少一个数据库实例组成:

- Oracle 数据库 : 数据库是一组存储数据的文件

- Oracle 数据库实例: 内存+进程, 即程序, 是客户端应用程序(用户)和数据库之间的接口, 在现实环境当中一个服务器一般只会安装一个Oracle实例

  - 系统全局区(SGA)，SGA是实例启动时分配的共享内存结构，关闭时释放
  
  - 程序全局区(PGA), PGA是会话开始时为每个会话分配的私有内存区，当会话结束时释放
  
  - 后台进程


## 结构组成

- 逻辑存储结构, 从小到大: (包含关系)

  - 数据块(Data blocks), 类似磁盘空间

  - 范围(Extents)：存储数据块个数

  - 段(Segments)：段是分配用于存储用户对象(例如表记录或索引)的一组范围。

  - 表空间(Tablespaces)：数据库被分成多个逻辑存储单元(称为表空间)。 表空间是段的逻辑容器。 每个表空间至少包含一个数据文件。

- 物理存储结构: 存储数据的纯文件, 包括 

  - 数据文件, 包含真实数据
  
  - 控制文件(描述数据库的物理结构，包括数据库名称和数据文件的位置。), 
  
  - 联机重做日志文件, 记录数据修改操作

  - 归档重做日志文件, 用于备份和恢复


## 11g 和 12c 区别

https://blog.csdn.net/u011334954/article/details/96305572


## Oracle 在 windows 下的服务

Oracle在window下安装完毕，会安装很多服务:

- OracleService+服务名（ORCL）:是Oracle数据库的基础，只有启动该服务才能正常使用Oracle数据库

- OracleOraDb11g_home1TNSlistener: 该服务为Oracle客户端提供监听程序的服务，只有服务器启动该服务，本地的客户端程序才能通过监听连接到数据库，和数据库进行交互

  在使用第三方客户端连接Oracle数据库时，OracleOraDb11g_home1TNSlistener 服务必须启动，才能连接到远程数据库！

- Oracle ORCL VSS Writer Service: (非必须启动)

- OracleMTSRecoveryService: 服务端控制。(非必须启动)

- OracleOraDb11g_home1ClrAgent: Oracle数据库 .NET扩展服务的一部分。 (非必须启动)

- OracleJobSchedulerORCL：Oracle作业调度(定时器)服务，ORCL是Oracle实例标识。(非必须启动)

# how to use

## 登录断开

```sql
-- 连接
-- 使用 自动安装的交互式查询工具
sqlplus

-- 用户名 sys, 然后输入密码
sys as sysdba

-- 也可以这样:
-- 将OT用户连接到位于PDBORCL可插拔数据库中的示例数据库
sqlplus OT@PDBORCL



-- 在Oracle 12c中，当连接到数据库服务器时，默认数据库是名为CDB$ROOT的ROOT容器数据库
SHOW con_name


-- 如果要切换到可插拔数据库，请使用ALTER SESSION语句将当前数据库设置为可插入数据库
ALTER SESSION SET CONTAINER = PDBORCL;

-- 断开
EXIT

-- 重新启动Oracle数据库和实例
SQL> startup open


```

## 用户 和 权限

一个Oracle用户代表着一个用户群，他们通过该用户登录数据库，进行数据库对象的创建、查询等开发。

不同的项目组使用不同的Oracle用户进行开发，不相互干扰

也可以理解为一个Oracle用户既是一个业务模块，这些用户群构成一个完整的业务系统，不同模块间的关联可以通过Oracle用户的权限来控制，来获取其它业务模块的数据和操作其它业务模块的某些对象


```sql
-- 创建用户语法
create user student--用户名
  identified by "123456"--密码
  default tablespace USERS--表空间名
  temporary tablespace temp --临时表空间名
  profile DEFAULT    --数据文件（默认数据文件）
  account unlock; -- 账户是否解锁（lock:锁定、unlock解锁）

-- --修改用户信息
alter user STUDENT
  identified by ******  --修改密码
  account lock;--修改用户处于锁定状态或者解锁状态 （LOCK|UNLOCK ）


-- 权限
-- Oracle数据库用户权限分为：系统权限和对象权限两种。
-- 系统权限：比如：create session可以和数据库进行连接权限、create table、create view 等具有创建数据库对象权限。
    --  系统权限只能通过DBA用户授权，对象权限有拥有该对象权限的对象授权（不一定是本身对象）！用户不能自己给自己授权！
-- 对象权限：比如：对表中数据进行增删改查操作，拥有数据库对象权限的用户可以对所拥有的对象进行相应的操作。

-- 数据库角色
-- CONNECT角色：connect角色是Oracle用户的基本角色，connect权限代表着用户可以和Oracle服务器进行连接，建立session（会 话）。
-- RESOURCE角色：resouce角色是开发过程中常用的角色。 RESOURCE给用户提供了可以创建自己的对象，包括：表、视图、序列、过程、触发器、索引、包、类型等。
-- DBA角色：DBA角色是管理数据库管理员该有的角色。它拥护系统了所有权限，和给其他用户授权的权限。SYSTEM用户就具有DBA权限


-- 授权语法
--GRANT 对象权限 on 对象 TO 用户    
grant select, insert, update, delete on JSQUSER to STUDENT;
--GRANT 系统权限 to 用户
grant select any table to STUDENT;
--GRANT 角色 TO 用户
grant connect to STUDENT;--授权connect角色
grant resource to STUDENT;--授予resource角色

-- 取消授权
-- Revoke 对象权限 on 对象 from 用户 
revoke select, insert, update, delete on JSQUSER from STUDENT;
-- Revoke 系统权限  from 用户
revoke SELECT ANY TABLE from STUDENT;
-- Revoke 角色（role） from 用户
revoke RESOURCE from STUDENT;



-- 创建一个新用户:ot
CREATE USER OT IDENTIFIED BY Orcl1234;
-- 授予OT用户权限：
GRANT CONNECT, RESOURCE, DBA TO OT;

-- 切换用户
CONNECT ot@orcl



-- 执行 SQL 文件, 语法 @path_to_file
SQL> @F:\website\oraok\ot\11g\ot_schema.sql

```


## 特有的 SQL 语法

```sql
-- 查看当前用户拥有的表
SELECT table_name FROM user_tables ORDER BY Table_name;

-- 数据库服务器当前时间
SELECT
  sysdate
FROM
  dual;
```

## 数据类型

### number

```sql
-- NUMBER[(precision [, scale])]
-- 数字1234.56的精度是6，尺度是2。所以要存储这个数字，需要定义为：NUMBER(6,2)。

-- 数字超过精度p，则Oracle将发出错误。 但是，如果数量超过尺度s，则Oracle将对该值进行四舍五入

-- 定义一个整数
NUMBER(p) -- 等价 NUMBER(p,0)

-- 将数值四舍五入到百位
NUMBER(5,-2)



-- 定义数字列的别名:
INT	NUMBER(38)
SMALLINT	NUMBER(38)
NUMBER(p,s)	NUMBER(p,s)
DECIMAL(p,s)	NUMBER(p,s)


-- FLOAT数据类型是NUMBER数据类型的子类型。 其主要目的是促进与ANSI SQL FLOAT数据类型的兼容。

```

### char

```sql
-- 存储固定长度的字符串。 CHAR数据类型可以存储1到2000字节的字符串。默认长度 1

-- 实际插入长度不够, 空格来凑, 超出, 报错

-- 语法
CHAR(length BYTE)  -- 默认
CHAR(length CHAR)



-- 例子
CREATE TABLE t (
    x CHAR(10), -- 长度 10 字节
    y VARCHAR2(10)
);


-- NCHAR
-- 用于存储固定长度的Unicode字符数据。NCHAR的字符集只能是AL16UTF16或UTF8

```


### varchar2


```sql
-- VARCHAR2
-- 可变长度的字符串
-- 可以存储1到4000字节的值。 这意味着对于单字节字符集，最多可以在VARCHAR2列中存储4000个字符

-- 语法
VARCHAR2(max_size BYTE) -- 默认使用 byte 单位
VARCHAR2(max_size char)




-- VARCHAR2列只能将字符存储在默认字符集中，而NVARCHAR2则可以存储几乎任何字符
```

### clob

大字段类型：存储的是大的文本，比如：非结构化的txt文本，字段大于4000长度的字符串。


### blob

二进制类型：存储的是二进制对象，比如图片、视频、声音等转换过来的二进制对象


### date

```sql
-- DATE数据类型存储年份(包括世纪)，月份，日期，小时数，分钟数和秒数。 它的范围从公元前4712年1月1日到公元9999年12月31日(共同时代)
-- 使用7个字节的固定长度的字段
-- 标准日期格式是DD-MON-YY，例如由NLS_DATE_FORMAT参数的值表示为： 01-JAN-17



-- 将标准日期格式更改为YYYY-MM-DD
ALTER SESSION SET NLS_DATE_FORMAT = 'YYYY-MM-DD';

-- 格式转换
SELECT
  TO_CHAR( SYSDATE, 'MM/DD/YYYY' )
FROM
  dual;


SELECT
  TO_DATE('2018-10-21', 'YYYY-MM-DD' )
FROM
  dual;

DATE '2017-08-01' -- 返回 date 类型, 输入格式必须 'YYYY-MM-DD'




-- 修改日期显示语言
ALTER SESSION SET NLS_DATE_LANGUAGE = 'FRENCH';
-- alter session set nls_language='SIMPLIFIED CHINESE';

```

### TIMESTAMP


```sql
-- 包括年，月，日，时，分和秒。
column_name TIMESTAMP[(fractional_seconds_precision)]
-- fractional_seconds_precision指定SECOND字段小数部分的位数。它的范围从0到9，这意味着可以使用TIMESTAMP数据类型来存储到纳秒的精度。
-- 如果省略fractional_seconds_precision，则默认为6

TIMESTAMP 'YYYY-MM-DD HH24:MI:SS.FF' -- 返回 timestamp 类型 


SELECT message,
    TO_CHAR(logged_at, 'MONTH DD, YYYY "at" HH24:MI')
FROM logs;

-- 提取年月日
SELECT 
    message,
    EXTRACT(year FROM logged_at) year,
    EXTRACT(month FROM logged_at) month,
    EXTRACT(day FROM logged_at) day,
    EXTRACT(hour FROM logged_at) hour,
    EXTRACT(minute FROM logged_at) minute,
    EXTRACT(second FROM logged_at) second
FROM 
    logs;


INSERT INTO logs (
    message,
    logged_at
    )
VALUES (
    'Test default Oracle timestamp format',
    TO_TIMESTAMP('03-AUG-17 11:20:30.45 AM')
    );
```

## 序列

序列Sequence是用来生成连续的整数数据的对象, 12c 中已经无需序列了

```sql
CREATE SEQUENCE sequence_name
[MAXVALUE num|NOMAXVALUE]
[MINVALUE num|NOMINVALUE]
[START WITH num]
[INCREMENT BY increment]
[CYCLE|NOCYCLE]
[CACHE num|NOCACHE]
```


## 管理表


### 创建表

```sql
-- oracle 12c 创建表语法
-- ot模式(或者称为用户)中创建一个的新表
CREATE TABLE ot.discounts (
    discount_id NUMBER GENERATED BY DEFAULT AS IDENTITY PRIMARY KEY,
    employee_id NUMBER not null,
    discount_name VARCHAR2(255) NOT NULL,
    amount NUMBER(3,1) NOT NULL,
    start_date DATE NOT NULL,
    expired_date DATE NOT NULL,
    CONSTRAINT fk_employee_id FOREIGN KEY( employee_id ) REFERENCES employees( employee_id ) 
        ON  DELETE CASCADE
)
tablespace USERS -- 存储到表空间 users 中
  storage -- 表示存储参数
  (
    initial 64K  -- 区段(extent)一次扩展64k，最小区段数为1，最大的区段数不限制。
    minextents 1
    maxextents unlimited
  );
-- Add comments to the table 
comment on column ot.discounts.col_name is 'xxx';
comment on table ot.discounts is 'xxxx表'

-- oracle 11g 创建表语法
drop sequence discounts_seq;

create sequence discounts_seq
 increment by 1
 start with 1
 maxvalue 9999999999
 nocache;

CREATE TABLE discounts (
    discount_id NUMBER, -- 插入数据时, 这列必须提供值: discounts_seq.nextval
    discount_name VARCHAR2(255) NOT NULL,
    amount NUMBER(3,1) NOT NULL,
    start_date DATE NOT NULL,
    expired_date DATE NOT NULL
);



-- 从其他表创建新表
CREATE TABLE customers_copy 
AS
(
SELECT
  *
FROM
  customers
)

```


### 修改表结构

```sql
ALTER TABLE persons 
ADD birthdate DATE NOT NULL;



ALTER TABLE table_name
ADD (
    column_name type constraint,
    column_name type constraint,
    ...
);


ALTER TABLE persons MODIFY birthdate DATE NULL;


ALTER TABLE persons MODIFY(
    phone VARCHAR2(20) NOT NULL,
    email VARCHAR2(255) NOT NULL
);



ALTER TABLE table_name
DROP COLUMN column_name;


ALTER TABLE persons 
RENAME COLUMN first_name TO forename;



ALTER TABLE persons RENAME TO people;

```

### 删除表

```sql
DROP TABLE brands;

-- 若有外键约束导致使用 drop 无法删除, 使用如下: (同时删除外键约束)
DROP TABLE brands CASCADE CONSTRAINTS;


-- 无法恢复
DROP TABLE cars purge;



```


## 管理数据

### 插入数据


```sql
-- Oracle 12c语法
INSERT INTO discounts(discount_name, amount, start_date, expired_date)
VALUES('双11电脑特价', 6.5, DATE '2017-11-11', DATE '2017-11-12');

-- oracle 11g语法
INSERT INTO discounts(discount_id, discount_name, amount, start_date, expired_date)
VALUES(discounts_seq.nextval, '双11电脑特价', 6.5, DATE '2017-11-11', CURRENT_DATE);



INSERT INTO target_table (col1, col2, col3)
SELECT col1,
       col2,
       col3
FROM source_table
WHERE condition;
```

### 删除数据


```sql
delete from some_table where ...


-- 删除大量数据速度快, cascade 表示级联删除
TRUNCATE TABLE table_name [cascade];
```

## 查询数据

### 内联查询

```sql
-- inner join
-- 它将表orders的每一行与表order_items的行进行比较，以查找满足连接谓词的所有行记录
SELECT
    *
FROM
    orders
INNER JOIN order_items 
-- on 子句可以使用 using(col_name) 代替, col_name 为公共字段
ON order_items.order_id = orders.order_id 
ORDER BY
    order_date DESC;

-- 条件放在ON子句中与放在WHERE子句中具有相同的效果

```

### 左连接

```sql

-- LEFT [outer] JOIN
-- 返回左表中的所有行，并从右表中返回匹配的行
-- 如果T1和T2表中的一对行满足连接谓词，查询将组合两个表中行的列值，
-- 如果T1表中的行在T2表中没有找到匹配的行，则查询将会将SELECT子句中出现在T2表的每个列的值设置为NULL并与T1表的行记录组合作为结果集输出
SELECT
    column_list
FROM
    T1
LEFT JOIN T2 ON
    join_predicate;



-- 对于左连接, 这两种是不同的
-- 写法 1: -- 条件在 where 子句, 作为连接后整张表的条件, 此时只返回订单ID为58的订单和销售员数据
SELECT
    order_id,
    status,
    employee_id,
    last_name
FROM
    orders
LEFT JOIN employees ON
    employee_id = salesman_id
WHERE
    order_id = 58; 

-- 写法 2: 将条件从WHERE子句移动到LEFT JOIN的ON子句, 查询将返回所有订单，但只有订单58具有与其关联的销售员数据
SELECT
    order_id,
    status,
    employee_id,
    last_name
FROM
    orders
LEFT JOIN employees ON
    employee_id = salesman_id
    AND order_id = 58; -- 只会和 id=58 的 order 记录做连接


```


### 右连接

```sql
-- RIGHT [OUTER] JOIN
-- 右外连接返回右表中的所有行，以及左表中的匹配行。
SELECT
    column_list
FROM
    T1
RIGHT OUTER JOIN T2 ON
    join_predicate;

```


### 伪列

伪列就像表中的字段一样，但是表中并不存储。伪列只能查询，不能增删改。Oracle的伪列有：rowid、rownum。



```sql
-- 使用 ROWID 可以快速的定位表中的某一行。 ROWID 值可以唯一的标识表中的一行
select t.*,t.rowid from stuinfo t where t.rowid='AAAShjAAEAAAAEFAAD';


-- ROWNUM为每个查询结果集的行标识一个行号，第一行返回1，第二行返回2，依次顺序递增
-- 可用来作为分页
-- ROWNUM 与 ROWID 不同， ROWID 是插入记录时生成， ROWNUM 是查询数据时生成。ROWID 标识的是行的物理地址。 ROWNUM 标识的是查询结果中的行的次序
select t.stuid,t.stuname,t.sex,t.classno,t.stuaddress ,rownum  from stuinfo t ;
```