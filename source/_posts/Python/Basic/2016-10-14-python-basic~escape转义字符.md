title: python-basic~escape转义字符的处理
date: 2016-10-14 16:50:42
category: [Python, Basic]
tags:
  - escape
description: python 处理转义字符escape
----
[Toc]

## python中的转义字符

\\         Backslash ()  反斜杠
\'         Single-quote (')  单引号
\"         Double-quote (")  双引号
\a         ASCII bell (BEL)  响铃符
\b         ASCII backspace (BS)  退格符
\f         ASCII formfeed (FF) 进纸符
\n         ASCII linefeed (LF) 换行符
\N{name}   Character named name in the Unicode database (Unicode only) Unicode数据库中的字符名；name就是它的名字
\r         ASCII  Carriage Return (CR)  回车符
\t         ASCII  Horizontal Tab (TAB)  水平制表符
\uxxxx     Character with 16-bit hex value xxxx (Unicode only) 值为16位十六进制xxxx的字符
\Uxxxxxxxx Character with 32-bit hex value xxxxxxxx (Unicode only) 值为32位十六进制xxxx的字符
\v         ASCII vertical tab (VT) 垂直制表符
\ooo       Character with octal value ooo  值为八进制ooo的字符
\xhh       Character with hex value hh 值为十六进制数hh的字符")')

看了文档后才意识到，Python 正则表达式模块的转义是独立的。例如匹配一个反斜杠字符需要将参数写成：'\\\\'：
Python 将字符串转义：\\\\ 被转义为 \\
re 模块获得传入的 \\ 将其解释为正则表达式，按照正则表达式的转义规则将其转义为 \
如此麻烦的前提下，Raw String 就大有作为了，顾名思义就是（除了结尾的反斜杠）不会被转义的字符串。于是匹配一个反斜杠字符就可以写作 r'\\'。

### 反转义

```python
s = "e:\test test.txt"

print(s.encode("unicode_escape"))
# Python 3 , s 为unicode,string类型
# output : b'e:\\test test.txt'
>>> repr(s)
# output : "'e:\\test test.txt'"

print(s.encode("string_escape"))
# Python 2 , s 为utf-8 类型
```

### escape() 与 unescape()

有一种简单粗暴可行的方式
``` bash
>>> a = "ab\\ncd"
>>> eval('"'+a+'"')
'ab\ncd'
>>> print(eval('"'+a+'"'))
ab
cd
```

其他可参考python code文件 `lib_y.py`

### str()与repr()的区别

* repr()
    Return a string containing a printable representation of an object. 
    This is the same value yielded by conversions (reverse quotes)

举例说明：
``` bash
>>> s = "ab\\ncd"
>>> print(s)
# output: ab\ncd
>>> str(a)
# output: 'ab\\ncd'
>>> repr(a)
# output "'ab\\\\ncd'"
# ps: here print(repr(a)) == str(a)
# ps: here eval(repr(a)) == str(a)
```
str()函数 ，它会把值转换为合理形式的字符串，以例用户可以理解；

repr()函数，它会创建一个字符串，它以合法的python表达式的形式来表示值。

请看下面例子
``` python
>>> print str("hello,world!")
hello,world!
>>> print str(10000L)

>>> print repr("hello,world!")
'hello,world!'
>>> print repr(10000L)
10000L
```

### 参考文档

1. [python中的字符串处理](http://www.cnblogs.com/dreamer-fish/p/3818443.html)
