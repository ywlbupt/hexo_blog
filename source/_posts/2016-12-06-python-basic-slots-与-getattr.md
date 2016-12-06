title: python-basic~__slots__与__getattr__
date: 2016-12-06 08:21:20
category: [python, basic]
tags:
description:
---
[Toc]

### 结合使用 .__setattr__ 和 .__slots__
``` python
>>> class Foo3(object):
...     __slots__ = ('x')
...     def __setattr__(self, name, val):
...         if name in Foo.__slots__:
...             object.__setattr__(self, name, val)
...     def __getattr__(self, name):
...         return "Value of %s" % name
...
>>> foo3 = Foo3()
>>> foo3.x
'Value of x'
>>> foo3.x = 'x'
>>> foo3.x
'x'
>>> foo3.y
'Value of y'
>>> foo3.y = 'y'   # Doesn't do anything, but doesn't raise exception
>>> foo3.y
'Value of y'
```
