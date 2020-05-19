---
layout:     post
title:      Python Socket
subtitle:   学习Python Socket
date:       2020-5-16
author:     xiaomaike
header-img: img/post-bg-python.jpg
catalog: true
tags:
    - Python
    - TCP/UDP
---

**udp => 写信模式**

**tcp => 打电话模式（需要建立链接，严格区分服务端、客户端，复杂而稳定）**

# udp写信模式
```
# client.py
import socket


client = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
IP_port = ('127.0.0.1', 20776)
msg = 'Hello World!'
client.sendto(msg.encode('utf-8'), IP_port) #  客户端先发后收
msg, service_addr = client.recvfrom(1024)
print('远端服务器service_ip:{service_addr};msg:{msg}'.format(service_addr=service_addr, msg=msg))

while True:
    msg = str(input('Please a string:'))
    client.sendto(msg.encode('utf-8'), ('127.0.0.1', 20776))
```
```
# service.py
import socket


service = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

service.bind(('127.0.0.1', 20776))

while True:
    msg, client_addr = service.recvfrom(1024) # 服务端先收后发
    print('来自client_addr:{client_addr};msg:{msg}'.format(client_addr=client_addr, msg=msg.decode('utf-8')))
    msg = 'client HW!'
    service.sendto(msg.encode('utf-8'), client_addr)
```
**注意：先启动service.py, 再启动client.py**

# udp写信模式，多线程版
```
# mulittask_udp_client.py
# 客户端 先发后收

import socket
import threading

# 发消息线程
def send_msg(udp_socket_client):
    while True:
        send_data = input('请输入要发送的信息:')
        udp_socket_client.sendto(send_data.encode('utf-8'), ('127.0.0.1', 27666))


# 收消息线程
def recv_msg(udp_socket_client):
    while True:
        recv_data = udp_socket_client.recvfrom(1024)
        print(f'来自{recv_data[1]}的信息:{recv_data[0].decode("utf-8")}')

def clientMain():
    udp_socket_client = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

    udp_socket_client.bind(('127.0.0.1', 27667))

    t_send = threading.Thread(target=send_msg, args=(udp_socket_client, ))
    t_recv = threading.Thread(target=recv_msg, args=(udp_socket_client, ))

    t_send.start()
    t_recv.start()




if __name__ == '__main__':
    clientMain()
```

```
#mulittask_udp_service.py
# 服务端 先收后发

import socket
import threading

# 收消息线程
def recv_msg(udp_service_socket):
    while True:
        recv_data = udp_service_socket.recvfrom(1024)
        print(f'来自{recv_data[1]}的信息:{recv_data[0].decode("utf-8")}')


# 发消息线程
def send_msg(udp_service_socket):
    while True:
        send_data = input('请输入要发送的信息:')
        udp_service_socket.sendto(send_data.encode('utf-8'), ('127.0.0.1', 27667))


def serviceMain():
    udp_service_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

    udp_service_socket.bind(('127.0.0.1', 27666))

    t_recv = threading.Thread(target=recv_msg, args=(udp_service_socket,))
    t_send = threading.Thread(target=send_msg, args=(udp_service_socket,))

    t_recv.start()
    t_send.start()




if __name__ == '__main__':
    serviceMain()
```
**注意：先启动mulittask_udp_service.py, 再启动mulittask_udp_client.py**

# tcp打电话模式

```
# client.py
import socket


def clientMain():

    # 1.创建 tcp_client_socket 套接字
    tcp_client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

    # 2.链接服务器
    service_addr = ('127.0.0.1', 8080)
    tcp_client_socket.connect(service_addr)

    # 3.发送/接收数据 处于阻塞状态
    send_data = input("请输入要发送的数据信息:")
    tcp_client_socket.send(send_data.encode('utf-8'))

    # 接收服务端的消息
    service_msg = tcp_client_socket.recv(1024)
    print(f"来自服务端的消息:{service_msg.decode('utf-8')}")

    # 4.关闭 tcp_client_socket 套接字
    tcp_client_socket.close()

if __name__ == '__main__':
    clientMain()
```
```
#service.py
import socket


def serviceMain():

    # 1.创建 tcp_service_socket 套接字
    tcp_service_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

    # 2.绑定服务器ip及port
    tcp_service_socket.bind(('127.0.0.1', 8080))

    # 3.设置为监听状态，有主动连接变为被动连接(默认为主动连接)
    tcp_service_socket.listen(128)

    # 4.等待连接 处于阻塞状态, 有连接则 解阻塞
    new_tcp_client_socket, tcp_client_addr = tcp_service_socket.accept()

    print(f'客户端的ip及port:{tcp_client_addr}')
    # 5.(接收客户端信息) 等待客户端发送数据信息, 处于阻塞状态
    client_msg = new_tcp_client_socket.recv(1024)
    print(f'客户端的消息:{client_msg.decode("utf-8")}')
    # 向客户端发送数据信息
    new_tcp_client_socket.send(b'Hello World!')

    # 6.关闭 new_tcp_client_socket 套接字
    new_tcp_client_socket.close()

    # 7.关闭 tcp_service_socket 套接字
    tcp_service_socket.close()

if __name__ == '__main__':
    serviceMain()
```
**注意：先启动service.py, 再启动client.py**

# tcp打电话模式，多线程版
```
# mulittask_tcp_client.py

import threading
import socket

clicent_sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# 连接服务器 IP 及 port
sericve_ip_port = ('127.0.0.1', 20776)

# 与客户端建立连接（三次握手）
clicent_sock.connect(sericve_ip_port)


# 向服务器发送消息
def send_tosrvice():
    while True:
        msg_tosericve = str(input('客户端：Please a string:'))
        clicent_sock.sendto(msg_tosericve.encode('utf-8'), sericve_ip_port)


# 接受服务器消息
def recv_fromsrvice():
    while True:
        msg_fromSericve = clicent_sock.recv(1024)
        print('\n来自sericve_addr:{sericve_addr} sericve_msg:{serice_msg}'.format(sericve_addr=sericve_ip_port,serice_msg=msg_fromSericve.decode('utf-8')))
        # print(msg_fromsericve.decode('utf-8'))



if __name__ == '__main__':    
    send_work = threading.Thread(target=send_tosrvice) # 发消息线程
    recv_work = threading.Thread(target=recv_fromsrvice) # 收消息线程


    recv_work.start()
    send_work.start()

    # recv_work.join()
    # send_work.join()
```
```
# mulittask_tcp_service.py

import socket
import threading

tcp_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# 绑定服务器 IP 及 port
serice_ip_port = ('127.0.0.1', 20776)
tcp_socket.bind(serice_ip_port)

# 监听客户端的连接情况
tcp_socket.listen(5)

# 返回客户端连接及客户端的IP、Port
client_conn, client_addr = tcp_socket.accept()


# 接受客户端信息
def recv_fromclicent():
    while True:
        msg_fromClient = client_conn.recv(1024)
        print('\n来自client_addr:{client_addr} client_msg:{msg}'.format(client_addr=client_addr, msg=msg_fromClient.decode('utf-8')))


# 向客户端发送消息
def send_toclicent():
    while True:
        msg_toClicent = str(input('服务端：Please a string:'))
        # msg = '来自远程服务端{serice_ip_port}的问候： {service_msg}'.format(serice_ip_port=serice_ip_port, service_msg=service_msg)
        client_conn.send(msg_toClicent.encode('utf-8'))


if __name__ == '__main__':
    recv_work = threading.Thread(target=recv_fromclicent) #收消息线程
    send_work = threading.Thread(target=send_toclicent) # 发消息线程

    send_work.start()
    recv_work.start()


    # recv_work.join()
    # send_work.join()
```
**注意：先启动mulittask_tcp_service.py, 再启动mulittask_tcp_client.py**