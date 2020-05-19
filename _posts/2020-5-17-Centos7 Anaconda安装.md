---
layout:     post
title:      Centos7 Anaconda安装
subtitle:   Centos7 Anaconda安装步骤
date:       2020-5-17
author:     xiaomaike
header-img: img/post-bg-centos.jpg
catalog: true
tags:
    - Centos7
    - Anaconda
---


### 1.在[清华镜像源](https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/)下载 [Anaconda3-4.4.0-Linux-x86_64.sh](https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/Anaconda3-2019.10-Linux-x86_64.sh)文件，上传至```/root/```目录下，在目标文件路径下，键入以下命令进行安装过程：
```
bash Anaconda3-4.4.0-Linux-x86_64.sh
```

**安装过程中会有两次提示选择yes/no，都选yes就ok。**

### Tips:
安装完anaconda，修改```~/.bash_profile```文件，添加anaconda的bin目录到PATH中
##### （如果最后一个提示你yes/no，选择yes就不需要更改）
**更改的过程：**
```
vi /root/.bashrc
```
**文件最后添加**
```

# added by Anaconda3 4.4.0 installer
export PATH="/root/anaconda3/bin:$PATH"
```
### 2.进入 conda  虚拟环境
**Tips: （第一次需要先激活，命令行键入以下命令： ```source /root/.bashrc```）	进入conda默认虚拟环境（base）**
```
conda activate
```
**退出conda虚拟环境**
```
conda deactivate  
```


### 3.为Anaconda添加TUNA镜像，命令行键入以下命令：
```
   conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
   conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main

# 设置搜索时显示通道地址

  conda config --set show_channel_urls yes
```
#### 拓展：
**conda 下创建虚拟环境**
##### 3.1创建虚拟环境(选择 python 版本 ) 命令行下键入以下命令语句：
```
conda create -n your_env_name python=3.7 
```

###### 3.2首先激活虚拟环境,	命令行下键入以下命令语句：(提示 ```conda deactivate``` 退出虚拟环境)
```
conda activate your_env_name 
```
### 4.开启远程访问jupyter
##### 4.1.切换到默认的anaconda安装路径中，生成配置文件
```
cd /root/anaconda3/etc/ 
```
```
jupyter notebook --generate-config
```

##### 4.2生成加密密码
**打开ipython(需提前安装该 ipython 库)，命令行键入以下命令：**
```
ipython
```

**在ipython中输入以下代码，然后回车输入我们的密码，会输出加密后的密码（输两次密码）**
```
from notebook.auth import passwd
passwd()
```

**将输出的sha1:......复制，一会我们需要用到 （将加密后的密码先复制 储存）
exit()  退出 ipython 交互环境**

##### 4.3修改默认配置文件，命令行键入以下命令:
```
vi /root/.jupyter/jupyter_notebook_config.py
```

**在文件末尾加入如下图所示的代码(vi  命令行模式下，shift+g 直接跳到文件末尾)**
```

c.NotebookApp.allow_remote_access = True # 允许远程
c.NotebookApp.ip = '*' # 允许访问此服务器的 IP，星号表示任意 IP
c.NotebookApp.password = u'sha1:f5...................' # 之前生成的密码 hash 字串
c.NotebookApp.open_browser = False # 运行时不打开本机浏览器
c.NotebookApp.port = 8888 # 使用的端口，随意设置
c.NotebookApp.enable_mathjax = True # 启用 MathJax
c.NotebookApp.notebook_dir = u'你要保存的工作路径'

```

##### 4.4启动jupyter notebook
以下两种启动方式选一种：（推荐第二种，因为第一种一旦关闭xshell远程连接jupyter就会关闭）
**a.直接启动:**

###### 切换到jupyter目录下:
```
cd /root/anaconda3/etc/jupyter
```

###### 启动：
```
jupyter notebook --allow-root
```

**b.后台启动:**
```
nohup jupyter notebook  --allow-root > jupyter.log 2>&1 &
```

##### 4.5开放相应的端口号:
**如果我们服务器的防火墙是开着的话是远程访问不到jupyter的**，所以我们要开放相应的端口，如我这配置的是默认的8888端口号，**(如果是阿里云服务器需在用户的控制面板下开启  该8888端口号)** 以下是开放端口号指令:

**添加端口，命令行键入以下命令：**
```
firewall-cmd --zone=public --add=8888/tcp --permanent
```
###### 重新载入，命令行键入以下命令：
```
firewall-cmd --reload
```

