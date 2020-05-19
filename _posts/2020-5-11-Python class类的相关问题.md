---
layout:     post
title:      Python class类的相关问题
subtitle:   学习Python class类的相关问题
date:       2020-5-11
author:     xiaomaike
header-img: img/post-bg-python.jpg
catalog: true
tags:
    - Python
---

```python
def __new__(cls, *args, **kwargs) 先于 def __init__(self) 执行
```



```python

class Sample_class(object):
    
    def __new__(cls, *args, **kwargs):	# __new__ 一定要有 返回值
        # 业务操作
        
        # 调用父类的（object）的new方法，返回一个Sample_class实例，这个实例传递给init的self参数
        return super().__new__(cls, *args, **kwargs) 	
    
    
    def __init__(self):
        pass
    
        
    def __repr__(self):
        pass
    
    # 描述符协议(三个)
    def __get__(self):
        pass
    
    def __set__(self):
        pass
    
    def __delete__(self):
        pass
    
    
```

  __repr__  和 __sttr__  对 class 类的描述(https://www.cnblogs.com/aomi/p/7026532.html)

1.对于一个object来说，__str__和__repr__都是返回对object的描述，只是，前一个的描述简短而友好，后一个的描述，更细节复杂一些。

2.对于有些数据类型，__repr__返回的是一个string，比如：str('hello') 返回的是'hello',而repr('hello')返回的是“‘hello’”



### 情况一：

```python
class Item():
    def __init__(self, name):
        self._name = name

    def __str__(self):
        return "Item's name is :"+self._name


print((Item("Car"),)) # =>  元组对象(<__main__.Item object at 0x000001EBAB8832E8>,)
```



### 情况二：

```python
class Item():
    def __init__(self, name):
        self._name = name

    # def __str__(self):
    #     return "Item's name is :"+self._name


    def __repr__(self):
        return self.__class__.__name__ + ": Item's name is :" + self._name


print((Item("Car"),)) # => 元组对象 (Item: Item's name is :Car,)
```

