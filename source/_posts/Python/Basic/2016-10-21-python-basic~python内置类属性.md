title: python-basic~python定制类及类属性
date: 2016-10-21 14:19:53
category: [Python, Basic]
tags:
description:
----
[Toc]

### 基本类属性 

*   `__class__`: 类名字，类函数内部可由此调用**自身类属性** `self.__class__.property`
*   `__name__`: 类名，外部调用，一般通过实例访问属性
    *   在测试单文档中，通常会有这么一个语句 `if __name__ == "__main__":`
        *   如果是直接运行该文档，则 `__name__= "__main__"`
        *   如果是从其他文件如`HelloPython.py`中`import`该模块，
            则`__name__ = "HelloPython"`
`__module__`: 类定义所在的模块（类的全名是`__main__.className`，如果类位于一个导入模块mymod中，那么`className.__module__` 等于 mymod）
__dict__ : 类的属性（包含一个字典，由类的数据属性组成）
__doc__ :类的文档字符串
__bases__ : 类的所有父类构成元素（包含了一个由所有父类组成的元组）

### 基础重载方法

基础重载方法
下表列出了一些通用的功能，你可以在自己的类重写：
序号  方法, 描述 & 简单的调用
*	`__init__ ( self [,args...] )` 构造函数
    简单的调用方法: `obj = className(args)`
*	`__del__( self )` 析构方法, 删除一个对象
    简单的调用方法 : `dell obj`
*	`__repr__( self )` 转化为供解释器读取的形式
    简单的调用方法 : `repr(obj)`
*	  `__str__( self )` 用于将值转化为适于人阅读的形式
    简单的调用方法 : `str(obj)`
    也可以通过`print(object)`打印
*	`__cmp__ ( self, x )` 对象比较
    简单的调用方法 : `cmp(obj, x)`
*   `__len__(self)`返回个数

### 运算符重载

*	`__add__(self, other)`

### TBD

`__import__`

`__setattr__`

### 生成器 iterator

*   `__iter__` 和 `__next__`

#### 像list,dict一样工作

*   `__getitem__`, 
    *   `isinstance(n, slice)` 切片处理判断
        *   把`slice`也当做一个对象处理
        *   `start = n.start` ,`stop = n.stop` 
    *   对负数进行处理判断

*   `__setitem__`方法，把对象视为`list dict`来对集合赋值
*   `__delitem__`方法，删除某元素

### 属性＠property

装饰器（decorator）可以给函数动态加上功能

对于类的方法，装饰器一样起作用。Python内置的@property装饰器就是负责把一个方法变成属性调用
```python
class Student(object):

    @property
    def score(self):
        return self._score

    @score.setter
    def score(self, value):
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')
        if value < 0 or value > 100:
            raise ValueError('score must between 0 ~ 100!')
        self._score = value
```
把一个方法`score()`变成属性，只需要加上`@property`便可。可以在方法中添加参数检查等

此时，`@property`又创建了另一个装饰器`@score.setter`，负责把一个setter()方法
变成属性赋值，于是可以如下应用

```bash
>>> s = Student()
>>> s.score = 60 # OK，实际转化为s.set_score(60)
>>> s.score # OK，实际转化为s.get_score()
60
>>> s.score = 9999
Traceback (most recent call last):
  ...
ValueError: score must between 0 ~ 100!
```

注意到这个神奇的`@property`，我们在对实例属性操作的时候，就知道该属性很可能不是直接暴露的，而是通过getter和setter方法来实现的。

**还可以定义只读属性，只定义getter方法，不定义setter方法就是一个只读属性**

### __getattr__ 动态调用类方法与属性


