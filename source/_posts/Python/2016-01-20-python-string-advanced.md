title: python-字符串的进阶
date: 2016-01-20 09:21:05
tags: python
---

## Python - 格式化(format())输出字符串 详解 及 代码

出处：[Python - 格式化(format())输出字符串 详解 及 代码](http://blog.csdn.net/caroline_wendy/article/details/17111451)

Python中格式化输出字符串使用`format()`函数, 字符串即类, 可以使用方法;

Python是完全面向对象的语言, 任何东西都是对象;

字符串的参数使用`{NUM}`进行表示,

0, 表示第一个参数,1, 表示第二个参数, 以后顺次递加;

使用":", 指定代表元素需要的操作, 如"`:.3`"小数点三位, "`:8`"占8个字符空间等;
还可以添加特定的字母, 如:

*   'b' - 二进制. 将数字以2为基数进行输出.
*   'c' - 字符. 在打印之前将整数转换成对应的Unicode字符串.
*   'd' - 十进制整数. 将数字以10为基数进行输出.
*   'o' - 八进制. 将数字以8为基数进行输出. 
*   'x' - 十六进制. 将数字以16为基数进行输出, 9以上的位数用小写字母.
*   'e' - 幂符号. 用科学计数法打印数字, 用'e'表示幂. 
*   'g' - 一般格式. 将数值以fixed-point格式输出. 当数值特别大的时候, 用幂形式打印. 
*   'n' - 数字. 当值为整数时和'd'相同, 值为浮点数时和'g'相同. 不同的是它会根据区域设置插入数字分隔符. 
*   '%' - 百分数. 将数值乘以100然后以fixed-point('f')格式打印, 值后面会有一个百分号. 
 
数字(0, 1, ...)即代表format()里面的元素, 所以可以使用"."调用元素的方法;

``` python
    # -*- coding: utf-8 -*-  
    #====================  
    #File: abop.py  
    #Author: Wendy  
    #Date: 2013-12-03  
    #====================  
    #eclipse pydev, python3.3  
    age = 25  
    name = 'Caroline'  
      
    print('{0} is {1} years old. '.format(name, age)) #输出参数  
    print('{0} is a girl. '.format(name))  
    print('{0:.3} is a decimal. '.format(1/3)) #小数点后三位  
    print('{0:_^11} is a 11 length. '.format(name)) #使用_补齐空位  
    print('{first} is as {second}. '.format(first=name, second='Wendy')) #别名替换  
    print('My name is {0.name}'.format(open('out.txt', 'w'))) #调用方法  
    print('My name is {0:8}.'.format('Fred')) #指定宽度  
```
输出：
```
    Caroline is 25 years old.   
    Caroline is a girl.   
    0.333 is a decimal.   
    _Caroline__ is a 11 length.   
    Caroline is as Wendy.   
    My name is out.txt  
    My name is Fred    .  
```

## Reference Link
