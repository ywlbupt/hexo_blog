title: python-basic~isinstance与type数据类型检查
date: 2017-05-17 23:14:46
category: [isinstance, type]
tags:
description:
---
[Toc]

* 使用isinstance进行数据类型检查，而不是Type
``` python
a = 1
b = 0.1
isinstance(a, (int, float))
# True
isinstance(b, (int, float))
# True
class C():
    pass

c = C()
isinstance(c, (int,C))
# True
```

* type对于subclass之类的就不行了

在python 3中，`isinstance`可以处理子类的类型检查，而Type不行
```python
class A():
    pass

class C(A):
    pass
```
* 摆事实讲证据
``` bash
>>> type(a)
<class '__main__.A'>
# a 是类A的实例
>>> type(A)
<class 'type'>
>>> type(C)
<class 'type'>
# 任意类本身是type的实例
>>> type(a) == A
True
>>> type(C) == A
False
>>> isinstance(a, C)
False
>>> isinstance(a, A)
True
>>> isinstance(c, A)
True

```

## REF

1. [Duck typing](https://en.wikipedia.org/wiki/Duck_typing)
