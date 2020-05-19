---
layout:     post
title:      Centos7 golang配置
subtitle:   Centos7 golang环境变量配置
date:       2020-5-17
author:     xiaomaike
header-img: img/post-bg-centos.jpg
catalog: true
tags:
    - Centos7
    - Golang
---

## Centos7操作流程
### tips:[golang 南京大学镜像下载站点](http://mirrors.nju.edu.cn/golang/)
### 1、下载 [go1.14.linux-amd64.tar.gz](http://mirrors.nju.edu.cn/golang/go1.14.linux-amd64.tar.gz),从win环境利用Xftp工具本地上传至```/usr/local```目录, 解压go1.14.linux-amd64.tar.gz, 解压命令如下：
```
tar -C /usr/local -xzf go1.14.linux-amd64.tar.gz
```

### 2、配置环境变量
```
vi /etc/profile 
```

### 3、在环境变量最后添加GOROOT环境变量，GOROOT变量为go的安装目录，类似java的jdk安装目录，GOPATH类似eclipse的workspace：
```
export GOROOT=/usr/local/go
export PATH=$PATH:$GOROOT/bin
export GOPATH=/root/go
export PATH=$PATH:$GOPATH/BIN
```

### 4、刷新环境变量：
```
source /etc/profile
```

### 5、查看go是否安装成功
```
go --version
```

### 6、下载 [goland](http://down-ww3.newasp.net/pcdown/soft/soft1/goland.2018.linux.zip) ，解压goland 2018.1文件（注意：提前安装了图形操作界面）
**在win环境下解压即可得到goland 2018.1的```tar.gz``` 包, 利用Xftp工具本地上传至```/root/mysoft```目录；通过命令行运行```/root/mysoft/GoLand-2018.1.3/bin```下的```goland.sh```主程序，然后在顶部菜单栏选择**

**tools -> Create desktop entry**
即可在桌面新建goland快捷方式。