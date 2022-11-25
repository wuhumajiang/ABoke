---
title: Hadoop安装
date: 2022-10-21 20:12:04
tags: 学习
category: 学习
cover: https://boke-file2.oss-cn-hangzhou.aliyuncs.com/bx11.jpeg
---

### hadoop分布式集群安装

**需下载**

hadoop-3.3.4.tar.gz			jdk-8u333-linux-x64.rpm   版本自己选择

**安装虚拟机 **  

**规划节点和IP地址（win+R cmd  ipconfig 查看VMnet8ip地址(我用的)把最后一个点后面改成自己想改的3个连续数字）**
node1 192.168.2.80 NN																																					node2 192.168.2.81 DN
node3 192.168.2.82 SN

**虚拟机设置管理员（安装CentOS7步骤里会有）(下面用户名均用user代替)**
用户名：自己设
密码：自己设

使用MobaXtem连接192.168.2.80（如果连接不上需要打开控制面板->网络和Internet->网络和共享中心->更改适配器设置->启用两个以太网VMware）（可以不用远程连接工具，只是为了方便上传文件）
**安装基础工具 **
sudo yum install net-tools
sudo yum install vim

在user的~目录下新建文件夹 mkdir software，修改权限 chmod -R 777 software
将Hadoop和jdk安装包放到soft文件夹下，注意，Java版本要求Java8及以下

使用sudo rpm -ivh jdk-8u333-linux-x64.rpm 命令安装jdk包，
使用sudo tar -zxvf hadoop-3.3.4.tar.gz -C /usr/ 将Hadoop解压到/usr目录下
使用sudo chown -R user:user /usr/hadoop-3.3.4/将hadoop-3.3.4改为user用户组的user用户

**关闭防火墙（如果不关闭可能出现节点间无法通信的情况）**
sudo systemctl stop firewalld.service （停止防火墙）
sudo systemctl disable firewalld.service （彻底关闭防火墙）

**关闭selinux（防止传输文件时出问题）**
sudo vim /etc/selinux/config
修改为 SELINUX=disabled

**添加hadoop环境变量**

把hdfs命令直接加到环境变量中，这样在任意地方执行hdfs命令都可以，不需要在进入hadoop-3.3.4/bin目录下执行，也可以直接在/etc/profile里改，但为了方便维护，就直接在profile.d文件夹下新增一个.sh文件hdfs.sh，如果后期不想要这个命令，可直接删除hdfs.sh文件hdfs
新增一个sh文件sudo vim /etc/profile.d/hdfs.sh，填入如下内容：
export HADOOP_HOME=/usr/hadoop-3.3.4
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
重启profile：source /etc/profile  （相当于重启了source，让hdfs.sh被激活，起作用）

**java环境变量配置:**
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.345.b01-1.el7_9.x86_64（这填自己的java安装目录）
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

whereis java（寻找java安装目录）
ls -lrt /usr/bin/java
ls -lrt /etc/alternatives/java

/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.345.b01-1.el7_9.x86_64/jre/bin(我的java安装目录)

找不到，详情可以看[ Centos7 bash：jps：未找到命令](https://blog.csdn.net/baidu32552365/article/details/108716966)

**创建HDFS的NN和DN工作主目录:**
sudo mkdir /var/big_data
sudo chown -R user:user /var/big_data

**配置Hadoop（一般.sh文件都是寻找Java运行环境，因此主要配置JAVA_HOME）**
进入 cd /usr/hadoop-3.3.4/etc/hadoop 目录下
sudo vim hadoop-env.sh
修改export JAVA_HOME=/usr/java/default

**为Yarn任务、资源管理器提供Java运行环境（hadoop-3.3.4无需配置）**
vim yarn-env.sh
export JAVA_HOME=/usr/java/default

**配置HDFS主节点信息、持久化和数据文件的主目录（如果tab不是4个空格，改一下sudo vim /etc/vimrc，添加set ts=4）**
        vim core-site.xml在<configuration>中添加如下配置
	<property>
		<name>fs.defaultFS</name>
		<value>hdfs://node1:9000</value>
	</property>
	<property>
		<name>hadoop.tmp.dir</name>
		<value>/var/big_data</value>
	</property>

**配置HDFS默认的数据存放策略**
        vim hdfs-site.xml
	<property>
		<name>dfs.replication</name>
		<value>2</value>
	</property>
	<property>
		<name>dfs.namenode.secondary.http-address</name>
		<value>node3:9868</value>
	</property>
	<property>
		<name>hadoop.proxyuser.user.hosts</name>
		<value>*</value>
	</property>
	<property>
		<name>hadoop.proxyuser.user.groups</name>
		<value>*</value>
	</property>

**配置mapreduce任务调度策略**
        vim mapred-site.xml 
	<property>
		<name>mapreduce.framework.name</name>
		<value>yarn</value>
	</property>
			
**配置Yarn资源管理角色的信息**
        vim yarn-site.xml
	<property>
		<name>yarn.nodemanager.aux-services</name>
		<value>mapreduce_shuffle</value>
	</property>
	<property>
		<name>yarn.resourcemanager.hostname</name>
		<value>node1</value>
	</property>
			
**配置datanode节点信息**
        vim slaves
node1
node2
node3
			
**提前准备主机名解析文件，为后面的克隆机器做好准备（可选，若不做，克隆后为每台机器重新添加亦可）**
    sudo vim /etc/hosts
192.168.33.80  node1
192.168.33.81  node2
192.168.33.82  node3
		
**重启 sudo reboot**

**克隆其他集群信息**
    关闭机器，准备克隆
    克隆后，修改node2、node3的IP和主机名
    修改主机名sudo vim /etc/hostname 
    修改IP:sudo vim /etc/sysconfig/network-scripts/ifcfg-ens33 
    然后重启：sudo reboot
	
**下面开始配置集群的ssh免密
    在3台机器上执行产生自己的公钥：**
        ssh-keygen -t rsa
    按照默认值回车确定
    将每台机器的公钥拷贝给每台机器，注意下面的指令要求3台机器都要执行：
        ssh-copy-id node1
        ssh-copy-id node2
        ssh-copy-id node3
		
**格式化hdfs**
    hdfs namenode -format

**开启集群测试**
start-dfs.sh

Hadoop3的web端访问端口为9870