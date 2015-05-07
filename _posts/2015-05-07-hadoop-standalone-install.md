---
layout: post
title: hadoop-1-单用户部署
share: true
comments: true
imagefeature:
tags: [hadoop]
category: code java
description: "hadoop Standalone on Mac"
---

### 1.下载

[hadoop-2.7.0](http://www.apache.org/dyn/closer.cgi/hadoop/common/hadoop-2.7.0/hadoop-2.7.0.tar.gz)

### 2.配置修改
etc/hadoop/hadoop-env.sh
<pre> # set to the root of your Java installation
  export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.7.0_75.jdk/Contents/Home</pre>
** 可以使用java -verbose查看Mac当前JDK的安装路径**

etc/hadoop/core-site.xml
<pre>
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration></pre>
etc/hadoop/hdfs-site.xml

<pre>
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
</configuration>
</pre>

ssh
打开mac的ssh服务在“系统偏好设置-->共享-->远程登录”
使用如下命令测试
<pre>
ssh localhost
</pre>

此时需要配置自动登录
<pre>
  $ ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa
  $ cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys
  $ export HADOOP\_PREFIX=/usr/local/hadoop
</pre>

### 3.启动&测试
* 初始化文件系统
<pre>
  $ bin/hdfs namenode -format
</pre>
* 启动文件服务
<pre>
  $ sbin/start-dfs.sh</pre>
  
  访问如下地址，查看服务状态
  <pre>
  NameNode - http://localhost:50070/  </pre>
* 测试
  <pre>
  $ mkdir input
  $ cp etc/hadoop/*.xml input
  $ bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.0.jar grep input output 'dfs[a-z.]+'
  $ cat output/*</pre>
参考：

i. [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/SingleCluster.html#Standalone_Operation](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/SingleCluster.html#Standalone_Operation)