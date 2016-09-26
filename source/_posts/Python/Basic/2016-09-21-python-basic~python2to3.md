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

#### File I/O

具体细节可参考笔记内链 TODO : 

考虑到**共存**问题，可以使用`codecs`库来兼容

##### codecs.open()

* 对于python 2 
  * 使用`open()`打开文件会有一些问题，open打开文件只能写入`str`类型，不管字符串是什么编码方式的。
    ```python
    fr = open("test.txt", 'a')
    line1="我爱祖国"
    fr.write(line1)
    ```
    这样是完全可以的，因为 line1 在python2中是字符串类型。
  * 但有时我们会遇到写入文件时编码不统一的情况(utf-8, gbk)，一般会转换成unicode，此时写入文件就有问题了：  
    ```python
    line2=u"我爱祖国"
    fr.write()
    ```
    会报错，抛出`UnicodeEncodeError`的错误
  * 对于上面的错误，可以采用先将`line2`编码成str类型，过程如下：  
    `input(utf-8) -> decode -> unicode -> encode -> output(gbk)`
  * 代替这个繁琐的操作的方法是采用`codecs.open()`，
    ```python
    import  codecs 
    fw = codecs.open('test.txt', 'a', encoding = 'utf-8')
    fw.write(line2)  # line2为unicode类型
    ```
    > 这种方法可以指定一个编码打开文件，使用这个方法打开的文件读取返回的将是unicode。写入时，如果参数 是unicode，则使用open()时指定的编码进行编码后写入；
    > 如果是str，则先根据源代码文件声明的字符编码，解码成unicode后再进行前述操作。
    > 相对内置的open()来说，这个方法比较不容易在编码上出现问题。

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
