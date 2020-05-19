---
layout:     post
title:      Golang Socket
subtitle:   学习Golang TCP通信
date:       2020-5-18
author:     xiaomaike
header-img: img/post-bg-golang.jpg
catalog: true
tags:
    - Golang
    - TCP/UDP
---

**service.go**
```
package main

import (
	"net"
	"fmt"
)

// TCP service

func main() {
	// 1.监听本地端口
	linser, err := net.Listen("tcp", "127.0.0.1:27666")
	if err != nil{
		fmt.Println("Linsten 127.0.0.1:27666 faild, error of:", err)
		return
	}

	// 2.等待客户端连接
	client_conn, err := linser.Accept()
	if err != nil {
		fmt.Println("conn error of:", err)
		return
	}

	// 3.接收客户端信息
	var msg[128]byte
	n, err := client_conn.Read(msg[:])
	if err != nil {
		fmt.Println("Read error of:", err)
		return
	}
	fmt.Println("The client msg of:", string(msg[:n]))

	// 4.给客户端回应消息
	_, err =client_conn.Write([]byte("666!"))
	if err != nil{
		fmt.Println("Write err of:", err)
        return
	}

	client_conn.Close()
	linser.Close()
}

```
**client.go**
```
package main

import (
	"net"
	"fmt"
)

// TCP client

func main() {
	// 1.连接服务端
	conn_service, err := net.Dial("tcp", "127.0.0.1:27666")
	if err != nil {
		fmt.Println("conned 127.0.0.1:27666 faild of:", err)
		return
	}

	// 2.给服务端发生消息
	_, err = conn_service.Write([]byte("Hello World!"))
	if err != nil {
		fmt.Println("Wirte err of: ", err)
		return
	}

	// 3.接收服务端消息
	var msg [128]byte // 声明一个接收信息的变量
	n, err := conn_service.Read(msg[:])
	if err != nil {
		fmt.Println("Read error of:", err)
        return
	}
	fmt.Println("The service msg:",string(msg[:n]))

	// 4.关闭连接
	conn_service.Close()
}

```