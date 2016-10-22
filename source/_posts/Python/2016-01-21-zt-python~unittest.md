title: zt-python-unittest
date: 2016-01-21 23:55:37
tags: 
    - zt
    - python
description: 
----

转载：[Python的单元测试（一）](http://kingname.info/2015/03/04/pythonunittest1/)
转载：[Python的单元测试（二）](http://kingname.info/2015/03/04/pythonunittest2/)

### (一)

测试驱动的软件开发方式可以强迫程序员在开发程序的时候使程序的函数之间实现高内聚，低耦合。这样的方式可以降低函数之间的依赖性，方便后续的修改，增加功能和维护。

说一个函数高内聚，就是指这个函数专注于实现单一的任务，不会做除了生产这个任务以外的其他事情。可以想象一个人，他把自己关在一个小房子里面生产东西，只留两扇窗户，他需要什么材料，你就从小窗户给他送进去（参数），他做好了东西，就给你从另一个窗户里面送出来（return），他不会说，我要生产一个轮子，但是我首先需要一个女人进来，他不会说，这是计划的一部分。

说几个函数是低耦合的，就是指他们的依赖性小。他们就像是葫芦娃，每个都有自己独特的能力，可以自己单干，在关键的时候还可以合体，变成小金刚。他们就像积木一样，各有各的功能，需要使用的时候直接组合在一起就可以了。

使用测试驱动开发，每一个测试只测试一个功能，这样就可以迫使函数把自己独立出来，尽量减少和其他函数的依赖。

例如，有一个文件1.txt，他的内容是两个数字，使用逗号隔开。形如“2,4”（不包括外侧双引号，下同）。我要写一个程序readandadd.py，读取硬盘上的1.txt文件，然后把这个文件的内容打印到屏幕上。

不规范的写法一：

    f= open('1.txt','r')
    b = f.read().split(',')
    f.close()
    print int(b[0])+int(b[1])
不规范写法二：

    def A():
        f= open('1.txt','r')
        b = f.read().split(',')
        f.close()
        print int(b[0])+int(b[1])
    A()
比较规范的写法：

    def read(filename):
        f= open(filename,'r')
        info = f.read()
        f.close()
        return info

    def getnum(info):
        twonum = info.split(',')
        return twonum

    def addnum(twonum):
        return int(twonum[0])+int(twonum[1])

    if __name__ == '__main__':
            info = read('1.txt')
            twonum = getnum(info)
            result = addnum(twonum)
            print result
这样写的好处是，如果想测试读文件的功能，就只需要测试read()函数，如果想测试把两个数分开的功能，就只需要测试getnum()函数。而相反，在不规范写法二中，虽然只想测试两个数字相加的功能，可是却不得不首先打开文件并读取文件然后把数字分开。

继续回到比较规范的写法当中，我相信很多人写完read()函数以后，肯定会输入如下代码：

    def read(filename):
        f= open(filename,'r')
        info = f.read()
        f.close()
        return info

    print read('1.txt')
然后运行程序，发现正常打印出'2,3'以后，再开始写getnum()函数。写完getnum以后，测试getnum()函数没问题以后再开始写然后测试addnum()函数。最后测试整个程序的功能。

其实这个过程，已经就是在做单元测试了。然而这样操作的弊端是什么？如果整体程序已经写好了，之前做测试点代码也就删除了。那么如果突然把程序做了修改。例如1.txt里面数字的分隔从1个逗号变成了空格，或者变成了3个数字，那必然要修改getnum()，但是又如何测试修改的部分呢？还要把不相干的代码给注释掉。不仅麻烦，而且容易出错。

现在，把测试的代码单独独立出来。会有什么效果呢？尝试创建一个test.py程序，代码如下：

    import readandadd

    def testread():
        print 'read:',readandadd.read('1.txt')

    def testgetnum():
        print 'getnum:',readandadd.getnum('2,3')

    def testaddnum():
        print 'addnum:',readandadd.addnum([2,3])

    if __name__ == '__main__':
        testread()
        testgetnum()
        testaddnum()
运行test.py以后输出结果如下：

    read: 2,3
    getnum: ['2', '3']
    addnum: 5
每一个函数的输出结果一目了然，而且在修改了readandadd.py的函数以后，重新运行test.py就可以知道输出结果有没有符合预期。

当然，这里这个例子非常的简单，因此可以人工通过观察test.py的输出结果来确定是否符合预期，那对于大量的函数的测试，难道也要让肉眼来看吗？当然不是。于是，下一篇文章将会介绍Python的单元测试`unittest`。

### (二)

在这个例子中，只有三个函数，于是可以把每个函数的输出结果打印到屏幕上，再用肉眼去看结果是否符合预期。然而假设有一个程序，有二十个类，每个类又有几十个函数，有些函数的输出结果还多达几十行，在这种情况下，肉眼如何看得出？

当然你可以使用if判断

    if 输出结果 == 预期结果:
        return True
    else:
        print u'不相等'
这个时候，你发现，程序有几个函数，后三行就要重复几次，本着代码简洁的原则，你把这个判断的过程写到一个函数中：

    def isequal(output,right_output):
        if output == right_output:
            return True
        else:
            print u'不相等'
那么恭喜你，你步入正规了，然而，这一切已经有人为你做好了。欢迎unittest模块出场。

unittest supports test automation, sharing of setup and shutdown code for tests, aggregation of tests into collections, and independence of the tests from the reporting framework. The unittest module provides classes that make it easy to support these qualities for a set of tests.

Python的官方文档这样写到，unittest支持自动化测试，测试的安装分享和关闭代码……

一句话说来，就是，unittest很好用。

还是用上一次的readandadd.py来演示unittest的基本用法,首先介绍unittest的一个函数，assertEqual(first,second),这个函数的作用是检查变量first的值与second的值是否相等，如果不相等就抛出错误。

先创建utest.py文件，输入以下代码并运行：

    #-*-coding:utf-8-*-
    import unittest
    import readandadd

    class basictest(unittest.TestCase): #类名可以随便取
        def testread(self): #每个函数都要以test开头
            output = readandadd.read('1.txt')
            self.assertEqual(output,'2,3')

        def testgetnum(self):
            output = readandadd.getnum('2,3')
            self.assertEqual(output,['2', '3'])

        def testaddnum(self):
            output = readandadd.addnum([2,3])
            self.assertEqual(output,5)

    if __name__ == '__main__':
        unittest.main()
运行结果如下：

    ...
    ----------------------------------------------------------------------
    Ran 3 tests in 0.001s

    OK
你也许会说，就一个ok，什么都没有啊。那我先把testread()函数下面的

    self.assertEqual(output,'2,3')
改为

    self.assertEqual(output,'2,4')
在运行utest.py看看输出结果如何：

    ..F
    ======================================================================
    FAIL: testread (__main__.basictest)
    ----------------------------------------------------------------------
    Traceback (most recent call last):
      File "E:/mystuff/unitest/utest.py", line 8, in testread
        self.assertEqual(output,'2,4')
    AssertionError: '2,3' != '2,4'

    ----------------------------------------------------------------------
    Ran 3 tests in 0.000s

    FAILED (failures=1)
这里准确的找出了错误的位置和错误的具体内容。注意看最上面，有个

    ..F
猜测它可能是标示错误的位置。保持testread的错误不改，再把testgetnum()函数中的以下内容

    self.assertEqual(output,['2', '3'])
改为

    self.assertEqual(output,['2', '6'])
再运行utest.py程序，输出结果如下：

    .FF
    ======================================================================
    FAIL: testgetnum (__main__.basictest)
    ----------------------------------------------------------------------
    Traceback (most recent call last):
      File "E:/mystuff/unitest/utest.py", line 12, in testgetnum
        self.assertEqual(output,['2', '6'])
    AssertionError: Lists differ: ['2', '3'] != ['2', '6']

    First differing element 1:
    3
    6

    - ['2', '3']
    ?        ^

    + ['2', '6']
    ?        ^


    ======================================================================
    FAIL: testread (__main__.basictest)
    ----------------------------------------------------------------------
    Traceback (most recent call last):
      File "E:/mystuff/unitest/utest.py", line 8, in testread
        self.assertEqual(output,'2,4')
    AssertionError: '2,3' != '2,4'

    ----------------------------------------------------------------------
    Ran 3 tests in 0.001s

    FAILED (failures=2)
可以看出，这里分别把两个错误显示了出来。并且第一行变成了

.FF
所以，第一行的内容应该从右往左读，它标明错误函数在所有函数的相对位置。

现在再把testread()和testgetnum()改回去，再看看全部正确的输出：

    ...
    ----------------------------------------------------------------------
    Ran 3 tests in 0.000s

    OK
印证了那句话，没有消息就是最好的消息。

这篇文章介绍了单元测试模块unittest的assertEqual的基本用法
