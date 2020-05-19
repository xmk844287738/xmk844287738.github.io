---
layout:     post
title:      Vue node js安装
subtitle:   Vue node js安装（win环境下）
date:       2020-5-18
author:     xiaomaike
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - Vue
---

### node.js安装(转载 https://www.jianshu.com/p/7f60fc39bab8)

### 1.(https://nodejs.org/en/download/)下载安装包

### 2.选中盘符进行安装(默认安装)


### 3.更换 npm 镜像源(更换为 cnpm 淘宝的镜像源)

win+R 依次运行以下指令

```swift
npm install -g cnpm --registry=https://registry.npm.taobao.org
```



### 4.将cnpm的执行路径添加到windows环境变量即可

```cpp
真正的执行路径是D:\Program Files\nodejs\node_global （具体以本机nodejs的安装位置为准）
```

### 5. vue安装

```bash
cnpm install vue
```

### 6.安装vue-cli脚手架构建工具

```
# 全局安装 vue-cli
$ cnpm install --global vue-cli
```



### 7.创建一个基于 webpack 模板的新项目

**切换至目录文件夹下, cmd 运行以下指令**

```
vue init webpack my-project
```



### 8.进行项目初始化命令操作
![输入图片说明](https://upload-images.jianshu.io/upload_images/2891127-6eff4683419a11e3.png?imageMogr2/auto-orient/strip|imageView2/2/w/595/format/webp "在这里输入图片标题")

### 9.安装项目依赖

```
cnpm install
```

### 10.启动项目

```
npm run dev
```