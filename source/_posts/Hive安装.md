---
title: Hive安装
date: 2022-10-21 20:13:13
tags: 学习
category: 学习
cover: https://qiansen.oss-cn-hangzhou.aliyuncs.com/Hive安装.jpg
---

### hive安装流程

**需下载(版本可以不同)**

mysql-5.7.29-1.el7.x86_64.rpm-bundle.tar

apache-hive-3.1.2-bin.tar.gz

mysql-connector-java-5.1.32.jar

**Mysql安装**

#卸载Centos7自带mariadb
rpm -qa|grep mariadb
会显示你虚拟机自带mariadb的版本
rpm -e	上面显示的版本(直接复制粘贴) --nodeps

#上传mysql-5.7.29安装包到/software下，解压
tar xvf mysql-5.7.29-1.el7.x86_64.rpm-bundle.tar

**执行安装**
rpm -ivh mysql-community-common-5.7.29-1.el7.x86_64.rpm mysql-community-libs-5.7.29-1.el7.x86_64.rpm mysql-community-client-5.7.29-1.el7.x86_64.rpm mysql-community-server-5.7.29-1.el7.x86_64.rpm

初始化mysql
mysqld --initialize
#更改所属组
chown mysql:mysql /var/lib/mysql -R

#启动mysql
systemctl start mysqld.service
#查看生成的临时root密码
cat /var/log/mysqld.log
#这行日志的最后就是随机生成的临时密码
[Note] A temporary password is generated for root@localhost: 临时密码

#修改mysql root密码、授权远程访问
mysql -u root -p
Enter password:     #这里输入在日志中生成的临时密码

#更新root密码  （设置为自己想设置的）
mysql> alter user user() identified by "123456";
Query OK, 0 rows affected (0.00 sec)
#授权
mysql> use mysql;
mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;
mysql> FLUSH PRIVILEGES;
#退出
exit;

#mysql的启动和关闭 状态查看
systemctl stop mysqld
systemctl status mysqld
systemctl start mysqld
systemctl status mysqld

#建议设置为开机自启动服务
systemctl enable  mysqld
#查看是否已经设置自启动成功
systemctl list-unit-files | grep mysqld

**Hive安装配置**

#上传解压安装包

cd /home/虚拟机用户名/software
tar zxvf apache-hive-3.1.2-bin.tar.gz -C /usr/
cd /usr
mv apache-hive-3.1.2-bin hive

#解决hadoop、hive之间guava版本差异（Java的一个工具包）
cd /usr/hive
rm -rf lib/guava-19.0.jar
cp /usr/hadoop-3.3.4/share/hadoop/common/lib/guava-27.0-jre.jar ./lib/

#添加mysql jdbc驱动到hive安装包lib/文件下
cd /home/虚拟机用户名/software/
mv mysql-connector-java-5.1.32.jar /usr/hive/lib/

#修改hive环境变量文件 添加Hadoop_HOME
cd /usr/hive/conf/
mv hive-env.sh.template hive-env.sh
vim hive-env.sh
export HADOOP_HOME=/usr/hadoop-3.3.4
export HIVE_CONF_DIR=/usr/hive/conf
export HIVE_AUX_JARS_PATH=/usr/hive/lib

#新增hive-site.xml 配置mysql等相关信息
vim hive-site.xml

#-----------------hive-site.xml--------------
<configuration>
    <!-- 存储元数据mysql相关配置 -->
    <property>
        <name>javax.jdo.option.ConnectionURL</name>
        <value> jdbc:mysql://localhost:3306/hivecreateDatabaseIfNotExist=true$amp;useSSL=false$amp;useUnicode=true&amp;characterEncoding=UTF-8</value>
    </property>

<property>
    <name>javax.jdo.option.ConnectionDriverName</name>
    <value>com.mysql.jdbc.Driver</value>
</property>

<property>
    <name>javax.jdo.option.ConnectionUserName</name>
    <value>root</value>
</property>

<property>
    <name>javax.jdo.option.ConnectionPassword</name>
    <value>123456</value>
</property>

<!-- 配置hiveservver2端口号和主机名 -->
<property>
    <name>hive.server2.thrift.port</name>
    <value>10000</value>
</property>
<property>
    <name>hive.server2.thrift.bind.host</name>
    <value>localhost</value>
</property>

<!-- 远程模式部署metastore 服务地址 -->
<property>
    <name>hive.metastore.uris</name>
    <value>thrift://node1:9083</value>
</property>

<!-- 关闭元数据存储授权  -->
<property>
    <name>hive.metastore.event.db.notification.api.auth</name>
    <value>false</value>
</property>

<!-- 关闭元数据存储版本的验证 -->
<property>
    <name>hive.metastore.schema.verification</name>
    <value>false</value>
</property>

</configuration>

#初始化metadata
cd /usr/hive
bin/schematool -initSchema -dbType mysql -verbos
#初始化成功会在mysql中的hive数据仓库创建74张表
mysql -uroot -p
123456

show databases;

**Metastore Hiveserver2启动**

#hive环境变量的配置
在/etc/profile.d下新建hive.sh文件，并添加环境变量信息：
sudo vim /etc/profile.d/hive.sh

添加：
export HIVE_HOME=/usr/hive
export PATH=$PATH:$HIVE_HOME/bin:$HIVE_HOME/sbin

保存并退出
:wq

刷新profile
source profile

#前台启动（关闭ctrl+c）
hive --service metastore

#后台启动 进程挂起（关闭使用jps + kill）
#输入命令回车执行 再次回车 进程将挂起后台
nohup /export/server/hive/bin/hive --service metastore &

#前台启动开启debug日志
/export/server/hive/bin/hive --service metastore --hiveconf hive.root.logger=DEBUG,console

启动hive总体流程
启动hive需要先启动集群：start-dfs.sh （前提是已经在/etc/profile.d/文件夹下添加了hdfs.sh文件，若没有添加，则需要进入hadoop所在文件夹下，执行sbin/start-dfs.sh）
再启动Metastore服务：nohup /usr/hive/bin/hive --service metastore &（远程模式需要手动启动）
再启动hiveservver2：nohup /usr/hive/bin/hive --service hiveserver2 &
再启动hive：bin/beeline，在里面通过JDBC启动与hiveserver的连接：!connect jdbc:hive2://localhost:10000，输入用户名为主机的用户名，密码为用户密码。