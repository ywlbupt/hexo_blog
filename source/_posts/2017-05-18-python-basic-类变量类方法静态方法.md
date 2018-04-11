title: python-basic~类变量类方法静态方法
date: 2017-05-18 23:22:46
category: [Python, Basic]
tags:
description:
---
[Toc]

## 实例方法，类方法，静态方法，类变量

``` python
class TestClassMethod(object): METHOD = 'method hoho' 
    def __init__(self):
        self.name = 'leon'

    def test1(self):
        print('test1')
        print(self)

    @classmethod
    def test2(cls):
        print ('test2')
        print (cls)
        print (TestClassMethod.METHOD)
        print ('----------------')

    @staticmethod
    def test3():
        print ('test3')
        print (TestClassMethod.METHOD)

if __name__ == '__main__':
    a = TestClassMethod()
    a.test1()
    a.test2()
    a.test3()
    TestClassMethod.test3()
```
输出
``` bash
test1
<__main__.TestClassMethod object at 0x7f9c65b2c6a0>
test2
<class '__main__.TestClassMethod'>
method hoho
----------------
test3
method hoho
test3
method hoho
```
* test1为实例方法
* test2为类方法，第一个参数为类本身
* test3为静态方法，可以不接收参数
* 类方法和静态方法皆可以访问类的静态变量(类变量)，但不能访问实例变量，test2、test3是不能访问self.name的,而test1则可以

## 类变量

1. 类变量的定义在类的定义之后
2. 在类代码之外，实例能够访问类变量，也可以通过类名访问

``` python
class A():
    val = 0
    def __init__(self):
        pass

if __name__ == "__main__":
    a = A()

    print(a.val)
    # 0
    print(A.val)
```
3. **在类的代码内，可以通过`self.__class__.val`的方式访问**
