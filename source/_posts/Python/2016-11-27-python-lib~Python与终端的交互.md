title: python-lib~Python与终端的交互
date: 2016-11-27 11:35:21
category: [Python, Lib]
tags:
    - cmd
description:
---
[Toc]

command line 界面下简单来说三种方法：

1. 用 `input` 去問使用者
2. 用 `sys.argv`
3. 用一些 package/module, 像是 [argparse](https://docs.python.org/3.5/library/argparse.html) 
   或 [clime](http://clime.mosky.tw/)
4. `curse`


### 使用`sys.argv`

假设你的 python file 叫 `test.py`
``` python
import sys
print(sys.argv)
```

运行:

```
$ python test.py hello world 2016-07-01 12:30
['test.py', 'hello', 'world', '2016-07-01', '12:30']
```
### `argparse` 

1. [HOWTo - argparse](https://docs.python.org/3/howto/argparse.html)
2. [python stl - argparse](https://docs.python.org/3.5/library/argparse.html)
