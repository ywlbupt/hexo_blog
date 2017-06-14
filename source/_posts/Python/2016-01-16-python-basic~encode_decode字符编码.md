title: python-basic~encode_decode字符编码
date: 2016-01-16 12:26:38
tags: python
description:
----

### 基本介绍-python3的字符串编码

在 Python 2 中，字符串分为 ASCII 码表示（‘some text’）和 Unicode 表示（u‘Unicode 字符串’），默认为 ASCII 码。

不过在 Python 3 中，默认就是万能的 Unicode 码了，所以字符串前面不用加字母 u 也可以写 Unicode 了，当然这不是重点，重点是不会有各种 ASCII 和 Unicode 转换和混用带来的错误了。

另外， **Python 3 中增加了一种 bytes 对象**（b‘\xb6\xfe\xbd\xf8\xd6\xc6\xca\xfd\xbe\xdd’），专门用来表示编码后的（二进制）数据，所以现在对字符串的编码就是从 str 到 bytes 的转换，反之亦然

-   bytes(*pytho3新增对象*)
    -   在网络上传输
    -   保存到磁盘
    -   python: bytes类型以 b 前缀打头。
-   Unicode
    -   计算机内存
    -   python3 str: 以unicode保存
    -   python2 str: str类型需以 u 前缀打头表示unicode类型
-   utf-8,ascii,gbk,etc...
    -   保存到硬盘，传输
    -   python3源代码: 默认假定.py源代码以utf-8保存，`# -*- coding: utf-8 -*-`告诉python解释器，按照`utf-8`读取源代码 

#### python3 中字符串编码的转换

在Python3中，每個字串都是Unicode，不使用內部编码表現，而使用str实例作為代表。如果想将字串转为指定的编码，可以使用`encode()`方法取得一個bytes实例，如果有個bytes实例，也可以使用`decode()`方法指定编码取得str实例：

    >>> '元'.encode('big5')
    b'\xa4\xb8'
    >>> '元'.encode('utf-8')
    b'\xe5\x85\x83'
    >>> '元'.encode('big5').decode('big5')
    '元'

![](/hexo_blog/images/python/string-encoding-python3.jpg)

*python3的`print`函数只支持`unicode`的str，貌似没有对bytes的解码功能，所以对对不能解码的bytes不能正确输出。*

#### python2.x 中的字符串编码转换
**在编码转换时通常需要以unicode作为中间编码**
先将其他编码的字符串解码(decode)成unicode，再从unicode编码(encode)成另一种编码

1. decode的作用是将其他编码的字符串转换成unicode编码
2. encode的作用是将unicode编码转换成其他编码的字符串

比如从文件读入utf-8编码格式的字符串s，

    s.decode("utf-8", "ignore").encode("gbk", "ignore") # 先由utf-8转为unicode，再由unicode转为gbk，ignore表示忽略非法字符

    >>> b'ABC'.decode('ascii')
    'ABC'
    >>> b'\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')
    '中文'

#### isinstance 判断


    def TestisStrOrUnicdeOrString():
      bs = b'Hello'
      ustr = 'abc'
      print (isinstance(bs, str))  #False
      print (isinstance(bs,bytes)) #True
      print (isinstance(ustr,str)) #True
      print (isinstance(ustr, bytes)) #False
      print (isinstance(bs,(bytes,str))) #True

#### 获取系统默认编码

Python3中默认是UTF-8，我们通过以下代码：

    import sys
    sys.getdefaultencoding()
    # 'uft-8'

#### 单字节的处理

1. ord():`ord('A') == 65`
    
2. chr():`chr(65) == 'A'`

### codecs 模块简介

codecs是encoders和decoders的缩写。

codecs模块为我们解决的字符编码的处理提供了lookup方法，它接受一个字符编码名称的参数，并返回指定字符编码对应的codecs.CodecInfo 对象，该对象包含了 encoder、decoder、StreamReader和StreamWriter的函数对象和类对象的引用。为了简化对lookup方法的调用， codecs还提供了getencoder(encoding)、getdecoder(encoding)、getreader(encoding)和 getwriter(encoding)方法；进一步，简化对特定字符编码的StreamReader、StreamWriter和 StreamReaderWriter的访问，codecs更直接地提供了open方法，通过encoding参数传递字符编码名称，即可获得对 encoder和decoder的双向服务。


这个模块的强大之处在于它提供了流的方式来处理字符串编码，当处理的数据很多时，这种方式很有用。
你可以使用IncrementalEncoder和IncrementalDecoder，但是强烈建议使用StreamReader和StreamWriter，因为使用它们会大大简化你的代码。

例如，有一个test.txt的文件，它的编码为gbk，现在我需要将它的编码转换为utf8，可以编写如下代码：

``` python
    #coding:utf8
    import codecs
    # 打开文件 如果此处用codecs.open()方法打开文件，就不用创建reader和writer 
    fin = open('test.txt', 'r') 
    fout = open('utf8.txt', 'w') 
    # 获取 StreamReader 
    reader = codecs.getreader('gbk')(fin) 
    # 获取 StreamWriter 
    writer = codecs.getwriter('utf8')(fout) 
    din = reader.read(10) 
    while din: 
        writer.write(din) 
        din = reader.read(10)
```

_用codecs提供的open方法来指定打开的文件的语言编码，它会在读取的时候自动转换为内部unicode_

#### python2 读写文件

内置的open()方法打开文件时，
read()读取的是str，读取后需要使用正确的编码格式进行decode()。
write()写入时，如果参数是unicode，则需要使用你希望写入的编码进行encode()，

如果是其他编码格式的str，则需要先用该str的编码进行decode()，转成unicode后再使用写入的编码进行encode()。
如果直接将unicode作为参数传入write()方法，Python将先使用源代码文件声明的字符编码进行编码然后写入。

```python
    # coding: UTF-8
     
    f = open('test.txt')
    s = f.read()
    f.close()
    print type(s) # <type 'str'>
    # 已知是GBK编码，解码成unicode
    u = s.decode('GBK')
     
    f = open('test.txt', 'w')
    # 编码成UTF-8编码的str
    s = u.encode('UTF-8')
    f.write(s)
    f.close()
```

模块codecs提供了一个open()方法，可以指定一个编码打开文件，使用这个方法打开的文件读取返回的将是unicode。
写入时，
如果参数是unicode，则使用open()时指定的编码进行编码后写入；
如果是str，则先根据源代码文件声明的字符编码，解码成unicode后再进行前述操作。
相对内置的open()来说，这个方法比较不容易在编码上出现问题。

 
```python
    # coding: GBK
    import codecs
     
    f = codecs.open('test.txt', encoding='UTF-8')
    u = f.read()
    f.close()
    print type(u) # <type 'unicode'>
     
    # codecs 以utf-8编码打开文件，转换成unicode编码
    f = codecs.open('test.txt', 'a', encoding='UTF-8')
    # 写入unicode
    f.write(u)
     
    # 写入str，自动进行解码编码操作
    # GBK编码的str
    s = '汉'
    print repr(s) # '\xba\xba'
    # 这里会先将GBK编码的str解码为unicode再编码为UTF-8写入
    f.write(s) 
    f.close()
```

### 与编码相关的方法

sys/locale模块中提供了一些获取当前环境下的默认编码的方法。

```python
    # coding:gbk
     
    import sys
    import locale
     
    def p(f):
        print '%s.%s(): %s' % (f.__module__, f.__name__, f())
     
    # 返回当前系统所使用的默认字符编码
    p(sys.getdefaultencoding)
     
    # 返回用于转换Unicode文件名至系统文件名所使用的编码
    p(sys.getfilesystemencoding)
     
    # 获取默认的区域设置并返回元祖(语言, 编码)
    p(locale.getdefaultlocale)
     
    # 返回用户设定的文本数据编码
    # 文档提到this function only returns a guess
    p(locale.getpreferredencoding)
     
    # \xba\xba是'汉'的GBK编码
    # mbcs是不推荐使用的编码，这里仅作测试表明为什么不应该用
    print r"'\xba\xba'.decode('mbcs'):", repr('\xba\xba'.decode('mbcs'))
     
    #在笔者的Windows上的结果(区域设置为中文(简体, 中国))
    #sys.getdefaultencoding(): gbk
    #sys.getfilesystemencoding(): mbcs
    #locale.getdefaultlocale(): ('zh_CN', 'cp936')
    #locale.getpreferredencoding(): cp936
    #'\xba\xba'.decode('mbcs'): u'\u6c49'
```


### Reference Link 
1. [字符串和编码-廖雪峰](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001431664106267f12e9bef7ee14cf6a8776a479bdec9b9000)
