---
layout:     post
title:      Centos7 安装docker
subtitle:   Centos7 安装docker、部署flask项目教程
date:       2020-5-17
author:     xiaomaike
header-img: img/post-bg-centos.jpg
catalog: true
tags:
    - Centos7
    - Docker
---

# 前叙
传统虚拟机：
启动慢(分钟级) 占用内存大
docker:
启动快(秒钟级)  轻量级虚拟机(Linux)

## docker三特性：
**镜像、容器、仓库**

### 1.查看Linux内核：
```
cat /etc/redhat-release
```
**或**
```
uname -r
```

### 2.使用root 权限登录 CentOS。确保 yum 包更新到最新。
```
yum -y update
```

### 3.卸载旧版本（在如果安装过旧版本的前提下）
```
yum remove docker docker-common docker-selinux docker-engine  docker-client docker-client-latest
yum remove docker-ce
```

### 4.安装驱动依赖，yum-util 提供yum-config-manager功能，另外两个是devicemapper驱动依赖的
```
yum install -y yum-utils device-mapper-persistent-data lvm2
```

### 5.根据你的发行版下载repo文件:
```
wget -O /etc/yum.repos.d/docker-ce.repo https://download.docker.com/linux/centos/docker-ce.repo
```
### 6.把软件仓库地址替换为 TUNA:
```
sed -i 's+download.docker.com+mirrors.tuna.tsinghua.edu.cn/docker-ce+' /etc/yum.repos.d/docker-ce.repo
```

### 7.安装docker
**清缓存**
```
yum makecache fast 
```
 **安装docker-ce**
```
yum install docker-ce
```
**注意！！！**
由于repo中默认只开启stable仓库，故这里安装的是最新稳定版
```
yum install docker-ce  17.12.0
```
安装其他版本：yum install 版本
```
yum install <FQPN>  
```

### 8.启动并加入开机启动
```
systemctl start docker
systemctl enable docker
```

### 9.验证安装是否成功
**如果有client和service两部分，则表示docker安装启动都成功了**
```
docker version
```

### 10.根据项目的python版本 pull 相应的 python 版本镜像 (现阶段项目以 pyhton 3.7.3 为主)
```
docker pull pyhton:3.7.3
```

### 11.在项目文件主目录下；vi gunicorn.conf.py 新文件；键入以下内容：
```
workers = 10
worker_class = "gevent"
bind = "0.0.0.0:8888"
```

### 12.以gunicorn 方式启动项目主程序，测试项目能否正常启动：
```
gunicorn microblog:app -c ./gunicorn.conf.py
```

### 13.vi Dockerfile 新建 Dockerfile 文件；键入以下内容：
```
FROM python:3.7.3

WORKDIR /usr/src/app

COPY . .
RUN pip install -i https://pypi.tuna.tsinghua.edu.cn/simple --no-cache-dir -r requirements.txt
COPY . .

CMD ["gunicorn", "my_flask_demo:app", "-c", "./gunicorn.conf.py"]
```

### 14.将项目全部文件拷贝至 /usr/src/app路径下,命令行键入以下命令：
```
cp /root/microblog/* /usr/src/app
```

### 15.制作镜像(image_name 自定义)
```
docker build -t 'image_name'  .
```

### 16.查看制作完成的镜像
```
docker image ls
```

### 17.启动镜像
**a:直接启动**
```
docker run -it --rm -p 宿主机的port:容器的port  image_name
```

**b:以后台方式启动**
```
docker run -d -p 宿主机的port:容器的port --name image_name  image_name
```

### 18.停止镜像服务
```
docker stop image_name
```


