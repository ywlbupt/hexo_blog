title: python-basic~nonlocal与global
date: 2017-06-23 11:17:20
category:
tags:
description:
---
[Toc]

## python变量

1. 当前作用域局部变量
2. 外层作用域变量
3. 全局变量
4. python内置变量

### 关键字global

* global关键字用来在**函数**或其他**局部作用域**中使用全局变量。
* **如果不修改全局变量也可以不使用global关键字**

### 关键字nonlocal

注：python2中没有`nonlocal`关键字，如需实现类似功能，只能通过`global`实现
python 3.x中，引入了新的关键字`nonlocal`

使得可以在函数闭包中通过nonlocal，引用与修改外层函数的变量

* nonlocal用于在**函数**或其他**局部作用域**中使用外层(非全局)作用域变量

```python
def make_counter():
    count = 0
    def counter():
        nonlocal count
        count += 1
        return count
    return counter
    
def make_counter_test():
  mc = make_counter()
  print(mc())
  print(mc())
  print(mc())
```
