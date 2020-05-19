---
layout:     post
title:      Centos7 golandIDE debug配置
subtitle:   Centos7 golandIDE debug的相关配置
date:       2020-5-17
author:     xiaomaike
header-img: img/post-bg-centos.jpg
catalog: true
tags:
    - Centos7
    - Golang
---

### 1.
go get -u github.com/go-delve/delve/cmd/dlv 

**[github go-delve debug配置插件项目相关信息(Linux版)](https://github.com/go-delve/delve/blob/master/Documentation/installation/linux/install.md)**

```
(或参考
git clone https://github.com/go-delve/delve.git $GOPATH/src/github.com/go-delve/delve
cd $GOPATH
go install github.com/go-delve/delve/cmd/dlv
)
```

### 2.
修改Goland的配置，Help->Edit Custom Properties编辑配置文件，增加dlv的路径配置，文件键入以下内容：
```
dlv.path=/你的GOPATH路径/bin/dlv
```