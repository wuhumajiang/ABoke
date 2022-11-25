---
title: hive数据仓库
date: 2022-10-10 22:04:29
tags: 学习
category: 学习
cover: https://boke-file2.oss-cn-hangzhou.aliyuncs.com/bx8.jpeg
---

#### 元数据

元数据是描述数据仓库内数据的结构和建立方法的数据。和表中数据没有任何关系，反应的是表本身的信息。Hive中的元数据包括表名、表所属的数据仓库、表的所有者、列/分区字段、表的类型(是否为外部表)、表的数据所在目录等。

#### 数据类型

基本数值类型、布尔类型、字符串类型、时间戳类型等。复杂数据类型包括数组(Array)类型、映射(Map)类型和结构体(Struct)类型。

TInyint(1字节)、Smallint(2字节)、Int(4字节)、Bigint(8字节)、Boolean(布尔类型)、Float(单精度浮点数)、Double(双精度浮点数)、Decimal(任意精度)、String、Varchar、Char(定长)、Date(日期，年月日)、TimeStamp(时间戳)、BInary(字节数组)

#### HIve表存储格式

行式存储：所有字段的一条数据为一块。即一行。TextFile(默认)、SequenceFile

列式存储：一个字段的所有数据为一块。即一列。ORC、Parquet(压缩最狠)

#### 数据仓库语法

创建	**create  database  [if not exists]  dataname  location  direction**;	数据仓库默认存储路径/usr/hive/warehouse/

删除	**drop  database  [if  exists]  database_name  [cascade]  ;**	cascade强制删除

查询当前数据仓库	**select  current_database();**

显示数据仓库	**show  databases;**

显示数据仓库详细信息	**desc  database  extended  database_name;**

切换	**use  database_name;**

修改	**alter  database  database_name set  dbproperties(key,value);**	数据仓库的Dbproperties键值对属性值可修。其他元数据信息不可更改。

#### 建表语法树

**create  [external]  table  [if  not  exists]  table_name**

**(col_name  data_type  [comment  col_comment],......)**

**[comment  table_comment]**

**[partitioned  by  (col_name  data_type  [comment  col_comment],......)]**

**[clustered  by  (col_name,col_name,......)  into  num_buckets  buckets]**

**[sorted  by  (col_name  [ASC|DESC],......)]**

**[row  format  delimited  row_format]**

**[stored  as  file_format]**

**[location  hdfs_path]**