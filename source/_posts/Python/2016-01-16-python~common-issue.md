title: python-common-issue
date: 2016-01-16 00:54:11
category: [Python]
tags: 
    - python
description: python常见问题与方法
----

### python调用cmd命令

Python调用cmd命令，比较常见的有两种方法：

第一种可以使用os.system(“cmd”)，如

    import os
    os.system("ipconfig /all")

第二种可以使用Popen模块产生新的process，如

    import subprocess
    proc = subprocess.Popen("ipconfig /all", shell = True)
    proc.wait() # 等待子进程结束

但是我们要注意的是，Popen使用时shell参数的设置。一般情况下，需要加上`shell = True`。

### 字符串前缀

-   r
    -   在Python的string前面加上‘r’， 是为了告诉编译器这个string是个raw string，不要转意backslash '\' 

### 可变参数`*args`与`**kwargs`的应用

``` python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

def f(*args, **kwargs):
    print ("list has {args_len} params, and dict has {dict_len} params".format(args_len=len(args), dict_len=len(kwargs)))

if __name__ == "__main__":
    f(1,2,3,a=4,b=5)
    # 下面这种方式 list 有 2 个参数，dict有0个参数
    # f([1,2,3],{"fa":"nohg"})

# output :
# list has 3 params, and dict has 2 params
```

### Reference Link
* [fireling的数据空间|宁哥的小站 » Python调用cmd命令](http://www.lining0806.com/python%E8%B0%83%E7%94%A8cmd%E5%91%BD%E4%BB%A4/)
