---
layout: post
title:  "Mac环境下,配置spark连接hive的本地开发环境"
date:   2022-10-22 16:00
categories: spark
tags: spark hive hadoop
author: Max
mathjax: true
---
* content
{:toc}

### 安装hadoop

mac安装hadoop需要先开启ssh登录电脑的功能。

#### 一、配置ssh localhost

终端下执行`ssh localhost`，如果报错则说明需要配置下。

1、进入系统偏好设置 --> 共享 --> 勾选远程登录->勾选所有用户

2、把个人公钥夹加到key上，保证可以本地登录。

```
cd ~/.ssh
cat id_rsa.pub >> authorized_keys
```

`id_rsa.pub`不存在的可以执行下`ssh-keygen`生成一个。

#### 二、安装和配置

1、`brew install hadoop`安装hadoop

2、进到`/usr/local/Cellar/hadoop/${version}/libexec/etc/hadoop`目录，修改`hdfs-site.xml`,`core-site.xml`文件

```xml
//hdfs-site.xml
<property>
   <name>dfs.replication</name>
   <value>1</value>
</property>
<property>
   <name>dfs.namenode.name.dir</name>
  <!-- 配置个人目录 -->
   <value>file:/usr/local/Cellar/hadoop/3.3.4/libexec/tmp/name</value>
</property>
<property>
   <name>dfs.datanode.data.dir</name>
  <!-- 配置个人目录 -->
   <value>file:/usr/local/Cellar/hadoop/3.3.4/libexec/tmp/data</value>
</property>


//core-site.xml 添加配置
  <property>
     <name>hadoop.tmp.dir</name>
     <!-- 配置个人目录 -->
     <value>file:/usr/local/Cellar/hadoop/3.3.4/libexec/tmp</value>
  </property>
  <property>
     <name>fs.defaultFS</name>
     <value>hdfs://localhost:8020</value>
  </property>
```

#### 三、启动服务

```shell
cd /usr/local/Cellar/hadoop/3.3.4/libexec/sbin
./start-dfs.sh
```

> 可以去 http://localhost:9870 查看服务

### 安装hive

我使用`brew install hive`后，服务不正常，可能我的系统版本太老了，所以我就使用文件目录来启动hive服务了

#### 一 、下载配置

我下载了老版本的hive，地址：[hive-2.3.9](https://dlcdn.apache.org/hive/hive-2.3.9/)

1、解压文件

2、配置环境变量

编辑环境变量`vim ~/.bash_profile`

```
//指定hive文件目录
export HIVE_HOME=/Users/max/Documents/tools/apache-hive-2.3.9-bin
export PATH=$HIVE_HOME/bin:$PATH%
```

刷新配置`source ~/.bash_profile`

#### 二、修改Metastore数据库配置

hive默认使用了内嵌的derby数据库，这个只能单线程，需要修改成mysql或者postgresql数据库。我使用了mysql

1、在本地mysql数据库新建database：`create database hive;`

2、添加mysql连接驱动器：`mysql-connector-java-8.0.11.jar`（没有的话搜索下载）,文件拷贝到 $HIVE_HOME/lib目录下。

3、进到`$HIVE_HOME/conf`下，配置`hive-site.xml`

```xml
<configuration>
<!-- <property><name>hive.metastore.local</name><value>true</value></property> -->
<property><name>hive.metastore.uris</name><value>thrift://localhost:9083</value></property>
	<!-- 连接数据库的url -->
  <property><name>javax.jdo.option.ConnectionURL</name><value>jdbc:mysql://localhost:3306/hive?useUnicode=yes&amp;characterEncoding=UTF-8&amp;useSSL=false&amp;useLegacyDatetimeCode=false&amp;serverTimezone=UTC</value></property>
	<!-- 连接数据库所用的驱动 -->
  <property><name>javax.jdo.option.ConnectionDriverName</name><value>com.mysql.cj.jdbc.Driver</value></property>
　<!--mysql用户名-->
  <property><name>javax.jdo.option.ConnectionUserName</name><value>root</value></property>
　<!--mysql密码-->
　<property><name>javax.jdo.option.ConnectionPassword</name><value>root</value></property>

	<!-- 本地模式不需要配置 -->
	<!--<property><name>hive.metastore.uris</name><value>thrift://cdh:9083</value></property>-->
  <property><name>hive.metastore.warehouse.dir</name><value>/user/hive/warehouse</value><description>location of default database for the warehouse</description></property>
</configuration>
```

#### 三、初始化

1、进到`$HIVE_HOME/bin`目录，执行初始化数据库：`schematool -dbType mysql -initSchema`

2、进到`$HIVE_HOME/bin`目录，执行`nohup ./hive --service metastore &`开启`thriftserver`服务，供spark连接hive使用

> 命令行执行hive命令后即可使用

### 配置spark

spark不需要安装，直接打开就好。

#### 配置连接hive环境

把刚刚的hive-site.xml配置文件copy到spark的conf目录下就好。

进到spark-shell环境，执行`sparl.sql(“show databases”).show`，可以看到hive的库。