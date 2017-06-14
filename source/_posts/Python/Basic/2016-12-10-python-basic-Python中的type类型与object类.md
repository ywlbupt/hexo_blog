title: python-basic~Python中的type与object
date: 2016-12-10 17:37:34
category: [Python, Basic]
tags:
    - object
    - type
description:
---
[Toc]

## python中的type与object

### Python中为什么要继承object类

* [python中为什么要继承object类](https://www.zhihu.com/question/19754936)
* [Python 新式类介绍 (包含 new-style-and-classic-classes 文档翻译)](http://www.kaka-ace.com/python2_new-style-and-classic-classes/)
* [根据 stackoverflow 答案引出的语言发明者 Guido 写的一篇文章](http://python-history.blogspot.com/2010/06/inside-story-on-new-style-classes.html)

py 2.2 后继承 object 的目的是使这个类成为 new style class, 

没有继承 object 的为传统 classic class,

在3.x里object已经做为所有东西的基类了

![img python object and type](/hexo_blog/images/python/python-object-type.png)
 
* 在Python 3.0中，用户定义的类对象是名为type的对象的实例，type本身是一个类
* 在Python 2.6中，
    * 新式类object是type的一个子类； `class A(object): pass` 
    * 传统类是type的一个实例，并且并不创建于一个类


#### python 3.x与Python 2.x中的新式类

`type()`内置函数返回任何对象的类型（类型本身也是一个对象）

在Python 3.0 中，

* step 1 : 对于一个列表，它的类型是一个内置类型，一个内置的列表类型 
```bash
>>> type([])
<class 'list'>
```
* step 2 :  列表类型的类型是type本身，顶层的type对象创建了**具体的类型**
``` bash
>>> type(type([]))
<class 'type'>
```
* step 3 : type是一种独特的内置对象，它位于类型层级的顶层，type的类型是它本身
```bash
>>> type(type)
<class 'type'>
```
**对于Python 2.x，也有同样的逻辑。**
``` bash
>>> type(type)
<type 'type'>
```

哲学来了，**实例创建自类，而类创建自类型**；
在Python 3中，类型的概念和类的概念合并了，这两者基本上是同义词，即
* 类型由派生自type的类定义
* 用户定义的类 是 类型类 的实例。
* 用户定义的类是产生它们自己的实例的类型


python 3.0中的类（以及Python 2.6中的新式类object）是type类的实例，
* 类连接到type的属性 `__class__`
* 实例连接到类的属性，`__class__`
``` bash
>>> class C: pass
...
>>> X=C()
>>> type(X)
<class '__main__.C'>
# 实例连接到创建它的类 X.__class__
>>> X.__class__
<class '__main__.C'>
>>> type(C)
<class 'type'>
# 类本身是type类的实例，通过 C.__class__ 连接到type
>>> C.__class__
<class 'type'>
```

#### python 2.x 中的传统类 

对于Python 2.x中的传统类，它们没有`__class__`的属性链接
``` bash
>>> class C:pass
...
>>> X = C()
# X 是 实例类型，instance type
>>> type(X)
<type 'instance'>
>>> type(type(X))
<type 'type'>
>>> type(C)
# C 是 class类型
<type 'classobj'>
>>> type(type(C))
<type 'type'>
# 实例通过__class__ 连接到创建它的类
>>> X.__class__
<class __main__.C at 0x7fc22fc7c530>
>>> C.__class__
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: class C has no attribute '__class__'
```

### Python 3.x与Python 2.x新式类的继续深入

> python学习手册

**python遵循一个标准的协议：**

在一条class语句的末尾，并且在运行了一个命名空间词典中的所有嵌套代码之后，会调用type对象来创建class对象
``` python
class = type(classname, superclasses, attributedict)
```
type 对象反过来定义了一个`__call__`运算符重载方法，当调用type对象时，该方法运行两个其他方法：
``` python
type.__new__(typeclass, classname, superclasses, attributedict)
type.__init__(typeclass, classname, superclasses, attributedict)
```
`__new__`方法创建并返回了新的`class`对象，而随后`__init__`方法初始化新创建的对象。

这里可以看出，普通类是type的一个实例(类型对象)

而元类是type类的子类，也是实例

#### 编写元类定制type

在Python 3.0的元类声明扩展（Python 2.x 有不同的声明方式）
``` python
# inherite of Eggs, instance of Meta
class Spam (Eggs, metaclass = Meta):  # python 3.0 or later
# class Spam (object):        # python 2.x (1)
    # __metaclass__ = Meta    # python 2.x (2)
    data = 1    # class data attribute
    def meth(self, arg):    # Class method attribute
        pass
```
这条class语句的末尾，Python内部运行如下代码来创建class对象：
``` python
Spam = Meta('Spam', (Eggs,), {'data': 1, 'meth':meth, '__module__':'__main__'})
```
如果元类`meta`定义了`__new__`和`__init__`的自己版本，在此类的调用期间，它们将一次有其继承的type类的`__call`方法调用，以创建并初始化新类对象

#### 一个什么也不干的元类

一个只是带有`__new__`方法的type子类
``` Python
class Meta(type):
    def __new__(meta, classname, supders, classdict):
        return type.__new__(meta, classname, supers, classdict)

```
#### 一个什么也不干的工厂函数（元函数？）

**实际上，任何可调用的对象都可以作为一个元类**

只要它接收传递的参数并且返回与目标类兼容的对象即可。

一个简单的对象工厂函数
``` python
def MetaFunc(classname, supers, attributedict):
    print("In MetaFunc: ", classname, supers, attributedict, sep='\n...')
    return type(classname, supers, attributedict)

class Eggs:
    pass

class Spam(Eggs, metaclass=MetaFunc):
    data = 1
    def meth(self, args):
        pass

print ("making instance")
X = Spam()
print ("data: ", X.data)
```

#### 元类->类对象的定制构建和初始化

单纯的类方法`__new__`与`__init__`可以用在创建**类实例**时管理实例的钩子，

而元类的`__new__`与`__init__`可以用做在创建**类对象**时管理类的钩子
* 类的构建方法在class语句之后运行，类的初始化方法在类构建方法之后运行
* 两者都在创建任何实例之前运行

#### 元类的属性不会被类实例继承

1. 类是元类的实例。所以，元类中定义的行为应用于类，而不是类随后的实例继承
2. 实例从它们的类和超类中获取行为，但是不从任何元类中获取行为
2. 实例的属性查找只是搜索实例及其所有类的`__dict__`字典；元类并不包含在实例查找中

``` python
class Metaone(type):
    def __new__(meta, classname, supers, attributedict):
        print("In Metaone.new: ", classname)
        # return type(classname, supers, attributedict)
        return type.__new__(meta,  classname, supers, attributedict)
    def toast(self):
        print("toast")

class Super(metaclass = Metaone):
    def spam(self):
        print ("spam")

class C(Super):
    def eggs(self):
        print("eggs")


X = C()
X.eggs()
X.spam()
X.toast()
```
输出
``` bash
In Metaone.new:  Super
In Metaone.new:  C
eggs
spam
Traceback (most recent call last):
  File "metacls.py", line 25, in <module>
    X.toast()
AttributeError: 'C' object has no attribute 'toast'
```

#### 基于元类的扩展，扩展类行为类方法

> metapcls.py
``` python
def eggsfunc(obj):
    return obj.value * 4

def hamfunc(obj, value):
    return value + obj.value

class Extender(type):
    def __new__ (meta, classname, supers, attributedict):
        attributedict['eggs'] = eggsfunc
        attributedict['ham'] = hamfunc
        return type.__new__(meta, classname, supers, attributedict)

class Client1(metaclass = Extender):
    def __init__ (self, value):
        self.value = value

    def spam(self):
        return self.value * 2

class Client2(metaclass = Extender):
    value = "test"
    # def __init__ (self, value):
        # self.value = value


X = Client1("Apple")
print(X.eggs())
print(X.spam())
print(X.ham("bacon "))
```
输出
``` bash
AppleAppleAppleApple
AppleApple
bacon Apple
```



### python C 源码

在Python的C源码中，用结构体表示Python的类；

每个表示类的结构体的第二个字段都是一个指向PyTypeObject对象的指针；

这个PyTypeObject就是类型对象（type得到的东西），

每一个类的对象共用一个PyTypeObject对象（所以用的指针）。

在python中只要有引用计数（一个long的整型），指向PyTypeObject的指针就可以成为类。

