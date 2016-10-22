title: python-basic~string
date: 2016-10-19 11:42:51
category: [Python, Basic]
tags:
  - string
description:
----
[Toc]

### 连接字符串

1. `stra + strb`
2. `stra,strb` ： 用空格连接两个字符串
3. `%` : `'name is %s, %s' % (stra, strb)`  多用于字符串的格式化
4. `','.join([stra, strb])` : 示例，使用`,`连接字符串列表
  

### readline() from a string

【参考链接】
1. [Python readline() from a string - stackflow](http://stackoverflow.com/questions/7472839/python-readline-from-a-string)

#### 方法1：`StringIO`

* Python 2
  You can use `StringIO`:
  ```bash
  >>> msg = "Bob Smith\nJane Doe\nJane,\nPlease order more widgets\nThanks,\nBob\n"
  >>> msg
  'Bob Smith\nJane Doe\nJane,\nPlease order more widgets\nThanks,\nBob\n'
  >>> import StringIO
  >>> buf = StringIO.StringIO(msg)
  >>> buf.readline()
  'Bob Smith\n'
  >>> buf.readline()
  'Jane Doe\n'
  ```
  Be sure to use cStringIO if performance is important.

* Python 3
  You can use `io.StringIO`:
  ```bash
  >>> import io
  >>> buf = io.StringIO(msg)
  >>> buf.readline()
  'Bob Smith\n'
  >>> buf.readline()
  'Jane Doe\n'
  >>> len(buf.read())
  ```


#### 方法2：string.split('\n') 

```bash
>>> foo = "ABC\nDEF\nGHI\nJKL"
>>> foo.split("\n", 1)
['ABC', 'DEF\nGHI\nJKL']
>>> foo.split("\n", 2)
['ABC', 'DEF', 'GHI\nJKL']
```
