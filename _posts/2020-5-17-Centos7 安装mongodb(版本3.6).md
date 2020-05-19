---
layout:     post
title:      Centos7 安装mongodb(版本3.6)
subtitle:   Centos7 安装mongodb(版本3.6)流程
date:       2020-5-17
author:     xiaomaike
header-img: img/post-bg-centos.jpg
catalog: true
tags:
    - Centos7
    - MongoDB
---

系统：CentOS 7
MongoDB版本：[mongodb-linux-x86_64-3.6.16.tgz](http://downloads.mongodb.org/linux/mongodb-linux-x86_64-3.6.16.tgz)
[MongoDB官网下载点](https://www.mongodb.com/download-center/community)
[MongoDB官网Linux版下载点](https://www.mongodb.org/dl/linux/)

**1.把mongodb-linux-x86_64-3.6.16.tgz通过Xftp5上传至```/usr/local```目录下并解压解压**

```
tar -zxvf mongodb-linux-x86_64-3.6.16.tgz
```



**2.重命名**

```
mv mongodb-linux-x86_64-3.6.16 mongodb
```

**3.编辑 ```/etc/profile ```配置文件使用以下命令**

```
vim /etc/profile
```

**4.添加以下语句**
```
export MONGODB_HOME=/usr/local/mongodb
export PATH=$MONGODB_HOME/bin:$PATH
```



**5.使修改的配置生效**

```
source /etc/profile
```



**6.在```/usr/local/mongodb```文件夹路径下创建数据库文件夹以及日志文件夹**

```
mkdir -p data
```
```
mkdir -p log
```
```
touch log/mongodb.log
```



**7.在 ```/usr/local/mongodb/bin```文件夹路径下创建数据库配置文件mongodb.conf**
```
vim mongodb.conf
```

**8.添加以下内容**
```
# 数据库路径 

dbpath=/usr/local/mongodb/data 

#日志文件路径 

logpath=/usr/local/mongodb/log/mongodb.log 

#表示日志文件末尾追加日志

logappend=true 

#启用端口号、及绑定ip

port=27017

bind_ip=0.0.0.0

 #是否在后台运行

 fork=true
```



**9.通过配置文件启动mongoDB，在 ```/usr/local/mongodb/bin ```目录下运行以下命令**
```
mongod -f ./mongodb.conf 
```



