---
layout:     post
title:      Golang Map与sync.Map
subtitle:   学习Golang Map与sync.Map的区别
date:       2020-5-18
author:     xiaomaike
header-img: img/post-bg-golang.jpg
catalog: true
tags:
    - Golang
---

**内置的map**
```
package main

import (
	"strconv"
	"fmt"
	"sync"
)

var map_obj = make(map[string]int) // 应对高并发时不安全

// 定义设置map数据类型的方法
func set(key string, val int)  {
	map_obj[key] = val
}

// 定义读取map数据类型的方法
func get(key string) int  {
	return map_obj[key]
}

func main() {
	//map_obj := make(map[string]int, 6)
	//map_obj["name"] = 666

	//var map_obj  map[string]int
	//fmt.Printf("%#v", map_obj) // map[string]int(nil)

	//var map_obj sync.Map
	//map_obj.Store("name","xiaomaike")
	//val, _ := map_obj.Load("name")
	//fmt.Println(val)

	var wg sync.WaitGroup

	for i := 0; i< 200; i++ {
		wg.Add(1)
		go func(n int){
			defer wg.Done()
			key :=strconv.Itoa(n)
			set(key, n)
			fmt.Println(key, get(key))
		}(i)
	}

	wg.Wait()
}

```
**增强版map(sync.Map)**
```
package main

import (
	"strconv"
	"fmt"
	"sync"
)


func main() {
	map_obj := sync.Map{} // sync.Map{} 内置并发版
	wg := sync.WaitGroup{}

	for i := 0; i< 2000; i++ {
		wg.Add(1)
		go func(n int){
			defer wg.Done()
			key :=strconv.Itoa(n)
			map_obj.Store(key, n)  // Store() => sync.Map设置map类型的方法
			read, _:= map_obj.Load(key) // Load => sync.Map 读取map类型的方法
			fmt.Println(key, read)
		}(i)
	}

	wg.Wait()
}

```
**使用互斥锁实现的增强版map**
```
package main

import (
	"strconv"
	"fmt"
	"sync"
)

var map_obj = make(map[string]int)

// 定义设置map数据类型的方法
func set(key string, val int)  {
	map_obj[key] = val
}

// 定义读取map数据类型的方法
func get(key string) int  {
	return map_obj[key]
}

func main() {
	//map_obj := make(map[string]int, 6)
	//map_obj["name"] = 666

	//var map_obj  map[string]int
	//fmt.Printf("%#v", map_obj) // map[string]int(nil)

	//var map_obj sync.Map
	//map_obj.Store("name","xiaomaike")
	//val, _ := map_obj.Load("name")
	//fmt.Println(val)

	var wg sync.WaitGroup
	var lock sync.Mutex // 定义一把锁对象

	for i := 0; i< 2000; i++ {
		wg.Add(1)
		go func(n int){
			defer wg.Done()
			key :=strconv.Itoa(n)

			lock.Lock() // 加入互斥锁操作,保证 map 应对高并发时的不安全性
			set(key, n)
			lock.Unlock()

			fmt.Println(key, get(key))
		}(i)
	}

	wg.Wait()
}

```