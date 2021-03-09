---
title: Mysql note
tags:
  - mysql
date: 2018-07-01 16:02:53
categories: java web
---


<div align="center">
[常用命令](https://blog.csdn.net/ooooooobh/article/details/54949274)
索引原理：http://blog.codinglabs.org/articles/theory-of-mysql-index.html
<div>

<!--more-->

<!-- TOC -->

- [命令行](#命令行)
- [SQL](#sql)
  - [各种查询](#各种查询)
  - [连接-元数据查看](#连接-元数据查看)
  - [user用户管理-权限](#user用户管理-权限)
  - [数据库管理](#数据库管理)
  - [表管理](#表管理)
    - [查看表结构](#查看表结构)
    - [创建删除表](#创建删除表)
    - [修改表结构](#修改表结构)
    - [操作索引](#操作索引)
  - [数据管理](#数据管理)
    - [增删改数据](#增删改数据)
    - [查询数据](#查询数据)
- [分库分表和读写分离-表设计](#分库分表和读写分离-表设计)
  - [读写分离配置](#读写分离配置)
- [MySQL](#mysql)
  - [MySQL的数据类型](#mysql的数据类型)
    - [选择合适的数据类型](#选择合适的数据类型)
    - [常见数据类型](#常见数据类型)
    - [java jdbc 和 mysql 数据库的字段对应关系](#java-jdbc-和-mysql-数据库的字段对应关系)
    - [Java中几种Date相关数据类型的区别](#java中几种date相关数据类型的区别)
    - [字段长度的意义and格式](#字段长度的意义and格式)
  - [MySQL存储过程](#mysql存储过程)
  - [存储引擎](#存储引擎)
    - [MyisAM与InnoDB相比较](#myisam与innodb相比较)
  - [范式and反范式](#范式and反范式)
  - [汇总表and缓存表](#汇总表and缓存表)
  - [优化 SQL](#优化-sql)
    - [基本原则](#基本原则)
    - [执行计划 explain](#执行计划-explain)
  - [怎么加快alter table操作](#怎么加快alter-table操作)
  - [怎么创建高性能索引](#怎么创建高性能索引)
    - [什么是索引](#什么是索引)
    - [索引的类型](#索引的类型)
      - [b-tree 索引](#b-tree-索引)
        - [满足哪些要求才能够使用到b-tree索引呢](#满足哪些要求才能够使用到b-tree索引呢)
        - [两种实现方式](#两种实现方式)
      - [哈希索引](#哈希索引)
        - [创建自定义hash索引](#创建自定义hash索引)
      - [空间数据索引](#空间数据索引)
      - [全文索引](#全文索引)
    - [索引有什么优缺点](#索引有什么优缺点)
    - [创建索引需要遵循哪些原则](#创建索引需要遵循哪些原则)
      - [始终使用最简化的条件表达式](#始终使用最简化的条件表达式)
      - [使用前缀索引and什么是索引选择性](#使用前缀索引and什么是索引选择性)
      - [联合索引也许好过多个单列索引](#联合索引也许好过多个单列索引)
      - [选择合适的索引列顺序 最左前缀匹配原则](#选择合适的索引列顺序-最左前缀匹配原则)
      - [多利用聚簇索引](#多利用聚簇索引)
      - [多利用索引覆盖](#多利用索引覆盖)
      - [使用索引进行排序](#使用索引进行排序)
      - [MyISAM引擎支持索引压缩](#myisam引擎支持索引压缩)
      - [不要冗余的索引](#不要冗余的索引)
      - [删除未使用的索引](#删除未使用的索引)
      - [索引and锁](#索引and锁)
      - [多利用 change buffer 所以不要使用唯一索引](#多利用-change-buffer-所以不要使用唯一索引)
    - [索引案例demo](#索引案例demo)
  - [binlog](#binlog)
    - [canal 增量日志订阅](#canal-增量日志订阅)
  - [MySQL的锁机制](#mysql的锁机制)
    - [表锁](#表锁)
    - [行锁](#行锁)
      - [记录锁](#记录锁)
      - [区间锁(间隙锁)](#区间锁间隙锁)
      - [临键锁](#临键锁)
  - [多版本并发控制 mvcc](#多版本并发控制-mvcc)
    - [为什么需要 mvcc](#为什么需要-mvcc)
    - [什么是 mvcc](#什么是-mvcc)
  - [事务](#事务)
    - [事务acid](#事务acid)
    - [实现 acid 特性的原理](#实现-acid-特性的原理)
      - [原子性](#原子性)
      - [持久性](#持久性)
      - [隔离性](#隔离性)
      - [一致性](#一致性)
    - [并发问题和隔离级别](#并发问题和隔离级别)
- [sql-server](#sql-server)
  - [数据类型](#数据类型)
    - [string类型](#string类型)
    - [number类型](#number类型)
    - [date类型](#date类型)
    - [sqlserver特有类型](#sqlserver特有类型)
  - [存储过程](#存储过程)
    - [无参存储过程](#无参存储过程)
    - [有参存储过程](#有参存储过程)
      - [参数是全局变量](#参数是全局变量)
      - [参数是局部变量](#参数是局部变量)
  - [碰到的问题记录](#碰到的问题记录)
- [oracle](#oracle)
  - [字段类型](#字段类型)
  - [Oracle  sql](#oracle--sql)

<!-- /TOC -->


# 命令行

怎么在命令行执行sql呢

> 如果未添加mysql的环境变量，可以切换到mysql的安装目录下的bin目录，再使用

方法1: 通过 shell script 执行sql:https://blog.csdn.net/stpeace/article/details/78334653

```sh
mysql -uroot -proot         # 登陆

source /var/ftp/pub/sogoodsoft.sql; # 执行sql脚本, 需要先登录MySQL

mysql -u root < script.sql # 方法2

```

# SQL

## 各种查询

```sql
/* Student(S#,Sname,Sage,Ssex) 学生表 
Course(C#,Cname,T#) 课程表 
SC(S#,C#,score) 成绩表 
Teacher(T#,Tname) 教师表 */


---------
-- 初始化
--------------
DROP TABLE IF EXISTS `student`;

CREATE TABLE `student` (
  `id` VARCHAR(36) CHARACTER SET utf8 NOT NULL COMMENT '主键',
  `name` varchar(50) CHARACTER SET utf8 NOT NULL COMMENT '姓名',
  `age` TINYINT(4) NOT NULL COMMENT '年龄',
  `sex` TINYINT(4) not NULL COMMENT '性别',
  PRIMARY KEY (`id`)
) ENGINE = INNODB DEFAULT CHARSET = utf8 COLLATE = utf8_bin COMMENT = '学生';

INSERT INTO `student` VALUES ('essgs', '小a', 10, 1);
INSERT INTO `student` VALUES ('esetcgfefss', '小b', 11, 1);
INSERT INTO `student` VALUES ('sisisow', '小c', 10, 0);
INSERT INTO `student` VALUES ('sooiaoeo', '小d', 13, 0);

drop table if EXISTS `course`;
CREATE TABLE `course` (
`id` VARCHAR(36) CHARACTER SET utf8 NOT NULL COMMENT '主键',
`name` varchar(50) CHARACTER SET utf8 NOT NULL COMMENT '课程名',
`t_id` VARCHAR(36) CHARACTER SET utf8 NOT NULL COMMENT '教师主键',
PRIMARY KEY (`id`)
) ENGINE = INNODB DEFAULT CHARSET = utf8 COLLATE = utf8_bin COMMENT = '课程';
INSERT INTO `course` VALUES ('eieiew', '语文', 'aqqee');
INSERT INTO `course` VALUES ('yutiwoq', '数学', 'eueuqee');
INSERT INTO `course` VALUES ('bnbnis', '英语', 'eiweqo');

drop table if exists `score`;
create TABLE `score` (
`id` VARCHAR(36) CHARACTER SET utf8 NOT NULL COMMENT '主键',
`s_id` VARCHAR(36) CHARACTER SET utf8 NOT NULL COMMENT '学生主键',
`c_id` VARCHAR(36) CHARACTER SET utf8 NOT NULL COMMENT '课程主键',
`score` decimal(4,1) NOT NULL COMMENT '分数',
PRIMARY KEY (`id`)
) ENGINE = INNODB DEFAULT CHARSET = utf8 COLLATE = utf8_bin COMMENT = '成绩';
INSERT INTO score VALUES('ghieo', 'essgs', 'eieiew', 50);
INSERT INTO score VALUES('ririr', 'essgs', 'yutiwoq', 60);
INSERT INTO score VALUES('ioioio', 'essgs', 'bnbnis', 70);
INSERT INTO score VALUES('uououo', 'esetcgfefss', 'eieiew', 70);
INSERT INTO score VALUES('rprpr', 'esetcgfefss', 'yutiwoq', 65);
INSERT INTO score VALUES('erere', 'esetcgfefss', 'bnbnis', 40);

drop table if EXISTS `teacher`;
CREATE TABLE `teacher` (
`id` VARCHAR(36) CHARACTER SET utf8 NOT NULL COMMENT '主键',
`name` varchar(50) CHARACTER SET utf8 NOT NULL COMMENT '姓名',
PRIMARY KEY (`id`)
) ENGINE = INNODB DEFAULT CHARSET = utf8 COLLATE = utf8_bin COMMENT = '教师';
INSERT INTO `teacher` VALUES ('aqqee', '老a');
INSERT INTO `teacher` VALUES ('eueuqee', '老b');
INSERT INTO `teacher` VALUES ('eiweqo', '老c');

----------
-- 问题
---------
 
--1、查询“语文”课程比“数学”课程成绩高的所有学生的学号； 
select c# from course where cname='语文'

select sc.s# from sc left join sc sc1 on sc.s#=sc1.s# where sc.c#=(select c# from course where cname='语文') and sc1.c#=(select c# from course where cname='数学') and sc.score>sc1.score

--更新， 两者都可以
SELECT student.id, t1.score, t2.score FROM student,
(SELECT * from score s WHERE s.c_id = (SELECT c.id FROM course c WHERE c.name='语文' LIMIT 1)) AS t1,
(SELECT * from score s WHERE s.c_id = (SELECT c.id FROM course c WHERE c.name='数学' LIMIT 1)) AS t2
WHERE student.id = t1.s_id AND t1.s_id = t2.s_id AND t1.score > t2.score;

select s1.s_id, s1.score, s2.score FROM score s1 INNER JOIN score s2 on s1.s_id = s2.s_id WHERE s1.c_id = (SELECT c.id FROM course c WHERE c.name='语文' LIMIT 1)
AND s2.c_id = (SELECT c.id FROM course c WHERE c.name='数学' LIMIT 1) AND s1.score > s2.score


/* Student(S#,Sname,Sage,Ssex) 学生表 
Course(C#,Cname,T#) 课程表 
SC(S#,C#,score) 成绩表 
Teacher(T#,Tname) 教师表 */

--2、查询平均成绩大于60分的同学的学号和平均成绩； 
 select s#, avg(score) avg_s from sc group by s# having avg(score)>60
--3、查询所有同学的学号、姓名、选课数、总成绩； 
  select s# '学号', count(c#) '选课数', sum(score) '总成绩' from sc group by s#
  
--4、查询姓“李”的老师的个数； 
  select COUNT(t#) from teacher where tname like '叶%'




--5、查询没学过“叶平”老师课的同学的学号、姓名； 
?
select  distinct s# from sc where c# not in (
    select c# from course where t#=(select t# from teacher where tname='叶平')
)

 
--6、查询学过“1”并且也学过编号“2”课程的同学的学号、姓名； 
select s#, sname from student where s# in (
    select sc.s# from sc left join sc sc1 on sc.s#=sc1.s# where sc.c#='1' and sc1.c#='2'
)

select s# from sc where s# in (
    select s# from sc where c# ='1')
and c# = '2'

 
--7、查询学过“叶平”老师所教的所有课的同学的学号、姓名； 
select s#, sname from student where s# in (
select s# from sc where c# in (
  select c# from course where t#=(select t# from teacher where tname='叶平')
)
)
--8、查询课程编号“2”的成绩比课程编号“1”课程低的所有同学的学号、姓名； 
select s#, sname from student where s# in (
    select sc.s# from sc left join sc sc1 on sc.s#=sc1.s# where sc.c#='1' and sc1.c#='2' and sc.score>sc1.score
)
--9、查询所有课程成绩小于60分的同学的学号、姓名； 
  select s#, max(score) from sc group by s# having max(score)<60


  
  
  
  
--10、查询没有学全所有课的同学的学号、姓名； 
select student.s#, student.sname from student 
right join (
   select s# from sc 
   group by s# 
   having COUNT(c#) < (select COUNT(c#) from course)
) t on student.s#=t.s#

Student(S#,Sname,Sage,Ssex) 学生表 
Course(C#,Cname,T#) 课程表 
SC(S#,C#,score) 成绩表 
Teacher(T#,Tname) 教师表
--11、查询至少有一门课与学号为“1”的同学所学相同的同学的学号和姓名； ##反面：与1号所学完全不同
select student.s#, student.sname from student 
right join (
  select distinct s# from sc where s# not in (
     select distinct s# from sc where c# not in (
      select c# from sc where s#='1'
    )
  ) 
) t on student.s# = t.s#
--12、查询至少学过学号为“1”同学所有一门课的其他同学学号和姓名； ##同上
    


--13、把“SC”表中“叶平”老师教的课的成绩都更改为此课程的平均成绩； 
update sc set score = (
  select avg(score) from sc group by c# having c# = (
    select course.c# from teacher 
    inner join course 
    on course.t# = teacher.t# and teacher.tname = '叶平'
  ) 
) where c# = (
  select course.c# from teacher 
  inner join course 
  on course.t# = teacher.t# and teacher.tname = '叶平'
)

--14、查询和“2”号的同学学习的课程完全相同的其他同学学号和姓名；
select s#, sname from student where s# in ( 
  select t1.s# from (
    select s# from sc 
    group by s# having count(c#)=(
      select count(c#) from sc where s#='2'
    )
  ) t1 inner join (
    select distinct s# from sc where c# in (
      select c# from sc where s#='2'
    ) 
  ) t2 on t1.s#=t2.s#
)


   
--15、删除学习“叶平”老师课的SC表记录； 
delect from sc where c# in (
     select course.c# from course 
   inner join teacher on teacher.t#=course.t# and teacher.tname='叶平'
)
   

 Student(S#,Sname,Sage,Ssex) 学生表 
Course(C#,Cname,T#) 课程表 
SC(S#,C#,score) 成绩表 
Teacher(T#,Tname) 教师表
--16、向SC表中插入一些记录，这些记录要求符合以下条件：没有上过编号“003”课程的同学学号、2、 号课的平均成绩； 
 insert into sc () values ()   
--17、按平均成绩从高到低显示所有学生的“数据库”、“企业管理”、“英语”三门的课程成绩，按如下形式显示： 学生ID,,数据库,企业管理,英语,有效课程数,有效平均分 
      
--18、查询各科成绩最高和最低的分：以如下形式显示：课程ID，最高分，最低分 
select course.cname, t.max_score,t.min_score from (
  select * from (
    select c# max_c#, max(score) max_score from sc group by c#
  ) t1 inner join (
    select c# min_c#, min(score) min_score from sc group by c#
  ) t2 on t1.max_c#=t2.min_c#
) t 
left join course on course.c# = t.max_c#




--19、按各科平均成绩从低到高和及格率的百分数从高到低顺序-- 
select t3.c#, t3.及格率, t4.平均分 from (
  select t1.c#, cast(t1.及格数 as float)/cast(t2.总人数 as float) '及格率' from (
    select t.c#,COUNT(t.s#) '及格数' from (select c#,s# from sc where score>=60) t group by t.c#--及格科目的及格人数
  ) t1 
  inner join (
    select c#, count(s#) '总人数'  from sc group by c#
  ) t2 on t1.c#=t2.c# --算出及格率
) t3 left join (
  select c#, avg(score) '平均分' from sc group by c# --各科平均分
) t4 on t3.c#=t4.c# order by '及格率' desc 



--20、查询如下课程平均成绩和及格率的百分数(用"1行"显示): 企业管理（001），马克思（002），OO&UML （003），数据库（004） ##各科
同上
    
--21、查询不同老师所教不同课程平均分从高到低显示 
  select teacher.tname, t.c#,t.平均分 from (
    select sc.c#, avg(score) '平均分' from sc group by c#
  ) t
  inner join course on t.c#=course.c#
  inner join teacher on teacher.t#=course.t#
  
Student(S#,Sname,Sage,Ssex) 学生表 
Course(C#,Cname,T#) 课程表 
SC(S#,C#,score) 成绩表 
Teacher(T#,Tname) 教师表
--22、查询如下课程成绩第 3 名到第 6 名的学生成绩单：企业管理（1），马克思（002），UML （003），数据库（004） 
--    [学生ID],[学生姓名],企业管理,马克思,UML,数据库,平均成绩 

select * from (
  select ROW_NUMBER() over (order by avg(score) ) sort,  s#, avg(score) avg_score from sc  group by s# 
) t where t.sort between 3 and 6

--23、统计列印各科成绩,各分数段人数:课程ID,课程名称,[100-85],[85-70],[70-60],[ <60] 
 select t1.c#, t1.[(100,85)], t2.[(85,70)], t3.[(70,60)], t4.[(60,0)] from (
   select c#, count(s#) '(100,85)' from (
     select * from sc where score between 85 and 100
   ) t group by c#
 ) t1 
 left join (
    select c#, count(s#) '(85,70)' from (
     select * from sc where score between 70 and 85
   ) t group by c#
) t2 on t1.c# = t2.c#
left join (
    select c#, count(s#) '(70,60)' from (
     select * from sc where score between 60 and 70
   ) t group by c#
 ) t3 on t1.c# = t2.c#
 left join (
    select c#, count(s#) '(60,0)' from (
     select * from sc where score between 0 and 60
   ) t group by c#
 ) t4 on t1.c# = t4.c#

--24、查询学生平均成绩及其名次 
 select row_number() over (order by avg(score)) '名次', s#, avg(score) '均分' from sc group by sc.s#     
--25、查询各科成绩前三名的记录:(不考虑成绩并列情况) 
select row_number() over (order by score) sort, * from sc where c# = '1' 
名次 1 2 3 4
1    
2
3

select t1.sort, t1.s# '语文', t2.s# '数学',t3.s# '英语', t4.s# '物理' from (
  select top 3 row_number() over (order by score desc) sort, * from sc where c# = '1' 
) t1 inner join (
  select top 3 row_number() over (order by score desc) sort, * from sc where c# = '2' 
) t2 on t1.sort = t2.sort
inner join ( 
  select top 3 row_number() over (order by score desc) sort, * from sc where c# = '3' 
) t3 on t3.sort = t1.sort
inner join ( 
    select top 3 row_number() over (order by score desc) sort, * from sc where c# = '4' 
) t4 on t4.sort = t1.sort


  
  
--26、查询每门课程被选修的学生数 
  select c#, count(s#) samount from sc group by c#
--27、查询出只选修了一门课程的全部学生的学号和姓名 
  select student.s#, student.sname from student inner join (
  select s#, count(c#) camount from sc 
  group by s# having count(c#)=1
) t on  student.s# = t.s#
  
  
--28、查询男生、女生人数 
 select ssex, count(s#)  from student group by ssex 
--29、查询姓“张”的学生名单 
   select * from student where sname like '张%'
--30、查询同名同性学生名单，并统计同名人数 
    select  s1.s#, s1.sname from student s1 
  inner join student s2 on s1.sname=s2.sname and s1.s# <> s2.s#
并统计同名人数 ?


Student(S#,Sname,Sage,Ssex) 学生表 
Course(C#,Cname,T#) 课程表 
SC(S#,C#,score) 成绩表 
Teacher(T#,Tname) 教师表
--31、1981年出生的学生名单(注：Student表中Sage列的类型是datetime) 
 select * from student where sage = 2017-1981
--32、查询每门课程的平均成绩，结果按平均成绩升序排列，平均成绩相同时，按课程号降序排列 
select c#, avg(score) avg_score from sc 
group by c# order by avg(score), c# desc
--33、查询平均成绩大于85的所有学生的学号、姓名和平均成绩 
select student.s#, student.sname, t.avg_score from student 
inner join (
   select s#, avg(score) avg_score from sc 
   group by s# having avg(score) > 85
) t on student.s# = t.s#
--34、查询课程名称为“数据库”，且分数低于60的学生姓名和分数 
select student.sname, t.score from student 
inner join (
  select s#,score from sc where c# = (
   select c# from course where cname = '语文' 
  ) and score <60
) t on student.s# = t.s#
--35、查询所有学生的选课情况； 
select student.sname, course.cname from student 
left join sc on student.s# = sc.s# 
left join course on course.c# = sc.c#
--36、查询任何一门课程成绩在70分以上的姓名、课程名称和分数； 
select distinct student.sname, course.cname, sc.score from student inner join sc on student.s# = sc.s# inner join course on course.c# = sc.c# 
where sc.score > 70
--37、查询不及格的课程，并按课程号从大到小排列 ##学生号正序排列

select course.c#, course.cname, t.s# from course right join (
   select c#, s# from sc where sc.score < 60 
) t on course.c# = t.c# order by course.c# desc, t.s#


--38、查询课程编号为3且课程成绩在80分以上的学生的学号和姓名； 
 select student.s#, student.sname from student 
 inner join sc on student.s# = sc.s# 
 inner join course on course.c# = sc.c# 
 where course.c# = '3' and sc.score > 80   
--39、求选了课程的学生人数 
 select COUNT(*) '选课学生数' from (
select distinct s# from sc) t   
--40、查询选修“叶平”老师所授课程的学生中，成绩最高的学生姓名及其成绩 

select top 1 sc.s#, student.sname, sc.score from sc 
left join student on student.s# = sc.s#
 where sc.c# in (
    select course.c# from course 
    right join (
      select t# from teacher where tname = '叶平'
    ) t on t.t# = course.t#
) order by score desc



  Student(S#,Sname,Sage,Ssex) 学生表 
Course(C#,Cname,T#) 课程表 
SC(S#,C#,score) 成绩表 
Teacher(T#,Tname) 教师表   
--41、查询各个课程及相应的选修人数 
select course.cname, t.samount from course right join (
  select c#, count(s#) samount from sc group by c# 
) t on t.c# = course.c#

?--42、查询不同课程成绩相同的学生的学号、课程号、学生成绩 
 select distinct sc1.* from sc sc1 inner join sc sc2 on sc1.score=sc2.score and sc1.c# = sc2.c# and sc1.s# <> sc2.s#
 
--43、查询每门功成绩最好的前两名 
同25
 
--44、统计每门课程的学生选修人数（超过10人的课程才统计）。要求输出课程号和选修人数，查询结果按人数降序排列，查询结果按人数降序排列，若人数相同，按课程号升序排列  
 select course.c#, course.cname, t.samount from course right join (
  select c#, count(s#) samount from sc group by c# 
) t on t.c# = course.c#  order by samount desc, c#
--45、检索至少选修两门课程的学生学号 ##反面：选了一门、零门
select distinct s# from sc where s# not in (
  select t.s# from (
    select s#, count(c#) camount from sc group by s# 
  ) t where t.camount in (0,1)
)

 Student(S#,Sname,Sage,Ssex) 学生表 
Course(C#,Cname,T#) 课程表 
SC(S#,C#,score) 成绩表 
Teacher(T#,Tname) 教师表 
--46、查询全部学生都选修的课程的课程号和课程名 
select t1.c#, course.cname from (
  select c#, count(s#) samount from (
    select distinct c#, s# from sc
  ) t group by c# 
) t1 inner join course on course.c# = t1.c#
 where t1.samount = (select COUNT(s#) from student)
--47、查询没学过“叶平”老师讲授的任一门课程的学生姓名 ##反面：最少一门
select sname from student where s# not in (
  select distinct s# from sc where c#  in (
    select course.c# from course 
    inner join teacher on teacher.t# = course.t# and teacher.tname = '叶平'
  )
) 



--48、查询两门以上不及格课程的同学的学号及其平均成绩 
 
select sc.s#, avg(sc.score) avg_score from sc right join (
   select * from (
     select s#, COUNT(c#) camount from (
      select * from sc where score < 60  
     ) t group by s#
   ) t1 where camount > 1 
 ) t2 on sc.s# = t2.s#  group by sc.s#

--49、检索“4”课程分数小于60，按分数降序排列的同学学号 
 select s# from sc where c# = '4' and score < 60 order by score desc
--50、删除“002”同学的“001”课程的成绩 
delete from sc where s# = '2' and c# = '1'



--假设有表ARTICLE,字段ID、YEAR...(其他省略)，数据53210条(客户真实数据，量不大)，
分页查询每页30条，查询第1500页（即第45001-45030条数据），字段ID聚集索引，YEAR无索引，Sqlserver版本：2008R2

    SELECT TOP 30 * FROM ARTICLE 
  WHERE ID NOT IN(
    SELECT TOP 45000 ID FROM ARTICLE ORDER BY YEAR DESC, ID DESC
  ) ORDER BY YEAR DESC,ID DESC  


SELECT w2.n, w1.* FROM ARTICLE w1, (
　　SELECT TOP 50030 row_number() OVER (ORDER BY YEAR DESC, ID DESC) n, ID FROM ARTICLE 
) w2 WHERE w1.ID = w2.ID AND w2.n > 50000 ORDER BY w2.n ASC 

```

## 连接-元数据查看

```sql
mysql -h 地址 -P 端口 -u 用户名 -p 密码
SHOW PROCESSLIST -- 显示哪些线程正在运行
SHOW VARIABLES -- 显示系统变量信息
```

## user用户管理-权限

可以参考这里 {% post_link linux-note 📚 Linux笔记 %}/#mysql环境

[一份 Ubuntu下 mysql user 设置指南](http://wiki.ubuntu.org.cn/MySQL%E5%AE%89%E8%A3%85%E6%8C%87%E5%8D%97)

```sql
-- 默认root没有密码
GRANT ALL PRIVILEGES ON *.* TO root@localhost IDENTIFIED BY "123456"; -- 设置root密码, *.*表示 <database>.<table>
-- 或者: 直接通过shell脚本
mysqladmin -u root password "root123"

--如果你想进行远程访问或控制，那么你要做两件事
GRANT ALL PRIVILEGES ON xoops.* TO xoops_root@"%" IDENTIFIED BY "654321";--允许xoops_root用户可以从任意机器上登入MySQL。
-- 还要 编辑 /etc/mysql/my.cnf , 注释掉 # skip-networking (旧版), 或者 bind-address=127.0.0.1 => bind-address= 你机器的IP (新版)



```

## 数据库管理

```sql
-- 显示当前时间、用户名、数据库版本
SELECT now(), user(), version();

SET NAMES utf8;            -- # 设置一下字符集

-- 创建库
CREATE DATABASE[ IF NOT EXISTS] 数据库名 数据库选项
数据库选项：
    CHARACTER SET charset_name
    COLLATE collation_name
-- 删除库
DROP DATABASE[ IF EXISTS] 数据库名

SHOW DATABASES [like 'pattern'];             --# 显示所有数据库

-- 查看当前数据库, 数据库元信息(引擎啥的)
SELECT DATABASE();
SHOW CREATE DATABASE 数据库名

-- 修改库的选项信息
ALTER DATABASE 库名 选项信息

use db_name                 --# 使用指定数据库


select database();          -- 当前使用的 db
show variables  like 'port'; -- 查看数据库使用端口, (编码等等)
show status like 'Threads%'; -- 数据库当前连接数，并发数
show variables like '%datadir%'; --数据文件存放路径

show engines; -- mysql 支持的引擎
show variables like '%storage_engine%'; -- mysql当前默认的存储引擎:
alter table table_name engine=innodb; -- 修改表引擎

字符集编码

/* 字符集编码 */ ------------------
-- MySQL、数据库、表、字段均可设置编码
-- 数据编码与客户端编码不需一致
SHOW VARIABLES LIKE 'character_set_%'   -- 查看所有字符集编码项
    character_set_client        客户端向服务器发送数据时使用的编码
    character_set_results       服务器端将结果返回给客户端所使用的编码
    character_set_connection    连接层编码
SET 变量名 = 变量值
    SET character_set_client = gbk;
    SET character_set_results = gbk;
    SET character_set_connection = gbk;
SET NAMES GBK;  -- 相当于完成以上三个设置

```

## 表管理

### 查看表结构

```sql
show tables [like 'pattern'];
show tables from <db name>
describe <table_name> [like 'pattern']       # 显示表结构
desc <table>
explain <table>

```

### 创建删除表

```sql
create table student(
     sid int,         --# 整数
     sname varchar(20), --#  字符串
     sage int
 );

DROP TABLE table_name

```

### 修改表结构

```sql
-- 修改表的字段机构（13.1.2. ALTER TABLE语法）
ALTER TABLE 表名 操作名
-- 操作名
    ADD[ COLUMN] 字段定义       -- 增加字段
        AFTER 字段名          -- 表示增加在该字段名后面
        FIRST               -- 表示增加在第一个
    ADD PRIMARY KEY(字段名)   -- 创建主键
    ADD UNIQUE [索引名] (字段名)-- 创建唯一索引
    ADD INDEX [索引名] (字段名) -- 创建普通索引
    DROP[ COLUMN] 字段名      -- 删除字段
    MODIFY[ COLUMN] 字段名 字段属性     -- 支持对字段属性进行修改，不能修改字段名(所有原有属性也需写上)
    CHANGE[ COLUMN] 原字段名 新字段名 字段属性      -- 支持对字段名修改
    DROP PRIMARY KEY    -- 删除主键(删除主键前需删除其AUTO_INCREMENT属性)
    DROP INDEX 索引名 -- 删除索引
    DROP FOREIGN KEY 外键    -- 删除外键

-- 修改表元数据
alter table <name> engine=MyISAM

rename table <old_name> to [db_name.]<new_name>

-- 删除表
    DROP TABLE[ IF EXISTS] 表名 ...
-- 清空表数据
    TRUNCATE [TABLE] 表名
-- 复制表结构
    CREATE TABLE 表名 LIKE 要复制的表名
-- 复制表结构和数据
    CREATE TABLE 表名 [AS] SELECT * FROM 要复制的表名

```

### 操作索引

```sql
-------------添加-----------------

-- alter table用来创建普通索引、UNIQUE索引或PRIMARY KEY索引
--PRIMARY KEY索引仅是一个具有名称PRIMARY的UNIQUE索引

ALTER TABLE table_name ADD INDEX [index_name] (column_list)

ALTER TABLE table_name ADD UNIQUE (column_list) -- 索引不包含重复值

ALTER TABLE table_name ADD PRIMARY KEY (column_list) -- 索引不包含重复值

-- create index可对表增加普通索引或UNIQUE索引
--不能用CREATE INDEX语句创建PRIMARY KEY索引

CREATE INDEX index_name ON table_name (column_list)

CREATE UNIQUE INDEX index_name ON table_name (column_list)

-- 或者在创建表时就添加索引
create table user_info(
  id int unsigned not null auto_increment,
  name varchar(64) not null DEFAULT '',
  email varchar(64) not null DEFAULT '',
  password ...,
  birth date DEFAULT null,
  address varchar(255) not null DEFAULT '',
  city varchar(64) not null  DEFAULT '',
  staff_id tinyint unsigned not null DEFAULT '0',
  primary key (id),
  UNIQUE key email (email),
  key staff_id (staff_id),
  key staff_id_2 (staff_id, city, address)
) engine=InnoDB

-----------------------------

----------删除-----------

DROP INDEX index_name ON talbe_name

ALTER TABLE table_name DROP INDEX index_name

ALTER TABLE table_name DROP PRIMARY KEY --如果没有创建PRIMARY KEY索引，但表具有一个或多个UNIQUE索引，则MySQL将删除第一个UNIQUE索引。

------------查看-------------------

show index from tblname;

show keys from tblname;

```

## 数据管理

### 增删改数据

```sql
-- 插入所有字段。一定依次按顺序插入
INSERT INTO student VALUES(1,'张三','男',20);
-- 插入部分字段
INSERT INTO student(id,NAME) VALUES(2,'李四');


-- 修改所有数据（建议少用）
UPDATE student SET gender='女';
-- 带条件的修改（推荐使用）
UPDATE student SET gender='男' WHERE id=1; -- 修改id为1的学生，修改性别为男
-- 修改多个字段,注意: SET 字段名=值,字段名=值,....
UPDATE student SET gender='男',age=30 WHERE id=2;


-- 删除所有数据
DELETE FROM student;
-- 带条件的删除
DELETE FROM student WHERE id=2;
--  另一种方式: truncate
-- delete from: 可以全表删除      1)可以带条件删除  2）只能删除表的数据，不能删除表的约束     3)使用delete from删除的数据可以回滚（事务）
-- truncate table: 可以全表删除   1）不能带条件删除 2）即可以删除表的数据，也可以删除表的约束 3）使用truncate table删除的数据不能回滚
TRUNCATE TABLE student;


```

### 查询数据

```sql
SELECT * FROM student;

SELECT id,name,gender FROM student;

-- 需求： 重命名column
SELECT id,name,gender,age AS '年龄'  FROM student;

-- 取出重复记录，需求： 查询学生的性别     男 女
SELECT DISTINCT gender FROM student;

----------------------
-------- 条件查询 ------
---------------------

---------------逻辑条件-----------

-- 需求： 查询id为2，且姓名为李四的学生
SELECT * FROM student WHERE id=2 AND NAME='李四'; -- 交集
-- 需求： 查询id为2，或姓名为张三的学生
SELECT * FROM student WHERE id=2 OR NAME='张三'; -- 并集

--------------比较条件------------------------

-- 需求： 查询servlet成绩大于70分的学生
SELECT * FROM student WHERE servlet>70;
-- 需求： 查询jsp成绩大于等于75，且小于等于90分的学生
SELECT * FROM student WHERE jsp>=75 AND jsp<=90;
-- 另一个语法
SELECT * FROM student WHERE jsp BETWEEN 75 AND 90; -- (包前包后)
-- 不等于
SELECT * FROM student WHERE gender<>'男';

---------------------判空 (null vs. '')--------------------

-- null vs  空字符串
-- null：表示没有值
-- 空字符串：有值，即‘’
-- 判断null
SELECT * FROM student WHERE address IS NULL ;
-- 判断空字符串
SELECT * FROM student WHERE address='';
-- 需求： 查询地址为空的学生（包括null和空字符串）
SELECT * FROM student WHERE address IS NULL OR address=''; -- （包括null和空字符串）

-- 需求： 查询有地址的学生(不包括null和空字符串)
SELECT * FROM student WHERE address IS NOT NULL AND address<>'';

-----------------模糊条件： like-------------------------

-- 通常使用以下替换标记：
-- % : 表示任意个字符
-- _ : 表示一个字符
-- 需求： 查询姓‘张’的学生
SELECT * FROM student WHERE NAME LIKE '张%';
-- 需求： 查询姓‘李’，且姓名只有两个字的学生
SELECT * FROM student WHERE NAME LIKE '李_';

----------------------------------------------------
----------------聚合查询（使用聚合函数的查询）----------
----------------------------------------------------

-- 常用的聚合函数： 
-- sum()  
-- avg()  
-- max()  
-- min()  
-- count() - count（）函数统计的数量不包含null的数据
-- 使用count统计表的记录数，要使用不包含null值的字段



----------------------------------------------------
----------------分页查询（limit 起始行,查询几行）----------
----------------------------------------------------

-- 起始行从0开始
-- 起始行 == (当前页-1)*每页显示多少条, 所以 分页必须知道：当前页 , 每页显示多少条
-- 分页查询当前页的数据的sql: SELECT * FROM student LIMIT (当前页-1)*每页显示多少条,每页显示多少条;

-- 需求： 查询第1,2条记录（第1页的数据）
SELECT * FROM student LIMIT 0,2;
-- 查询第3,4条记录（第2页的数据）
SELECT * FROM student LIMIT 2,2;
-- 查询第5,6条记录（第3页的数据）
SELECT * FROM student LIMIT 4,2;
-- 查询第7,8条记录 (没有记录不显示)
SELECT * FROM student LIMIT 6,2;


----------------------------------------------------
---------------查询排序（order by ）----------
----------------------------------------------------

-- 语法 ：order by <字段> asc/desc
-- asc: 顺序，正序。数值：递增，字母：自然顺序（a-z）
-- desc: 倒序，反序。数值：递减，字母：自然反序(z-a)

-- 默认情况下，按照插入记录次序排序
SELECT * FROM student;

-- 需求： 按照id顺序排序
SELECT * FROM student ORDER BY id ASC; -- 正序
SELECT * FROM student ORDER BY id; -- 默认正序

SELECT * FROM student ORDER BY id DESC;-- 反序

-- 多个排序条件
-- 需求： 按照servlet正序，按照jsp的倒序
SELECT * FROM student ORDER BY servlet ASC,jsp DESC;


----------------------------------------------------
------------------------分组查询(group by)----------
------------------------k----------------------------


-- 需求： 查询男女的人数
-- 预期结果：
  --  男   3
  --- 女   2
  -- 1) 把学生按照性别分组(GROUP BY gender)
  -- 2) 统计每组的人数(COUNT(*))
SELECT gender,COUNT(*) FROM student GROUP BY gender;

--------------- 分组查询后筛选-----------------

-- 需求： 查询总人数大于2的性别
-- 1) 查询男女的人数
-- 2）筛选出人数大于2的记录(having)
--- 注意： 分组之前条件使用where关键字，分组之前条件使用having关键字
SELECT gender,COUNT(*) FROM student GROUP BY gender HAVING COUNT(*)>2;


----------------------------------------------------
-----------------------关联查询(多表查询)----------
----------------------------------------------------

-----------------------内连接查询 join, INNER join xxx ON yyy---------------------------

-- 只有满足条件的结果才会显示(使用最频繁)
SELECT empName,deptName       -- 2）确定哪些哪些字段
  FROM employee,dept          -- 1）确定查询哪些表
  WHERE employee.deptId=dept.id  -- 3）表与表之间连接条件
  
-- 内连接的另一种语法
SELECT empName,deptName
  FROM employee
  INNER JOIN dept
  ON employee.deptId=dept.id;
  
-- 使用别名
SELECT e.empName,d.deptName
  FROM employee e
  INNER JOIN dept d
  ON e.deptId=d.id;

---------------------------左[外]连接查询 left/right [outer] join------------------------------------------

-- 使用左边表的数据去匹配右边表的数据，右边表如果符合连接条件的记录则显示，如果不符合连接条件则显示null
 -- （注意： 左外连接：左表的数据一定会完成显示！）
SELECT d.deptName,e.empName
  FROM dept d
  LEFT OUTER JOIN employee e
  ON d.id=e.deptId;
-- 预期结果：
 --  软件开发部  张三
 --  软件开发部  李四
 --  应用维护部  王五
 --  秘书部      陈六
 --  总经办      null 

--------------------------------右[外]连接查询---------------------------------

-- 使用右边表的数据去匹配左边表的数据，如果符合连接条件的结果则显示，如果不符合连接条件则显示null
 -- （注意： 右外连接：右表的数据一定会完成显示！）
SELECT d.deptName,e.empName
  FROM employee e
  RIGHT OUTER JOIN dept d
  ON d.id=e.deptId;

--------------------------自连接查询----------------------------

-- 需求：查询员工及其上司
-- 预期结果：       
  -- 张三    null
  -- 李四    张三
  -- 王五    李四
  -- 陈六    王五
SELECT e.empName,b.empName
  FROM employee e 
  LEFT OUTER JOIN employee b
  ON e.bossId=b.id;

----------------------------- 合并两个sql的结果(字段个数, 类型必须相同) union, union all ------------------
-- union 禁止重复行, 同时进行默认排序, 因为要进行重复行扫描, 效率低
-- union all 允许重复行, 不排序, 但是效率高


----------------------- intersect 交集, 不允许重复行, 进行默认排序-----------------
--------------------minus 差集, 不允许重复行, 进行默认排序 ---------------------

```




# 分库分表和读写分离-表设计

outline-about-db-design-note#数据库分库分表

## 读写分离配置

https://veevv.com/2019/04/24/mysql_main_slave_config/
TODO

# MySQL

## MySQL的数据类型

### 选择合适的数据类型

- 范围尽量小
- 尽量简单 (如使用专门的类型而不是varchar来存储日期时间; 整型来存ip)
- 避免默认值为null (因为 null 更占用空间, 含有null的列生成的索引也更大)


### 常见数据类型

[MySQL 常见数据类型及其长度](https://www.cnblogs.com/ghjbk/p/6681470.html)
[MySQL-Java-类型对应](http://www.cnblogs.com/JemBai/archive/2009/08/20/1550683.html)

char是一种固定长度的类型，varchar则是一种可变长度的类型，它们的区别是：char(M)类型的数据列里，每个值都占用M个字节，如果某个长度小于M，MySQL就会在它的右边用空格字符补足, 当然查询出来会将末尾空格截断丢弃在返回结果; 在varchar(M)类型的数据列里，每个值只占用刚好够用的字节再加上一个用来记录其长度的字节 , varchar 类型虽然在磁盘占用上是变长的, 但是在内存消耗上每个单位是固定的, 所以 varchar(M) 中的 M 还是要尽可能小

int(1), int(10) 范围, 磁盘占用没有不同, 只是显示出来的长度不同, 和 zerofill 一起用

blob和text: blob 是存储大量的二进制字符串, text是存储大量的字符串有排序规则和字符集

datetime和timestamp: 前者范围更大, 精度为 s, 如 2019-01-22 22:22:00; 后者只能存储1970到2038年的秒数, mysql提供 from_unixtime() 和 unix_timestamp() 函数互相转换; 后者占用空间更小; 可通过bigint 存储更小精度的时间.


### java jdbc 和 mysql 数据库的字段对应关系

[MySQL里的数据类型到底该选择哪种Java类型与之对应](http://www.cnblogs.com/waterystone/p/6226356.html)

### Java中几种Date相关数据类型的区别

有时候两种类型间涉及到互相转换, mybatis中提供许多内置的 TypeHander

[java.util.Date、java.sql.Date、java.sql.Time、java.sql.Timestamp区别和总结](https://langgufu.iteye.com/blog/2202744):  java.util.Date这个类是java.sql.Date,  java.sql.Time,  java.slq.Timestamp这三个类的父类。这三个类对java.util.Date类进行了包装。 java.sql.Date类屏蔽了java.util.Date类的时间有关的方法（形如：hh:mm:ss）,因此，不可以通过这个类访问时间有关的信息; Java.sql.Time类屏蔽了java.util.Date的日期有关的字段;  Java.sql.Timestamp字段则对java.util.Date这个类进行了扩充，它在java.util.Date类的基础上增加了毫秒的时间访问控制，因此，你可以通过getNanos（）方法去获取时间的毫微秒数, 也是为了和数据库中的Timestamp数据类型进行匹配


### 字段长度的意义and格式

ref：https://www.cnblogs.com/winner-0715/p/6761078.html

tinyint(1) 和 tinyint(4) 中的1和4并不表示存储长度，只有字段指定zerofill是有用，
如tinyint(4)，如果实际值是2，如果列指定了zerofill，查询结果就是0002，左边用0来填充


## MySQL存储过程

https://www.cnblogs.com/mark-chan/p/5384139.html

```sql
-- 定义分隔符 DELIMITER //
-- 还原默认分隔符, 默认分隔符 为 ";" DELIMITER ; 

-- 参数类型:
-- IN参数的值必须在调用存储过程时指定，在存储过程中修改该参数的值不能被返回，为默认值
-- OUT:该值可在存储过程内部被改变，并可返回
-- INOUT:调用时指定，并且可被改变和返回

-- 过程体
-- 过程体的开始与结束使用BEGIN与END进行标识。

-----------------------------------


DELIMITER // 
  CREATE PROCEDURE myproc(OUT s_out int)
    BEGIN
      SELECT COUNT(*) INTO s FROM students;
    END
    // -- procedure 结束
DELIMITER ;

---------------------------------------循环------------------------------

DELIMITER //
  CREATE PROCEDURE proc5()
    BEGIN
      DECLARE var INT;
      SET var=0;
      WHILE var<6 DO
        INSERT INTO t VALUES (var);
        SET var=var+1;
      END WHILE ;
    END;
  //
DELIMITER ;
------------------------------------------
DELIMITER //
  CREATE PROCEDURE proc7 ()
    BEGIN
      DECLARE v INT;
      SET v=0;
      LOOP_LABLE:LOOP -- LOOP_LABLE 是一个标签
        INSERT INTO t VALUES(v);
        SET v=v+1;
        IF v >=5 THEN
          LEAVE LOOP_LABLE; -- 离开 loop
        END IF;
      END LOOP;
    END;
  //
DELIMITER ;

```

## 存储引擎

### MyisAM与InnoDB相比较

- InnoDB支持事务，MyisAM不支持；
- MyisAM支持全文索引（FULLTEXT）、压缩索引，InnoDB不支持；
- MyISAM 不支持聚簇索引, 它的主键索引就是名为 primary 的普通索引, InnoDB支持聚簇索引;
- 两者索引文件不同 - https://www.jianshu.com/p/e110359bd29a
- 支持的锁不同， My 支持 表级锁， In 支持 行级锁

## 范式and反范式

三范式

1. 字段不可再分
2. 要有主键, 其他字段完全依赖此主键
3. 非主键字段必须互不依赖

优缺点:

- 写操作更方便, 更好维护
- 会创建更多的表, 需要更多的 join, 更多的索引

反范式:

两张表合并到一张表, 引入冗余数据, 换取更少的join

或者两张表中拥有相同的字段, 换取更高效的查询, 但是更新操作将比较麻烦

## 汇总表and缓存表

//todo

## 优化 SQL

https://www.iteye.com/blog/nopainnogain-667595
http://www.chengxy-nds.top/2020/05/22/%E4%B8%8D%E4%BC%9A%E7%9C%8B%20Explain%E6%89%A7%E8%A1%8C%E8%AE%A1%E5%88%92%EF%BC%8C%E5%8A%9D%E4%BD%A0%E7%AE%80%E5%8E%86%E5%88%AB%E5%86%99%E7%86%9F%E6%82%89%20SQL%E4%BC%98%E5%8C%96/


### 基本原则

开发涉及 SQL 的业务都会去本地环境跑一遍 SQL，用 explain 去看一下执行计划，看看用没用到相关的索引，然后再去线上环境跑一下看看执行时间

MySQL 最新版是有缓存的, 执行测试SQL需要 加上 SQL NoCache 去跑以排除缓存干扰

- 分批次处理代替一次性处理

    因为一次执行涉及到大量数据行的SQL可能堵死其他SQL

    可以将大量数据行分多批次处理, 如一次处理 10000 行, 可以使用 limit , 分为多段处理

- 使用 union 优化 or, 不等于

    在Innodb引擎下or无法使用组合索引, 不等于操作符是永远不会用到索引的

    可以将a<>0 改为 a>0 or a<0, 或者进一步, 改成两个select 然后  union all

- 缩小 ID 查找范围来优化 limit 

    limit用于分页查询时越往后翻性能越差，解决的原则：缩小扫描范围

    如: `select * from orders where id > (select id from orders order by id desc  limit 1000000, 1) order by id desc limit 0,10`

### 执行计划 explain

```sh
1.查询t_base_user
select * from t_base_user where name="andyqian";

2.查看上述语句的执行计划
explain select * from t_base_user where name="andyqian";


#######################

select_type:
查询类型,常见可选值有: 
SIMPLE(简单查询), 
PRIMARY(SQL 中包含复杂的子查询，此时最外层查询标记为该值) ，
SUBQUERY(SQL 在 select 或者 where 里包含的子查询被标记为该值)
derived 是 SQL 中 from 子句中包含的子查询被标记为该值，MySQL 会递归执行这些子查询，把结果放在临时表。下图展示了上述两种类型。
dependent subquery：子查询中的第一个 select，取决于外侧的查询，一般是 in 中的子查询

table: (数据所在表)
表示输出的行数据所在表

type: (表的连接类型，其性能由高到低排列为 system，const，eq_ref，ref，range，index 和 all。)
表示访问类型,有很多种访问类型, ALL(全表扫描), index(索引扫描),range(范围扫描, 比如条件为between..and..),ref (非唯一索引扫描),eq_ref(唯一索引扫描,),(const)常数引用比如主键索引, 
system 表示表只有一行记录，相当于系统表
const 通过索引一次就找到，只匹配一行数据，用于常数值比较PRIMARY KEY 或者 UNIQUE索引。
eq_ref 唯一性索引扫描，对于每个索引键，表中只有一条记录与之匹配，常用于主键或唯一索引扫描
ref 非唯一性索引扫描，返回匹配某个单独值的所有行，和 eq_ref 的区别是索引是非唯一索引
range 只检查给定范围的行，使用一个索引来选择行，当使用 =, between, >, <, 和 in 等操作符，并使用常数比较关键列时
index 与 ALL 类型类似，唯一区别就是只遍历索引树读取索引值，比 ALL 读取所有数据行要稍微快一些
ALL 全表扫描，通常情况下性能很差

possible_keys: 
查询可能走的索引,(如某些字段上索引的名字)这里提供的只是参考,而不是实际走的索引
如果该列为 NULL，则没有使用相关索引。需要检查 where 子句条件来创建合适的索引

key : 
显示MySQL实际使用的索引,其中就包括主键索引(PRIMARY),或者自建索引的名字, 如果没有选择索引，则值为 NULL

key_len : 表示索引所使用的字节数

ref :
连接匹配条件,表示使用其他表的哪个列或者常数来从表中选择行
如果走主键索引的话,该值为: const, 全表扫描的话,为null值

rows :
扫描行数,也就是说，需要扫描多少行,才能获取目标行数,一般情况下会大于返回行数。通常情况下,rows越小,效率越高, 大部分SQL优化，都是在减少这个值的大小。
这个值仅仅是一个近似值, 因为MySQL是这样得出这个值的: MySQL 中数据的单位都是页，MySQL 又采用了采样统计的方法，采样统计的时候，InnoDB 默认会选择 N 个数据页，统计这些页面上的不同值，得到一个平均值，然后乘以这个索引的页面数

Extra :
# 使用EXPLAIN判断SQL语句是否合理使用索引，尽量避免extra列出现：Using File Sort，UsingTemporary
这个属性非常重要,该属性中包括执行SQL时的真实情况信息 .
"using where"，子句用于限制哪一行。也就是读取数据后使用了 Table Filter 进行过滤
"Using temporary", 使用临时表 .
"using filesort": 使用文件排序. 
"using index", 使用了覆盖索引
```



## 怎么加快alter table操作

这是一种hack 方法, 通常使用常规方法

//todo

## 怎么创建高性能索引

https://mp.weixin.qq.com/s?__biz=Mzg2OTA0Njk0OA==&mid=2247488345&idx=2&sn=cf64cd654608bc4db8f04e9012c32742&chksm=cea25c92f9d5d584cd89fc48d2c0cf1b09c1a9b7b34602018f146cb74b583f9bafe62f1920e0&mpshare=1&scene=24&srcid=&sharer_sharetime=1592801583067&sharer_shareid=ea500fdbb56041ff403167241e1dfbd9#rd



### 什么是索引

http://imysql.com/2017/08/08/quick-deep-into-mysql-index.shtml
https://mp.weixin.qq.com/s?__biz=MzI2NDU3OTg5Nw==&mid=2247483736&idx=1&sn=e3fa0bf61b77c9ca12ee1d295c51313a&chksm=eaab3f0edddcb618d38fabaccfd6a7cd18df0f2d77dfeeaa1403bae02c7eb588e29607149dcc&scene=21#wechat_redirect

一种存储引擎中的 【数据结构】，用于快速找到某条记录

在对查询性能进行优化时候， 最先考虑 索引优化

索引建在 “某一列” or “多列” 上；eg：“select colum_a from table_a where column_b = 5”, 如果在列 colum_b 上建有索引, 则MySQL使用该索引找到该列值为 5 的所有行 (in other words, MySQL会先在索引上按照值进行查找, 然后返回所有包含该值的数据行)

对于 "多列" 索引, 列的 "顺序" 很重要, MySQL只能高效使用索引的 "最左前缀列", 创建一个包含两列的索引 and 创建两个单独的只包含一列的索引 是不同的

### 索引的类型

#### b-tree 索引

![alt](Snipaste_2019-01-18_14-13-30.png)

最常见， 使用 b+tree 数据结构来存储数据;

b+tree 索引是顺序存储索引列的数据的， `很适合查找范围`， 比如查找 “以a ~ b 开头的名字”

InnoDB (默认引擎) 显式支持

eg：

```sql
create table people (
  last_name varchar(50) not null,
  first_name varchar(50) not null,
  birth date not null,
  gender enum('m', 'f') not null,
  key(last_name, first_name, birth) -- 定义索引时列顺序, 代表排序优先级
)
```

索引中包含了三列数据, 当 last_name, first_name 都同样时, 根据 birth 决定索引中的排序

##### 满足哪些要求才能够使用到b-tree索引呢

场景: 适合 `全键值`, `键值范围`, `键前缀 (最左前缀)`, `排序` 查找 (也就是 对于 sql 的要求)

- 全值匹配 - 和index 中的所有列进行匹配

- 匹配最左前缀(最左列) - 即只使用 index 的第一列 (📌: 如果不是通过 index中的最左列查找, 则无法使用 index, 如: 查找特定 birth 的人; )

    - 匹配列前缀(最左列开头部分) - 即匹配最左列的开头的部分, 如:查找 last_name 以xxx开头的人 (📌: 相反的 也无法在查找 last_name 以xxx结尾的行时使用index)

    - 匹配范围 - 即匹配最左列的某一范围(如 查找 last_name 在 Tom 和 Jerry 之间的人)

- 精确匹配某一列and范围匹配另外一列 - 如: 查找 "last_name=xxx, 而 first_name在xxx~yyy之间的人" 即 全值匹配 last_name, 范围匹配 first_name

- 只访问 index 的查询 - 即查询只需要访问index 而无需访问数据行, 下文提及 (即 覆盖索引)

有哪些限制呢

- 如果不按照最左列查找, 则无法使用index, 同理 不按照最左列开头部分查找, 无法使用index

- 不能跳过 index 中的列, 如: 查找 last_name=xxx, birth=yyy 而不指定 fist_name, 那么只能使用 index 的最左列即 last_name

- 如果index中有某个列有范围查询 (比如: <, >, IN(...)), 那么右边列索引失效, 如: `where last_name=xxx and first_name like 'J%' and birth=yyy`这个查询只能使用index 中的 last_name 和 fist_name, birth 会失效 (📌: 如果这里的范围查询的范围有限, 可以 用多个"=" 条件代替范围条件 来优化)

- 如果在SQL中对索引字段进行了函数运算, 不会走索引. 比如 select _ from tradelog where id + 1 = 10000 就走不上索引，select _ from tradelog where id = 9999 就可以

    - 隐式类型转换

        对于 select * from t where id = 1, 如果 id 是字符类型的，1 是数字类型的，你用 explain 会发现走了全表扫描，根本用不上索引

        因为 MySQL 底层会对你的比较进行转换，相当于加了 CAST( id AS signed int) 这样的一个函数

    - 隐式字符编码转换

        如果两个表的字符集不一样，一个是 utf8mb4，一个是 utf8，因为 utf8mb4 是 utf8 的超集，所以一旦两个字符比较，就会转换为 utf8mb4 再比较, 转换的过程相当于加了 CONVERT(id USING utf8mb4)函数

    

##### 两种实现方式

mysql中普遍使用B+Tree做索引，但在实现上又根据`聚簇索引`和`非聚簇索引`而不同。

- 聚簇索引 - 就是指主索引文件和数据文件为同一份文件, B+Tree的叶子节点上的data就是数据本身，key为主键 (主要用在Innodb存储引擎中)

- 非聚簇索 - 就是指B+Tree的叶子节点上的data，并不是数据本身，而是数据存放的地址 (主要用在MyISAM存储引擎中)

  非聚簇索引比聚簇索引多了一次读取数据的IO操作，所以查找性能上会差。

#### 哈希索引

基于哈希表实现, 只有`精确匹配索引所有列`的查询才有效

对于每一行数据, db engine 都会对所有 indexed column 计算一个 hashcode, 作为 key 存入 哈希索引中, 该行数据的指针作为value存入 索引

只有 memory 引擎显式支持

eg:

```sql
create table hashindex (
  fname varchar(50) not null,
  lname varchar(50) not null,
  key using hash(fname)
) engine=memory

```

- index 自身 只需要存储hashcode 和 行指针, 体积小; 反过来每次查询必须读取行数据, 因为 index 中没有存储实际值
- index不是按照 index column 中的数据的顺序存储的, so无法用于排序
- 不支持部分 index column 的查询, 因为 hashcode 是按照 全部的 index column计算的
- 只支持等值查询, 包括 "=", "in(xxx)", 不支持范围查询, 如 "<", ">"

##### 创建自定义hash索引

在 b-tree 的基础上创建一个伪hash index , 和真正的 hash index 不是一回事,  因为还是使用 b-tree 进行查找, 但是不使用数据本身创建索引而是使用 hashcode 创建索引

eg: 需要存储大量url, 并更具url查找, 这是如果针对 url 创建索引, 势必会index 会占用大量空间, 因为 url 都很长, 这是查找sql `select id from url_table where url = ""www.google.com `; 创建自定义hash索引这样做: 删除 原来 url 列上的index, 新增一列 "url_crc" 并索引, 使用 crc32 做哈希函数, 此时查找sql `select  id from url_table where url = ""www.google.com and url_crc = crc32("www.google.com")`;

这种方案的缺陷是需要维护哈希值;

注意不要使用 sha1(), md5() 这种作为哈希函数, 因为他们都是强加密, 生成的 hashcode 很长, 会浪费大量空间, 比较时也慢

#### 空间数据索引

MyISAM 引擎支持 空间index, 可作为地理数据存储

#### 全文索引

查找文本中的关键词, 而不是直接比较 index中的值, 不是简单的 where条件匹配, 下文详述



### 索引有什么优缺点

好处:

- 索引中存储了实际的列值, 所以有些查询只需要使用索引就能完成全部查询
- b-tree 有序, 所以可以加速 order by , group by 操作
- 因为 b-tree 有序, 相关数据在磁盘上会放在一起, 加快磁盘i/o

缺点: 

- 创建和维护索引需要耗费资源, 时间

### 创建索引需要遵循哪些原则

怎么评价索引好不好呢

三星系统(three-star-system):索引将相关记录放到一起获取第一星, 索引中的数据顺序和查找中的排列顺序一致则获得第二星, 如果索引中包含了查询中需要的所有列则获得第三星

- 相关数据在一起
- 顺序一致
- column一致

#### 始终使用最简化的条件表达式

索引列不能是 表达式的一部分, 不能是函数的参数, index不会生效, 如: `select xxx from yyy_table where zzz_column + 1 = 4 `,  index不会生效, zzz_column 必须独立使用, 应该改为 `select xxx from yyy_table where zzz_column = 3`;

`select xxx from yyy_table where func(zzz_column) = 4` index 无效


#### 使用前缀索引and什么是索引选择性

有时候索引列的长度很长, 这会使得 index 变得又大又慢, 一种解决方式是: 把字段 hash 为另外一个字段存起来，每次校验 hash 就好了，hash 的索引也不大

此外还可以做哪些工作? 采用倒序，或者删减字符串这样的操作去提高区分度

- 比如本来是 www.aobing@qq,com 其实前面的www.基本上是没任何区分度的, 可以 substring()函数截取掉前面的，然后建立索引
- 如人的身份证都是区域开头的，同区域的人很多，那怎么做良好的区分呢？REVERSE（）函数翻转一下，区分度可能就高了

"`索引的选择性`": 不重复的索引值数(基数)/表总记录数, 选择性越高, 则查询越快; 唯一索引的选择性=1, 速度最快 (因为选择性更高的索引列可以让MySQL在查询时过滤掉更多的数据行)

`可以选取索引列的部分字符进行索引`, 减少index体积的同时也可以保证合适的索引选择性

具体怎么做呢, 

```sql
-- 有这样的表
create table city (
  name varchar(50) not null
)

--------------

-- 查看出现频次最高的10个城市
select count(*) as count,
  name 
from city 
group by name
order by count desc 
limit 10;

-- 试着只取城市名的前3个字符, 计算频次, 此时频次明显升高
select count(*) as count,
  left(name, 3) as pref
from city
group by pref
order by count desc
limit 10;

-- 然后尝试新的前缀长度, 取前5, 7, 9 ..., 直到前缀频次接近完整名称的频次 (每次取更长的
--前缀, 频次都会下降, 不断接近完整城市名的频次, 取下降不明显的那一次前缀长度)
...

-- 创建前缀索引
alter table city add key (name(7));

-----------------------------------------------------

-- 也可以一步到位, 直接算出不同长度前缀的平均选择性, 最好和上面的结合使用
select count(distinct left(name, 3))/count(*) as pref3,
  count(distinct left(name, 4))/count(*) as pref4,
  count(distinct left(name, 5))/count(*) as pref5,
  count(distinct left(name, 6))/count(*) as pref6,
  count(distinct left(name, 7))/count(*) as pref7 -- 发现 前缀取到7时, 再增加前缀长度, 选择性增幅很小了
from city


```

#### 联合索引也许好过多个单列索引

- sql 中有多个 and 时 (即MySQL对多个单列索引做相交操作), 意味着需要一个多列索引而不是多个单列索引
- 如果只有单列索引, 且sql 中有多个or (联合操作), 需要耗费大量组员去做数据的合并, 此时推荐将sql拆分成多个sql然后通过 union all 合并结果

如果 explain 中有 "Extra: Using union(index_1, index_2...)", 证明存在索引的合并, 说明索引建立的不好

#### 选择合适的索引列顺序 最左前缀匹配原则

首先需要遵守最左前缀匹配原则: 对于联合索引, 条件字段需要从最左边开始


另外, 将 "选择性"最高的列放在索引的最前列 是很好的策略, 这时index 作用主要用于优化 where 条件查找

eg:

```sql
-- 有如下表
create table test (
  staff_id varchar(50) not null,
  customer_id varchar(50) not null
)

-- 现在有一个这样的查询, 怎么创建索引呢
select * from test where staff_id = 2 and customer_id = 666;

-- 考虑过程是这样的:
--where 后跟多个字段, 必定需要创建多列index, 然后, 先看看每个 column 的 选择性如何
--
select count(distinct staff_id)/count(*) as staff_id_selectivity,
  count(distinct customer_id)/count(*) as customer_id_selectivity,
  count(*)
from test;
-- 结果:
--staff_id_selectivity customer_id_selectivity count(*)
--0.0001               0.0343                   10000
-- customer_id_selectivity 更高, 所以这个字段在 index 最前列
alter table test add key(customer_id, staff_id);

```

#### 多利用聚簇索引

https://www.jianshu.com/p/e110359bd29a

![alt](Snipaste_2019-01-18_14-19-25.png)

典型的 b+tree 结构, 节点页只有主键值作 查询引导, 叶子页中存有全部行数据, 因此在sql 中根据 主键查找总是最快的, 因为用到了 聚簇索引;

并非新的单独的索引类别, 只是一种`数据存储方式`: 索引和数据行 存储在同一个b+tree结构中 (一般的索引,数据行都是存储在单独的文件, b+tree 结构的节点中存储的是数据行的内存地址)

聚簇索引的 数据行存储在索引的 叶子页 (leaf page), 节点页均都只存储 indexed column (一般为主键); 而普通索引中的节点页, 叶子页都存有 indexed column 的值

每个表都有一个聚簇索引（clustered index, 就是主键索引 ），除此之外的表上的每个非聚簇索引都是`二级索引`，又叫辅助索引（secondary indexes）; `二级索引的叶子节点中保存的不是行数据的磁盘地址, 而是行的主键`, 

带来的好处是:InnoDB在移动行时, 二级索引不必改动, 因为二级索引叶子节点存的是主键值而不是磁盘地址

一个表只有一个聚簇index, 一般为主键; 如果没有定义主键, InnoDB会选择一个非空列的index代替, 如果也没有not null column 的 index, InnoDB会自己定义一个值rowid作为主键, 使其为聚簇索引

优缺点:

- 优点:根据主键查找迅速, 因为不用出索引文件
- 缺点:如果碰到不规则的数据插入, 造成频繁的页分裂, 会使得插入速度变慢, 同时索引占用更多的磁盘
- 缺点:二级索引访问需要两次查找, 而不是一次


据此有什么 best practice呢: 聚簇索引列(通常为主键) 使用递增的整型最好 (分布式场景下有需要另外考虑, 参考 "表设计笔记#全局主键生成策略")

聚簇索引使用递增整型和使用uuid有什么具体不同呢:

![alt](Snipaste_2019-01-18_15-37-31.png)![alt](Snipaste_2019-01-18_15-38-42.png)


#### 多利用索引覆盖

如果一个 index 包含(也称覆盖)要查询的所有字段值, 就称为 覆盖索引 ; 只能使用 b-tree类型的 index, 其他索引类型如哈希索引, 索引文件中没存实际数据行

`通过索引直接获取数据返回而无需查询真实的数据行`, 这一现象称为索引覆盖

explain的extra列 "Extra: Using index"

例如: 对于索引 (store_id, film_id), 有sql "select store_id, film_id from xxx_table;", 会有索引覆盖

#### 使用索引进行排序

如果一次查询没有产生索引覆盖， 那么每次读取一条索引记录都需要到数据表中查一次真实数据行， 这基本都属于`随机io`， 而全表扫描基本是顺序io， 因此， 非覆盖索引数据查询慢于全表扫描

如果 explain 出来的结果的 type:index, 说明 sql 使用了 索引扫描来进行排序

- 📌 只有当indexed column 顺序和 order by 后面的column顺序 (或者 where 和 order by 两者的column拼接后的顺序)一致, 且按照 column 都为 正序/倒叙 排序时, 才能使用索引进行排序; 
- 📌 如果多张表join, 只有 order by 后面的 column 都是第一张表中的 column 时才能使用index排序;
- 📌对查询sql也有要求: 必须满足索引的最左前缀要求. 但是也有特例: 有一种特例order by 子句可以不遵循 index 的最左前缀要求-----那就是 "前导列" 为常量的时候 比如 `select a, b, c from xxx_table where a = 'yyy' order by b, c`, 有索引 index_a(a, b, c), 那么这个sql虽然 order by 没有满足index最左前缀, 但是 前导列 "a" 为一个常数yyy, 所以仍然可以使用 index 排序

#### MyISAM引擎支持索引压缩

可以节省大量磁盘空间, 是在 cpu内存资源和 磁盘之间做取舍;

原理类似 单词查找树(trie tree), 索引中后一个索引值相对于前一个索引值重复的前缀字符串会被压缩 (也就是说, 后一个值依赖前一个值), 因此扫描必须顺序进行, order by 操作 顺序快, 逆序慢

对于 cpu密集型app, 扫描需要随机查找, 压缩索引使得随机查找更慢

对于 io密集型app, 有大量的顺序查找, 优势大于劣势

#### 不要冗余的索引

创建 index(a, b), 然后创建 index_2(a), 这是冗余, 不可取, 后者包含的列是前者的前缀, 前者其实可以当作后者来用

创建 index(a, 主键), 冗余的, 不可取, 因为对于InnoDB来说, 主键列已经包含在二级索引中了;

#### 删除未使用的索引

//todo

#### 索引and锁

索引能够减少InnoDB访问的行数, 而引擎只有在访问行的时候才会加锁,所以`index 可以让查询锁定更少的数据行`

```sql
set autocommit=0;--关闭自动提交
begin; -- 开启一个事务, 不关闭
select a from xxx_table where a < 5 and a <> 1; 
-- 以上查询仅仅返回 2, 3, 4, 但实际上获取了 1~4 的排他锁; 即使没有返回 1, 1行也被锁了
-- 这是因为这个查询选择的执行计划是索引范围扫描, explain 一下, type: range
-- Extra: Using where 表示MySQL将在存储引擎返回数据行以后再执行where条件过滤

-- 新开一个连接
set autocommit=0;
begin;
select a from xxx_table where a = 1; -- 会被挂起, 因为 行1 被锁住了


```

#### 多利用 change buffer 所以不要使用唯一索引

- 当需要更新一个数据页时，如果数据页在内存中就直接更新，而如果这个数据页还没有在内存中的话, InooDB 会将这些更新操作缓存在 change buffer 中，这样就不需要从磁盘中读入这个数据页了, 加快了更新速度. 

    在下次查询需要访问这个数据页的时候，将数据页读入内存，然后执行 change buffer 中与这个页有关的操作 (merge操作)

    除了访问这个数据页会触发 merge 外，系统有后台线程会定期 merge。在数据库正常关闭（shutdown）的过程中，也会执行 merge 操作

- 什么条件下可以利用 change buffer呢? 

    唯一索引的更新就不能使用 change buffer，只有普通索引可以使用

    对于唯一索引来说，所有的更新操作都要先判断这个操作是否违反唯一性约束, 即表中是否已经存在待插入数据, 也就必须将数据页读取到内存中, 所以没法用到 change buffer 了

- change buffer 用的是 buffer pool 里的内存，因此不能无限增大，change buffer 的大小，可以通过参数 innodb_change_buffer_max_size 来动态设置，这个参数设置为 50 的时候，表示 change buffer 的大小最多只能占用 buffer pool 的 50%

- change buffer 使用场景

    在一个数据页做 merge 之前，change buffer 记录的变更越多（也就是这个页面上要更新的次数越多），收益就越大. 
    
    因此，对于写多读少的业务来说，页面在写完以后马上被访问到的概率比较小，此时 change buffer 的使用效果最好，这种业务模型常见的就是账单类、日志类的系统



### 索引案例demo

http://fangjian0423.github.io/2017/07/05/mysql-index-summary/


## binlog

references: https://www.cnblogs.com/xhyan/p/6530861.html //todo

订阅mysql binlog

### canal 增量日志订阅

https://blog.csdn.net/u013705066/article/details/86470900

https://www.jianshu.com/p/87944efe1005 //todo

## MySQL的锁机制

https://zhuanlan.zhihu.com/p/52678870


就是数据库为了保证数据的一致性，所设计的一种规则

- 乐观锁
- 悲观锁
  - 范围锁
    - 表锁
    - 行锁
      - 记录锁
      - 间隙锁
      - 临键锁 (MySQL行锁默认)
  - 属性锁
    - 共享锁(读锁, 许读不许写)
    - 排他锁(写锁, 读写均不可)
  - 状态锁
    - 意向共享锁
    - 意向排他锁

### 表锁

最大颗粒度的锁定机制，性能消耗最小，支持的并发度低，没有死锁问题

当更新数据库数据时，如果没有触发索引,即全表扫描，则会锁表，锁表后再对表做任何变更操作都会导致锁冲突

使用表级锁定的主要是MyISAM， 这是 mysql 自己实现的锁机制

myIsam 的读表操作，会获取表的读锁，会阻塞其他请求写表，不会阻塞都表；
myisam 的写表， 会获取表的写锁，会阻塞其他请求的读表， 写表； （请求获取 table read lock， table write lock 是有优先级的， 通过 `SET LOW_PRIORITY_UPDATES=1`, 使读比写的优先级高, 默认是写优先级高的）

加表锁 是 MyISAM 自动进行的，无需人为干预

如何优化表锁：也即是 如何让锁定时间尽可能的短， 减少大的复杂Query，将复杂Query分拆成几个小的Query分布进行



### 行锁

锁定粒度小，性能消耗大，支持并发度高，有死锁

当更新数据库数据时，如果触发索引，则会锁行，只有相关联的数据行被锁, 其他数据行可访问;

使用行级锁定的主要是InnoDB存储引擎 （不是 mysql 自己实现的锁机制）, 同时也支持表锁

InnoDB行锁是通过给索引上的索引项加锁来实现的


- 共享锁（S）， 和 s， IS 兼容， 和 x， IX 冲突
- 排它锁（X）， 和所有冲突
- 意向共享锁（IS）， 和   s， IS， IX 兼容， 和 X 冲突
- 意象排他锁（IX）， 和 IX， Is 兼容， 和 x， s 冲突

如果一个事务请求的锁模式与当前的锁兼容，InnoDB就将请求的锁授予该事务；反之，如果两者不兼容，该事务就要等待锁释放。

意向锁是InnoDB自动加的，不需用户干预。

对于UPDATE、DELETE和INSERT语句，InnoDB会自动给涉及数据集加排他锁（X)；对于普通SELECT语句，InnoDB不会加任何锁；事务可以通过以下语句显示给记录集加共享锁或排他锁：

```sql
-- 主要用在需要数据依存关系时来确认某行记录是否存在，并确保没有人对这个记录进行UPDATE或者DELETE操作。
共享锁（S）：SELECT * FROM table_name WHERE ... LOCK IN SHARE MODE

-- 如果当前事务也需要对该记录进行更新操作，则很有可能造成死锁，对于锁定行记录后需要进行更新操作的应用，应该使用SELECT... FOR UPDATE方式获得排他锁。
排他锁（X)：SELECT * FROM table_name WHERE ... FOR UPDATE

```

（1）在不通过索引条件查询的时候，InnoDB使用的是表锁，而不是行锁。

（2）由于MySQL的行锁是针对索引加的锁，不是针对记录加的锁，所以虽然是访问不同行的记录，但是如果是使用相同的索引键，是会出现锁冲突的。比如 索引列只是过滤条件的一部分

（3）当表有多个索引的时候，不同的事务可以使用不同的索引锁定不同的行，另外，不论是使用主键索引、唯一索引或普通索引，InnoDB都会使用行锁来对数据加锁。

（4）即便在条件中使用了索引字段，但是否使用索引来检索数据是由MySQL通过判断不同执行计划的代价来决定的，如果MySQL认为全表扫描效率更高，比如对一些很小的表，它就不会使用索引，这种情况下InnoDB将使用表锁，而不是行锁。因此，在分析锁冲突时，别忘了检查SQL的执行计划，以确认是否真正使用了索引。


如何解决死锁：

- InnoDB_lock_wait_timeout 设置锁等待超时
- 在业务逻辑中， 尽量约定以相同的顺序来访问表


#### 记录锁

等号条件查找, 命中索引, 查到一条记录

记录锁：记录锁锁的是表中的某一条记录，记录锁的出现条件必须是`精准命中索引`并且`索引是唯一索引`，如主键id

#### 区间锁(间隙锁)

Next-Key Locks

当我们查询数据用`范围查询`而不是相等条件查询时，查询条件`命中索引`， 并请求共享或排他锁时，此时就会将查询条件中的范围数据行以及整个 “范围” 对应的索引项加锁

目的：避免幻读 (间隙锁只会出现在`可重复读`的事务隔离级别中)。 innodb 会对 这个 “范围” 加锁， 不允许插入新数据进入这个范围


间隙锁又称之为区间锁，每次锁定都是锁定一个区间, 间隙锁所锁定的区间是一个`左开右闭`的集合

InnoDB除了通过范围条件加锁时使用间隙锁外，如果使用相等条件请求给一个不存在的记录加锁，InnoDB也会使用间隙锁。

#### 临键锁

范围查询条件`命中索引`, `有匹配到数据库记录`

间隙锁所锁定的区间是一个左开右闭的集合，而临键锁锁定是当前记录的区间和下一个记录的区间

临键锁锁定区间和查询范围`后匹配值`很重要，如果后匹配值存在，则只锁定查询区间，否则锁定查询区间和后匹配值与它的下一个值的区间。

## 多版本并发控制 mvcc


### 为什么需要 mvcc

为什么需要MVCC呢？数据库通常使用锁来实现隔离性。最原生的锁，锁住一个资源后会禁止其他任何线程访问同一个资源。但是很多应用的一个特点都是读多写少的场景，很多数据的读取次数远大于修改的次数，而读取数据间互相排斥显得不是很必要。所以就使用了一种读写锁的方法，读锁和读锁之间不互斥，而写锁和写锁、读锁都互斥。这样就很大提升了系统的并发能力。之后人们发现并发读还是不够，又提出了能不能让读写之间也不冲突的方法，就是读取数据时通过一种类似快照的方式将数据保存下来，这样读锁就和写锁不冲突了，不同的事务session会看到自己特定版本的数据。当然快照是一种概念模型，不同的数据库可能用不同的方式来实现这种功能。

### 什么是 mvcc

https://baijiahao.baidu.com/s?id=1629409989970483292&wfr=spider&for=pc

是mysql的InnoDb存储引擎实现隔离级别的一种具体的方式, 解决并发访问数据一致性问题

MVCC可以认为是行级锁的一个变种，但是他在很多情况下都避免了加锁操作， 采用乐观锁的策略，因此开销更低

每行数据都有隐藏的两个字段，trx_id和db_roll_ptr, trx_id表示最近修改的事务的id,db_roll_ptr指向undo segment中的undo log， 新增一个事务时事务id会增加

Undo log分为Insert和Update两种，delete可以看做是一种特殊的update，即在记录上修改删除标记。

update undo log记录了数据之前的数据信息，通过这些信息可以还原到之前版本的状态。

当进行插入操作时，生成的Insert undo log在事务提交后即可删除，因为其他事务不需要这个undo log。 （所以 面试题：同个事务中， 先插入， 成功， 后删除， 失败， 问插入是否会回滚？ 不会，因为 undo log 被删除了 ）

进行删除修改操作时，会生成对应的undo log，并将当前数据记录中的db_roll_ptr指向新的undo log。

对于修改， 每次更新数据都更新该版本号，修改时copy出当前版本。随意修改，各个事务之间互不干扰，保存时比较版本号，如果版本号一致则commit，不一致就rollback

MVCC只在 READ COMMITTED 和 REPEATABLE READ 两个隔离级别下工作。其他两个隔离级别够和MVCC不兼容, 因为 READ UNCOMMITTED 总是读取最新的数据行, 而不是符合当前事务版本的数据行。而 SERIALIZABLE 则会对所有读取的行都加锁。

## 事务

### 事务acid

捆绑在一起的一组业务逻辑操作, 要么全部成功，要么全部不成功。部分成功任然算失败, 且成功的部分需要 rollback

ACID: atomic (原子性) | consistency(一致性) | isolation (隔离性) | durability(持久性)

-   A: 构成事务的所有操作，要么都执行完成，要么全部不执行，不可能出现部分成功部分失败的情况

    部分操作失败则回滚整个事务. 基于 日志实现

-   C: 事务结束后, 数据库会从一个状态更新到新的状态, 新状态中的数据一致性约束没有被破坏

    什么是一致性约束: 主外键约束, 非空约束 等等. 如: a, b 两行记录代表两个银行账户, 转账事务结束, a, b 总金额不变, ab 的总额就是一个一致性约束; 如果 a 扣款了, b 没有响应的增加, 则不是一致性的

     一致性是最基本的特性, 其它的三个特性都为了保证一致性而存在的

-   I: 并发的两个事务相互隔离.

    数据库中的事务一般都是并发的

    隔离级别高, 表示一个事务不能看到其他事务运行过程的中间状态 (也就是多个事务并发执行后的状态，和它们串行执行后的状态是等价的)

    数据库的隔离性依赖于加锁 (悲观锁, 乐观锁) 或者多版本控制

-   D: 事务一旦结束, 数据就被持久化到磁盘了, 无法回滚


### 实现 acid 特性的原理

#### 原子性

undo log 会记录当前执行的SQL的反操作,用于回滚, 实现事务原子性

InnoDB存储引擎还提供了两种事务日志：redo log(重做日志)和undo log(回滚日志)。其中redo log用于保证事务持久性；undo log则是事务原子性和隔离性实现的基础


#### 持久性

利用 redo log 实现

数据保存到磁盘就持久了, 为什么还需要 redo log? 要解释这个问题, 需要介绍 Buffer Pool (innoDB 的缓存): 每次读写数据都需要数据库磁盘IO, 为了提高效率, 引入缓存, 当从数据库读取数据时，会首先从Buffer Pool中读取，如果Buffer Pool中没有，则从磁盘读取后放入Buffer Pool；当向数据库写入数据时，会首先写入Buffer Pool，Buffer Pool中修改的数据会定期刷新到磁盘中. 但是也带了新的问题：如果MySQL宕机，而此时Buffer Pool中修改的数据还没有刷新到磁盘，就会导致数据的丢失，事务的持久性无法保证

于是，redo log被引入来解决这个问题：当数据修改时，除了修改Buffer Pool中的数据，还会在redo log记录这次操作；当事务提交时，会调用fsync接口对redo log进行刷盘。如果MySQL宕机，重启时可以读取redo log中的数据，对数据库进行恢复

redo log 写入磁盘要快于Buffer Pool的刷脏, 为什么? 

- 刷脏是随机IO，因为每次修改的数据位置随机，但写redo log是追加操作，属于顺序IO。
- 刷脏是以数据页（Page）为单位的，MySQL默认页大小是16KB，一个Page上一个小修改都要整页写入；而redo log中只包含真正需要写入的部分，无效IO大大减少。


redo log 和 bin log 区别: (bin log 也是用于数据恢复)

- 作用不同：redo log是用于crash recovery的，保证MySQL宕机也不会影响持久性; binlog是用于保证服务器可以基于时间点恢复数据，此外binlog还用于主从复制
- 层次不同：redo log是InnoDB存储引擎实现的，而binlog是MySQL的服务器层实现的，同时支持InnoDB和其他存储引擎
- 内容不同：redo log是物理日志，内容基于磁盘的Page；binlog的内容是二进制的，根据binlog_format参数的不同，可能基于sql语句、基于数据本身或者二者的混合


#### 隔离性

- (一个事务)写操作对(另一个事务)写操作的影响：锁机制保证隔离性, 

    这里的锁和 java 中的 锁类似, 事务需要先获取锁才能写数据 (innoDB 支持行锁, 表锁)

- (一个事务)写操作对(另一个事务)读操作的影响：MVCC保证隔离性

    repeatable read 解决脏读、不可重复读、幻读等问题，使用的是MVCC (MVCC全称Multi-Version Concurrency Control，即多版本的并发控制协议), MVCC最大的优点是读不加锁，因此读写不冲突，并发性能好

    多个版本的数据可以共存，主要是依靠数据的隐藏列(也可以称之为标记位)和undo log, 其中数据的隐藏列包括了该行数据的版本号、删除时间、指向undo log的指针等等；当读取数据时，MySQL可以通过隐藏列判断是否需要回滚并找到回滚需要的undo log

    InnoDB实现的RR通过next-key lock机制避免了幻读现象: next-key lock是行锁的一种，实现相当于record lock(记录锁) + gap lock(间隙锁)；其不仅会锁住记录本身(record lock的功能)，还会锁定一个范围(gap lock的功能)



查看锁情况:

```sql
-- 其中lock_type为RECORD，代表锁为行锁(记录锁)；lock_mode为X，代表排它锁(写锁)。
select * from information_schema.innodb_locks; --锁的概况

show engine innodb status; --InnoDB整体状态，其中包括锁的情况

```

#### 一致性

一致性是事务追求的最终目标：前面提到的原子性、持久性和隔离性，都是为了保证数据库状态的一致性

- 数据库本身提供保证, 如不允许向整形列插入字符串值、字符串长度不能超过列的限制等
- 应用层面进行保障，例如如果转账操作只扣除转账者的余额，而没有增加接收者的余额，无论数据库实现的多么完美，也无法保证状态的一致



### 并发问题和隔离级别

总结表:

|             事务隔离级别              | 脏读 | 不可重复读 | 幻读 |                       解释                       |
| :-----------------------------------: | :--: | :--------: | :--: | :----------------------------------------------: |
|     读未提交（read-uncommitted）      |  是  |     是     |  是  |            可以读其他事务未提交的数据            |
| 不可重复读/读已提交（read-committed） |  否  |     是     |  是  |            只能读其他事务已提交的数据            |
|      可重复读（repeatable-read）      |  否  |     否     |  是  | 并发事务间无影响, 无论事务是否提交【MySql 默认】 |
|        串行化（serializable）         |  否  |     否     |  否  | serializable 时会锁表,是最安全的, 但是基本不会用 |

MySQL 设置, 查看:

```sql
-- 设置
set session transaction isolation level read uncommitted;

-- 查看全局隔离级别
select @@global.tx_isolation

-- 查看当前会话隔离级别
select @@tx_isolation
```

SQL 标准中，定义了四个隔离级别 (由低到高):

-   READ_UNCOMMITTED - 两个事务并发访问 db, 事务 a 可以读取到 事务 b 未提交的数据

    存在脏读 (dirty read): 一个事务 A 读到另一个事务 B 没有提交的数据

    如: 事务 1 更新一行数据, 未提交, 事务 2 读取这条数据, 然后事务 1 回滚, 那么事务 2 读取到了脏数据

    如: 事务 1 将 n 从 0 累加到 10, 事务 2 可能读取到 0 ~ 10 的任意值 造成脏读

-   READ_COMMITTED - 两个事务并发访问, 互相只允许读到对方已经提交的数据, 未提交的无法读取到

    解决了脏读. 如: 事务 A 在将 n 从 0 累加到 10 的过程中，B 无法看到 n 的中间值，最终只能获取 10

    存在不可重复读 (Non-repeatable Read): 并发访问的两个事务, 在一个事务里面读取了两次某条数据，读出来的数据不一致 (强调 update, 也就是这条数据被其他事务更新了). 如: 事务 1 读取到一行, 事务 2 修改这行, 事务 1 再次读取这行 数据结果不同

-   REPEATABLE_READ - 并发事务中，事务 1 看到的数据是固定的, 无论事务2修改后是否提交

    会加行级锁. 解决了 不可重复读

    存在幻读 (Phantom Read): 在同一个事务中, 对某个范围查询, 可能出现不一致的结果 (强调 insert, 即其他事务新插入了某条数据到这个范围中)

    如何解决: 锁住满足条件的整张表, 使得其他事务无法插入新的记录

-   SERIALIZABLE - 所有事务 串行执行

    也就是不允许事务并发访问 db.

    最严格, 效率最低

不同的数据库/存储引擎默认不同的隔离级别，比如 InnoDB 存储引擎默认支持 REPEATABLE_READ，而 Mongodb 只支持 READ_UNCOMMITTED


# sql-server

## 数据类型

### string类型

![alt](Snipaste_2018-10-18_14-37-10.png)

### number类型

```
tinyint  允许从 0 到 255 的所有数字。  1 字节

smallint  允许介于 -32,768 与 32,767 的所有数字。  2 字节

int  允许介于 -2,147,483,648 与 2,147,483,647 的所有数字。  4 字节

bigint  允许介于 -9,223,372,036,854,775,808 与 9,223,372,036,854,775,807 之间的所有数字。  8 字节

decimal(p,s)    固定精度和比例的数字。    5-17 字节

```

### date类型

![alt](Snipaste_2018-10-18_14-46-06.png)

### sqlserver特有类型

![alt](Snipaste_2018-10-18_14-46-48.png)

## 存储过程

存储过程，带有逻辑的sql语句

1）执行效率非常快，编译一次后，再使用无需编译
2）移植性很差！不同数据库的存储过程不能移植

### 无参存储过程

```sql
create proc <proc_name>
as         --此处 as 不可以省略不写
begin       --begin 和 end 是一对，不可以只写其中一个，但可以都不写
select S#,Sname,Sage,Ssex from student
end


```

### 有参存储过程

#### 参数是全局变量

```sql
-- 全局变量也称为外部变量，是在函数的外部定义的，它的作用域为从变量定义处开始，到本程序文件的末尾。

create proc <StuProc>
@sname varchar(100)
as
begin
select S#,Sname,Sage,Ssex from student where sname=@sname
end
go

exec StuProc 'XXX' -- //执行语句, -- 这是在外部给变量赋值，也可以在内部直接给变量设置默认值

create proc <StuProc>
@sname varchar(100)='XXX'
as
begin
select S#,Sname,Sage,Ssex from student where sname=@sname
end
go

exec StuProc 

-- 也可以把变量的内容输出，使用output
create proc StuProc
@sname varchar(100),
@IsRight int output --//传出参数
as
if exists (select S#,Sname,Sage,Ssex from student where sname=@sname)
set @IsRight =1
else
set @IsRight=0
go

declare @IsRight int
exec StuProc 'XXX' , @IsRight output
select @IsRight


```

#### 参数是局部变量

```sql
-- 局部变量是在函数内作定义说明的。其作用域仅限于函数内部，离开该函数后再使用这种变量是非法的。
-- 局部变量的定义：必须先用Declare命令定以后才可以使用: declare{@变量名 数据类型}
-- 局部变量的赋值方法：set{@变量名=表达式}或者select{@变量名=表达式}
-- 局部变量的显示：select @变量名
create proc <StuProc>
as
declare @sname varchar(100)
set @sname='XXX'
select S#,Sname,Sage,Ssex from student where sname=@sname
go

exec StuProc

-- 那如果是要把局部变量的数据显示出来怎么办呢？
create proc StuProc
as
declare @sname varchar(100)
set @sname=(select Sname from student where S#=01)
select @sname
go

exec StuProc


```

## 碰到的问题记录

除法运算结果为小数时显示0的解决方案

```sql
ref: http://blog.sina.com.cn/s/blog_8020e41101019k7t.html

SELECT field1/field2 FROM TB;
　　当 field1的数值 > field2的数值时，除法得出的结果是<1的，即0.xxxxxx
　　这个时候在DB2的环境下SELECT出来的值是0
解決方法：
先把field1转换成DOUBLE类型的，这样计算出来的就会得出小数点了，会显示出0.xxxx
SELECT CAST(field1 AS FLOAT)/field2 FROM TB;
ps.網上搜的資料，寫的是double，但在SQL Server2008中一直報錯，改成FLOAT就沒問題了。
小數點後顯示4位小數。
 
　可以進一步四舍五入，保留两位小数点
SELECT ROUND(CAST(field1 AS DOUBLE)/field2, 2) FROM TB;
不過因為我要用到百分比，就改成了
SELECT CAST(field1 AS FLOAT)/field2 * 100 FROM TB;
然後再頁面顯示時再加上“%”
 
如果数据列的值为NULL，将其设置为0，那么sql就要这么写
　　SELECT ROUND(COALESCE(CAST(field1 AS DOUBLE), 0)/field2, 2) FROM TB;
　　COALESCE这个函数系统的用法如下:
　　a. 输入参数为字符类型，且允许为空的，可以使用COALESCE(inputParameter,”)把NULL转换成”;
　　b. 输入类型为整型，且允许为空的，可以使用COALESCE(inputParameter,0)，把空转换成0;
　　c. 输入参数为字符类型，且是非空非空格的，可以使用COALESCE(inputParameter,”)把NULL转换成”，然后判断函数返回值是否为”;
　　d. 输入类型为整型，且是非空的，不需要使用COALESCE函数，直接使用IS NULL进行非空判断。
----------------------------------------------------------------
 
备注：
CAST函数用来类型转换
ROUND函数用来控制小数位数


```

查询数据是添加一列自增的列

```sql
第一个办法:用ROW_NUMBER() OVER(ORDER BY  你原来排序的方式 );
eg：
select ROW_NUMBER() over (order by avg(score)) '序号',  s#, avg(score) '均分' from sc  group by s#

第二个办法:使用Identity+临时表,例如:---------select语句必须有into
select Identity(int,1,1),t.*  Into #temptable from  表 t;
select * from #temptable;


```

# oracle

## 字段类型

```
varchar  -- 存放定長的字符数据，最长2000個字符；

varchar2 -- 存放可变长字符数据，最大长度为4000字符。 推荐

```

https://blog.csdn.net/superit401/article/details/51565119 mysql 对比 , 数据类型转换



## Oracle  sql

```sql
-- 删除/彻底删除
drop table tb_maintence [purge];


```