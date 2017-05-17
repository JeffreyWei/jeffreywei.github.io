---
layout: post
title: hadoop-单用户部署
share: true
comments: true
imagefeature:
tags: [hadoop]
category: hadoop
description: "Hadoop Standalone on Mac"
---

Mac上单机模式部署hadopp2.7.0

<!--more-->

### 1.Hadoop下载

![ ][2]

[hadoop-2.7.0](http://www.apache.org/dyn/closer.cgi/hadoop/common/hadoop-2.7.0/hadoop-2.7.0.tar.gz)

### 2.配置修改
etc/hadoop/hadoop-env.sh
	 
	 # set to the root of your Java installation
  	 export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.7.0_75.jdk/Contents/Home
	可以使用java -verbose查看Mac当前JDK的安装路径

etc/hadoop/core-site.xml

	<configuration>
    	<property>
        	<name>fs.defaultFS</name>
        	<value>hdfs://localhost:9000</value>
	    </property>
	</configuration>

etc/hadoop/hdfs-site.xml


	<configuration>
    	<property>
        	<name>dfs.replication</name>
	        <value>1</value>
    	</property>
	</configuration>

ssh
打开mac的ssh服务在“系统偏好设置-->共享-->远程登录”
使用如下命令测试

		ssh localhost

此时需要配置自动登录

	$ ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa
	$ cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys
	$ export HADOOP\_PREFIX=/usr/local/hadoop


### 3.启动&测试
* 初始化文件系统

		$ bin/hdfs namenode -format

* 启动文件服务

		$ sbin/start-dfs.sh
  
  访问[http://localhost:50070/](http://localhost:50070/)，查看服务状态
  
	![ ][1]

* 测试
		
		$ mkdir input
		$ cp etc/hadoop/*.xml input
		$ bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.0.jar grep input output 'dfs[a-z.]+'
		$ cat output/*
		
		
**参考：**

[官方文档](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/SingleCluster.html#Standalone_Operation)




[1]: {{ site.url }}/images/posts/2015-05/1.png "hadoop web admin page"
[2]: {{ site.url }}/images/posts/2015-05/hadoop-logo.jpg "hadoop logo"



