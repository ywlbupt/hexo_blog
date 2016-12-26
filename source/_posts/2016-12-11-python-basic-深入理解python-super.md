title: python-basic~深入理解python-super
date: 2016-12-11 16:04:11
category:
tags:
description:
---
[Toc]

### 参考文档
1. [理解python super](https://laike9m.com/blog/li-jie-python-super,70/)
2. [Python’s super() considered super!--Raymond Hettinger](https://rhettinger.wordpress.com/2011/05/26/super-considered-super/)
3. [MRO 官方文档](https://www.python.org/download/releases/2.3/mro/)

### super()

子类里访问父类的同名属性，而又不想直接引用父类的名字，因为说不定什么时候会去修改它，所以数据还是只保留一份的好。

还有更好的理由不去使用父类名字，详情参考文档[2]

在python 3.x中，可使用`super()`引用MRO中的下一个类（非父类）
``` python
class A:
  def m(self):
    print('A')

class B(A):
  def m(self):
    print('B')
    # super(B, self).m() # python 2.x中super需要参数
    super().m()

B().m()
```

### 关于super(cls , inst)

super并不是指的父类，而是MRO的下一个类
``` python
def super(cls, inst):
    mro =  inst.__class__.mro 
    return mro[mro.index[cls]+1]
```
两个参数 cls 和 inst 分别做了两件事： 
1. inst 负责生成 MRO 的 list 
2. 通过 cls 定位当前 MRO 中的 index, 并返回 mro[index + 1] 

MRO 代表着类继承的顺序

**在文档[2]中指出，如果你要用 super，那么最佳实践就是一串类全都用 super，每个类的方法提取自己要用的那个参数。**

``` python
>>> class A():
...     pass
...
>>> class B():
...     pass
...
>>> class C(A, B):
...     pass
...
>>> C.__mro__
(<class '__main__.C'>, <class '__main__.A'>, <class '__main__.B'>, <class 'object'>)

# C继承自A，所以这里A的位置不能在C的前面，报错
>>> class D(A,C):
...     pass
...
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: Cannot create a consistent method resolution
order (MRO) for bases A, C

>>> class D(C,A):
...     pass
...
>>> D.__mro__
(<class '__main__.D'>, <class '__main__.C'>, <class '__main__.A'>, <class '__main__.B'>, <class 'object'>)


>>> class D(C,B):
...     pass
...
>>> D.__mro__
(<class '__main__.D'>, <class '__main__.C'>, <class '__main__.A'>, <class '__main__.B'>, <class 'object'>)
```

### MRO - Method Resolution Order


