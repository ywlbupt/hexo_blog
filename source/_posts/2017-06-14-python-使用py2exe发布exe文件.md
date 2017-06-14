title: python~使用py2exe发布exe文件
date: 2017-06-14 16:10:28
category:
tags:
description:
---
[Toc]

## py2exe

参考：[offical Tutorial](http://www.py2exe.org/index.cgi/Tutorial)

### 安装

通过pip直接安装
```
pip install py2exe
```

### 生成exe

1. 进入python程序所在文件夹，在该目录下新建`setup.py`文件，内容如下，假设待转化的python程序为example.py
``` python
from distutils.core import setup
import py2exe

setup(windows=["example.py"])
```

2. 通过命令行，执行`python setup.py py2exe`，等候时间有点长。。。
3. 执行成功后，会在该文件夹下生成dist文件夹，里面的exe文件即为所求。
