title: python-basic~const常量的处理
date: 2016-10-29 08:03:43
category: [Python, Basic]
tags:
  - const
description:
---
[Toc]

1. 《编写高质量代码：改善Python程序》

### 命名风格-约定成俗

通过命名风格来提醒使用者该变量代表的是常量

* 常量所有的字母大写
* 用下划线连接各个单词。

### 常量的属性常量的属性

* 风格要有-所有字母均大写；下划线连接各个单词
* 常量值一旦绑定便不能再修改

**可以通过自定义类来实现常量的属性**

```python
# const.py
class _const:
    class ConstError(TypeError): pass
    class ConstCaseError(ConstError): pass

    def __setattr__(self, name, value):
        if self.__dict__.has_key(name):
            raise self.ConstError, "Can't change const.%s" % name
        if not name.isupper():
            raise self.ConstCaseError, \
              'const name "%s" is not all uppercase' % name
        self.__dict__[name] = value

import sys
# 如此操作与私有类相似
sys.modules[__name__]=_const()
```

如果上面的代码对应的模块名为`const`，使用的时候只需要`import const`，便可以直接定义常量了，如以下代码：
```python
import const
const.COMPANY = "IBM"
``！`
上面的代码中常量一旦赋值便不可再更改，因此`const.COMPANY = "SAP"`会抛出`const.ConstError`:异常，
而常量名称如果小写，如`const.name = "Python"`，也会抛出`const.ConstCaseError`异常。
