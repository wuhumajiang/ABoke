---
title: MySQL数据库
date: 2022-10-13 18:09:49
cover: https://qiansen.oss-cn-hangzhou.aliyuncs.com/7.png
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

详情--> [MySQL约束总结(CONSTRAINT)](https://blog.csdn.net/z_johnny/article/details/113820405)

**外键约束**	[MySQL：简述MySQL外键约束](https://xiaoer.blog.csdn.net/article/details/86293678?spm=1001.2101.3001.6650.3&utm_medium=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~Rate-3-86293678-blog-125227840.pc_relevant_multi_platform_whitelistv4&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~Rate-3-86293678-blog-125227840.pc_relevant_multi_platform_whitelistv4&utm_relevant_index=6)