title: python-basic~python2to3
date: 2016-09-21 23:04:50
category: [Python,Basic]
tags:
description:
---
[Toc]

【参考文档】
1. [Supporting Python3 -- 常见迁移问题](https://my.oschina.net/soarwilldo/blog/510752)
2. [Writing Forwards Compatible Python Code](http://lucumr.pocoo.org/2011/1/22/forwards-compatible-python/)

## python2和python3的共存

#### reload

**共存**
* **python3** 中，使用 `imp.reload(module)` : Reload a previously imported module. 
  * 替代python2中的reload()函数


#### exception

基本语法格式都是
```python
Try... except... finally...
```

不同的地方在于`exception`的语法
* python3:  `except ERRORTYPE as e:`
* python2: `except ERRORTYPE , e:`

TODO :

如果你需要同时支持Python 2和Python 3，你很可能需要根据Python版本使用条件语句。在这本书里，我一贯使用sys.version_info元组和(3,)元组比较。但是还有一些其他的方式来做相同的测试，像sys.version_info[0] !=3或者使用sys.version字符串。使用哪 一个主要是个人爱好。如果你最终做了很多测试，设定一个常量是个好主意：

>>> import sys
>>> PY3 = sys.version_info > (3,)
然后你可以在软件的剩余部分仅使用PY3常量。

#### python 3 返回迭代对象；python 2返回列表

python 3 中返回的是可迭代的对象，而不像 python 3 中返回的是列表
* 对于迭代的对象，可以通过`list()`简单的将迭代对象转换成列表
  * map/filter
  * zip()
  * dict.key(), dict.value(), dict.item() 方法
