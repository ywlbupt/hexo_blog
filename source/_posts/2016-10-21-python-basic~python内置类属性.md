title: python-basic~python定制类及类属性
date: 2016-10-21 14:19:53
category: [Python, Basic]
tags:
description:
---
[Toc]

### 基本类属性 

__dict__ : 类的属性（包含一个字典，由类的数据属性组成）
__doc__ :类的文档字符串
__name__: 类名，外部调用，一般通过实例访问属性
__class__: 类名字，类函数可由此调用自身类属性 
__module__: 类定义所在的模块（类的全名是'__main__.className'，如果类位于一个导入模块mymod中，那么className.__module__ 等于 mymod）
__bases__ : 类的所有父类构成元素（包含了一个由所有父类组成的元组）

### 基础重载方法

基础重载方法
下表列出了一些通用的功能，你可以在自己的类重写：
序号  方法, 描述 & 简单的调用
* `__init__ ( self [,args...] )`
  构造函数
  简单的调用方法: `obj = className(args)`
* `__del__( self )`
  析构方法, 删除一个对象
  简单的调用方法 : `dell obj`
* `__repr__( self )`
  转化为供解释器读取的形式
  简单的调用方法 : `repr(obj)`
* `__str__( self )`
  用于将值转化为适于人阅读的形式
  简单的调用方法 : str(obj)
* `__cmp__ ( self, x )`
  对象比较
  简单的调用方法 : `cmp(obj, x)`

### 运算符重载

* `__add__(self, other)`
