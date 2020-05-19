---
layout:     post
title:      Golang Once()
subtitle:   学习Golang sync.Once()的应用
date:       2020-5-18
author:     xiaomaike
header-img: img/post-bg-golang.jpg
catalog: true
tags:
    - Golang
---

```
package main

// once 只执行一次，底层实现：应用互斥锁机制

import (
	"sync"
	"fmt"
)


var once sync.Once // 返回一个结构体指针类型

func fun1(endNum int)  {
	res := 0
	for i :=0; i <= endNum; i++ {
		res += i
	}
	fmt.Println("res:", res)
}

func main() {

	endNum := 100

	// Do 函数接受一个无参数函数作为输入
	once.Do(func(){ fun1(endNum) })
}

```