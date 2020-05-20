---
layout:     post
title:      Python SSH工作流程
subtitle:   利用Python 实现SSH工作流程(客户端、服务端)
date:       2020-5-20
author:     xiaomaike
header-img: img/post-bg-python.jpg
catalog: true
tags:
    - Python
---

#SSH客户端
```
import socket

# 1. 创建符合TCp协议的手机
client = socket.socket(socket.AF_INET,socket.SOCK_STREAM)

# 2. 拨号
client.connect(('192.168.11.210',8000))

while True:
    msg = input('please enter your msg')  # dir
    # 3. 发送消息
    client.send(msg.encode('utf8'))

    # 4. 接收消息
    data = client.recv(10)
    print(data.decode('gbk'))


```

#SSH服务端
```
import socket
import subprocess

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

server.bind(('192.168.11.210', 8000))
server.listen(5)

print('start...')
while True:
    conn, client_addr = server.accept()
    print(client_addr)

    while True:
        try:
            cmd = conn.recv(1024)  # dir
            print(cmd)

            # 帮你执行cmd命令,然后把执行结果保存到管道里
            pipeline = subprocess.Popen(cmd.decode('utf8'),
                                        shell=True,
                                        stderr=subprocess.PIPE,
                                        stdout=subprocess.PIPE)

            stderr = pipeline.stderr.read()
            stdout = pipeline.stdout.read()

            conn.send(stderr)
            conn.send(stdout)

        except ConnectionResetError:
            break

```