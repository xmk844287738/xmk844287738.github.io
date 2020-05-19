---
layout:     post
title:      JavaScript 原生Ajax请求
subtitle:   实现JavaScript 原生Ajax请求
date:       2020-5-17
author:     xiaomaike
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - JavaScript
---


### get 请求
```
<script type="text/javascript">
	let request = new XMLHttpRequest() // 1

	request.open('get', 'http://127.0.0.1:5000/infro', true) // 2

	request.send() //3

	request.onload() = function() { // 4
		console.log(request.responseText)
	}
</script>
```

### post 请求
```
<script type="text/javascript">
	let request = new XMLHttpRequest() // 1

	request.open('post', 'http://127.0.0.1:5000/infro', true) // 2

	//post需要设置请求头
    xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded;charset-UTF-8"); // 3
    //发送请求
    xhr.send("type=1&kk=2"); // 4

	request.onload() = function() { // 5
		console.log(request.responseText)
	}
</script>
```