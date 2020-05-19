---
layout:     post
title:      Hello Vue
subtitle:   Vue 显示一个基础的 Vue.js 界面
date:       2020-5-18
author:     xiaomaike
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - Vue
---

### 利用vue.js javascript文件显示一个基础vue页面
**注意！！！
vue.js 文件需预先下载（[https://cn.vuejs.org/v2/guide/installation.html](https://cn.vuejs.org/v2/guide/installation.html))并放到项目的```js```文件夹下**

```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>Vue_demo1</title>
	</head>
	<body>
		<div id="app">
			hello:{{ hello_str }}
		</div>
	</body>
	<script src="../js/vue.js" type="text/javascript" charset="utf-8"></script>
	<script type="text/javascript">
		//定义全局变量vn
		let vn = new Vue({
            el: '#add',
            data: {
                hello_str: 'Hello Vue'
            }
        });
		
	</script>
</html>

```

