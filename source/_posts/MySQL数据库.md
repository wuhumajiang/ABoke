---
title: MySQL数据库
date: 2022-10-13 18:09:49
cover: https://qiansen.oss-cn-hangzhou.aliyuncs.com/MySQL数据库.png
tags: 学习
category: 学习
---

#### 注释

​	单行注释: 	--注释内容 或 # 注释内容

​	多行注释:     /* 注释内容 */ 

#### SQL分类

​	DDL	数据定义语言，用来定义数据库对象（数据库，表，字段）

​	DML	数据操作语言，用来对数据库中的数据进行增删改

​	DQL	数据查询语言，用来查询数据库中表的记录

​	DCL	数据控制语言，用来创建数据库用户、控制数据库的访问权限

##### DDL

查询所有数据库	**show databases;**

查询当前数据库	**select databases;**

创建数据库	**create database	[if not exists]  数据库名  [defaul  charset  字符集]  [collate  排序规则]**

[if not exists]	判断数据库是否存在，存在则不创建，不会报错，不加这个创建已有数据库，会报错。

[defaul  charset  字符集]	即数据编码格式 详情看这  [mysql建数据库的字符集与排序规则说明](https://blog.csdn.net/qq_38224812/article/details/80745868)

删除数据库	**drop databases  [if exists]  数据库名;**

使用数据库	**use  数据库名;**

查询当前数据库所有表	**show tables;**

查询表结构	**desc  表名;**

查询指定表的建表语句	**show create  table 表名;**

创建表	**create  table  表名(字段  字段类型  [comment  字段注释])  [comment  表注释];**    多个字段用逗号隔开

添加字段	**alter table  表名  add  字段名  类型(长度)  [comment   注释]   [约束];**

修改数据类型	**alter  table  表名  modify  字段名  新数据类型  (长度);**

修改字段名是字段类型	**alter  table  表名  change  旧字段名  新字段名  类型(长度)  [comment  注释]  [约束];**

删除字段	**alter  table  表名  drop  字段名;**

修改表名	**alter  table  表名  rename  to  新表名;**

删除表	**drop  table  [if  exists]  表名;**

删除指定表，并重新创建该表	**truncate  table  表名;**

##### DML

指定字段添加数据	**insert  into  表名  (字段1,字段2, ...)  values  (值1，值2, ...),(值1,值2, ...);**

全部字段添加数据	**insert  into  values  (值1,值2, ...),(值1,值2, ...);**

修改数据	**update  表名  set  字段1 = 值1,字段2 = 值2  [where  条件];**

删除数据	**delete  from  表名  [where  条件];**

##### DQL

查询数据	**select  字段列表  from  表名   where  条件列表  group  by  分组字段列表  having  分组后条件列表  order  by  排序字段列表  limit  分页参数  **

**基本查询**	

**select  字段1,字段2  ...  from  表名;**         **select * from 表名;**  (查询所有数据)

设置别名	**select  字段1  [as  别名1],字段2  [as  别名2]  ...  from  表名;**

去除重复记录	**select  distinct  字段列表  from  表名;**

**条件查询**

**select  字段列表  from  表名  where  条件列表;**

详情 -->  [ MySQL 条件查询_WYSCODER的博客-CSDN博客_mysql 条件查询](https://blog.csdn.net/sheng0113/article/details/122165646)

**聚合函数**

将一列数据作为一个整体，进行纵向计算

统计数量	**count(字段名)**

最大值	**max(字段名)**

最小值	**min(字段名)**

平均值	**avg(字段名)**

求和	**sum(字段名)**

**分组查询**

**select  字段列表  from  表名  [where  条件]   group  by  字段分组名  [having   分组后过滤条件]**

**where和having区别**

having是在分组后对数据进行过滤
where是在分组前对数据进行过滤
having后面可以使用聚合函数
where后面不可以使用聚合

**排序查询**

**select  字段列表  from  表名  order  by  字段1  排序方式1,字段2  排序方式2 ;**

降序	**DESC**		升序	**ASC**    (默认)		

**多字段排序时，第一个字段值相同时，才会根据第二个字段进行排序**

**分页查询**

**select  字段列表  from  表名  limit  起始索引，查询记录数;**

**起始索引从0开始,起始索引 = (查询页码-1) * 每页显示记录数。**

**如果查询的是第一页数据，起始索引可以省略，直接简写为limit 10。**

**执行顺序**

**from  表名列表  where  条件列表  group by 分组字段列表  having  分组后条件列表  select 字段列表  order by 排序字段列表   limit  分页参数**

**DCL**

查询用户	**use mysql;**	**select * from user;**

创建用户	**create  user  '用户名'@'主机名'   identified by  '密码';**

修改用户密码	**alter  user  '用户名'@'主机名'   identified  with  mysql_native_password  by  '新密码';**

删除用户	**drop  user  '用户名'@'主机名';**

查询权限	**show  grants  for  '用户名'@'主机名';**

授予权限	**grant  权限列表  on  数据库名.表名  to  '用户名'@'主机名';**

撤销权限	**revoke  权限列表  on  数据库名.表名  from  '用户名'@'主机名';**

#### 约束

详情--> [MySQL约束总结(CONSTRAINT)](https://blog.csdn.net/z_johnny/article/details/113820405)		主键自增: auto_increment(在主键约束后加即可中间空格)

**外键约束**	[MySQL：简述MySQL外键约束](https://xiaoer.blog.csdn.net/article/details/86293678?spm=1001.2101.3001.6650.3&utm_medium=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~Rate-3-86293678-blog-125227840.pc_relevant_multi_platform_whitelistv4&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~Rate-3-86293678-blog-125227840.pc_relevant_multi_platform_whitelistv4&utm_relevant_index=6)

#### 多表查询

**多表关系**

一对多(多对一)		实现：在多的一方建立外键，指向一的一方的主键

多对多		实现: 建立第三张中间表，中间表至少包含两个外键，分别关联两方主键。

一对一		多用于单表拆分，将一张表的基础字段放在一张表中，其他详情字段放在另一张表中。

实现:	**在任意一方加入外键，关联另外一方的主键，并且设置外键是唯一的(unique)**

#### 连接查询

内连接:	**相当于查询A、b交集部分数据**

外连接:	**左外连接:	查询左表所有数据，以及两张表交集部分数据。**	**右外连接:	查询右表所有数据，以**及两张表交际部分数据。**

自连接:	**当前表与自身的连接查询，自连接必须使用别名。**	

子查询

![连接查询](https://qiansen.oss-cn-hangzhou.aliyuncs.com/%E8%BF%9E%E6%8E%A5%E6%9F%A5%E8%AF%A2.png)

**内连接**

隐式内连接		**select  字段列表  from  表1，表2   where  条件 ...;**

显示内连接		**select  字段列表  from  表1  [inner]  join  表2  on  连接条件;**

**外连接**

左外连接			**select  字段列表  from  表1  left  [outer]  join  表2  on  连接条件;**

右外连接			**select  字段列表  from  表1  right  [outer]  join  表2  on  连接条件;**

**自连接**

**select  字段列表  from  表A  别名A  join  表A  别名B  on  条件 ...;**		(自连接，可以是内连接，也可以是外连接)

**联合查询**

**select  字段列表  from  表A  ...  union [all]  select  字段列表  from  表B  ...;**			

对于联合查询的多张表的列数必须保持一致，字段类型也需要保持一致。

union  all  会将全部的数据直接合并在一起，union会对合并之后的数据去重。

**子查询**

#### 事务

事务是一组操作的集合，它是一个不可分割的工作单位，事务会把所有的操作作为一个整体一起向系统提交或撤销操作请求，即这些操作要么同时成功，要么同时失败。

**查看/设置事务提交方式**		**selcet  @@autocommit;**	(显示1为自动提交，mysql默认为1=自动)			**set  @@autocommit = 0;**  (设置为手动提交)

**提交事务**		**commit;**

**回滚事务**		**rollback;**

**开启事务**		**start  transacition**    或   **begin**

**事务四大特性(ACID):**

**原子性(Atomicity):**		事务是不可分割的最小操作单元，要么全部成功，要么全部失败。

**一致性(Consistency):**		事务完成时，必须使所有的数据都保持一致状态。

**隔离性(Isolation):**		数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行。

**持久性(Durability):**		事务一旦提交或回滚，它对数据库中的数据的改变就是永久的。

**并发事务问题**

**脏读**					一个事务读到另外一个事务还没有提交的数据。

**不可重复读	**	一个事务先后读取同一条记录，但两次读取的数据不同。

**幻读**					一个事务按照条件查询数据时，没有对应的数据行，但是在插入数据时，又发现这行数据已经存 在，好像出现了‘幻影‘。

**事务隔离级别**											                      脏读					不可重复读						幻读

**Read uncommitted**（读未提交）							   √								√									√

**Read committed**（读已提交）									×								√									√

**Repeatable Read**（可重复读）								    ×								×									√

**Serializable**（序列化）											      ×								×									 ×

**查看事务隔离级别**			**select  @@transacition_isolation;**

**设置事务隔离级别**			**set  [session | global ]  transacition isolation level  {read uncommitted | read comitted | Repeatable Read | Serializable}**   （事务级别关键字不区分大小写）

事务隔离级别越高，数据越安全，但是性能越低。