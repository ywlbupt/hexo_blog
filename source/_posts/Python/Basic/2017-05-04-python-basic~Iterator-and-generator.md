title: python-basic~Iterator_and_generator迭代器与生成器
date: 2017-05-04 08:56:11
category: [Iterator, Generator]
tags:
description:
---
[Toc]


Python中关于迭代有两个概念，第一个是Iterable，第二个是Iterator，

* 凡是可作用于for循环的对象都是Iterable类型；
* 凡是可作用于next()函数的对象都是Iterator类型，它们表示一个惰性计算的序列；
* 集合数据类型如list、dict、str等是Iterable但不是Iterator，不过可以通过iter()函数获得一个Iterator对象。
* Python的for循环本质上就是通过不断调用next()函数实现的

协议规定Iterable的__iter__方法会返回一个Iterator, Iterator的__next__方法（Python 2里是next）会返回下一个迭代对象，如果迭代结束则抛出StopIteration异常。

所以有下面例子，
```python
class Fib(object):
    def __init__(self):
        self.a = 0
        self.b = 1

    def __next__(self):
        self.a , self.b = self.b , self.a + self.b
        return self.a

    def __iter__(self):
        return self
```
1. `__iter()__`函数只有一句话，就是`return self`，`__iter()__`首先返回一个迭代器Iterator。
2. 通过调用`__iter()__`返回的迭代器的`__next()__`方法，返回下一个迭代对象

许多对象比如list、dict，是可以重复遍历的，甚至可以同时并发地进行遍历，通过__iter__每次返回一个独立的迭代器，就可以保证不同的迭代过程不会互相影响。而生成器表达式之类的结果往往是一次性的，不可以重复遍历，所以直接返回一个Iterator就好。让Iterator也实现Iterable的兼容就可以很灵活地选择返回哪一种。

重复遍历
