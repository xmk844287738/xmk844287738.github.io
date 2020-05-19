---
layout:     post
title:      Centos7 mongoDB(版本3.6)数据迁移
subtitle:   win下的mongoDB(版本3.6)数据迁移至linux 操作步骤
date:       2020-5-17
author:     xiaomaike
header-img: img/post-bg-centos.jpg
catalog: true
tags:
    - Centos7
    - MongoDB
---

## win环境下导出数据：

### 1:导出所有数据库:

找到mongoDB主程序所在路径：例如	F:\Programefile\MongoDB\Server\3.6\bin  

```
双击运行 mongodump.exe 文件
```

### 2:导出指定数据库:
mongodump -h 192.168.142.130 -d
### 拓展资料
```
在cmd窗口切换至mongoDB主程序所在路径，运行以下命令
mongodump -h IP --port 端口 -u 用户名 -p 密码 -d 数据库 -o 文件存在路径

如果没有用户，可以去掉-u和-p。
如果导出本机的数据库，可以去掉-h。
如果是默认端口，可以去掉--port。
如果想导出所有数据库，可以去掉-d。
如果不指定-o，文件备份在当前目录下
```
## 注意！！！
**导出的数据文件在（你的安装盘符:\MongoDB\Server\3.6\bin\dump）下**

## linux环境下 mongorestore还原数据库:

### 1:恢复所有数据库到mongodb中:
在Linux下切换至mongoDB主程序所在路径，键入以下命令：
```
root@bin:# mongorestore --port 端口值  -d 数据库名 数据库备份文件的本地路径  
````
**这里的路径是数据库的备份路径**(从win 环境下上传**)**

## 2:还原指定的数据库, 键入以下命令:
```
root@bin:# mongorestore  --port 端口值  -d movie /root/dump/myblog/movie/  
```
**/root/dump/myblog/movie/  数据库的备份路径**
```
root@bin:# mongorestore --port 端口值 -d movie_new /root/dump/myblog/movie/  
```
**将movie还原movie_new数据库中**