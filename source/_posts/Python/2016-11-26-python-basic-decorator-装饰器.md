title: python-basic~decorator-装饰器
date: 2016-11-26 12:49:40
category: [Python, Basic]
tags:
description:
-------
[Toc]

装饰器本质上是一个Python函数，

**它可以让其他函数在不需要做任何代码变动的前提下增加额外功能，**

装饰器的返回值也是一个函数对象。

它经常用于有切面需求的场景，比如：插入日志、性能测试、事务处理、缓存、权限校验等场景。

装饰器是解决这类问题的绝佳设计，有了装饰器，我们就可以抽离出大量与函数功能本身无关的雷同代码并继续重用。

**概括的讲，装饰器的作用就是为已经存在的对象添加额外的功能。**

## 类装饰器

1. [Python编写类装饰器](http://blog.csdn.net/gavin_john/article/details/50923988)

### 类装饰器实现类实例的管理

由于类装饰器可以**拦截实例创建调用，所以它们可以用来管理一个类的所有实例**，或者扩展这些实例的接口。
下面的类装饰器实现了传统的单体编码模式，即最多只有一个类的一个实例存在。

``` python
instances = {} # 全局变量，管理实例
def getInstance(aClass, *args):
    if aClass not in instances:
        instances[aClass] = aClass(*args)
    return instances[aClass]     #每一个类只能存在一个实例

def singleton(aClass):
    def onCall(*args):
        return getInstance(aClass,*args)
    return onCall
```

为了使用它，装饰用来强化单体模型的类：
``` python
@singleton        # Person = singleton(Person)
class Person:
    def __init__(self,name,hours,rate):
        self.name = name
        self.hours = hours
        self.rate = rate
    def pay(self):
        return self.hours * self.rate
 
@singleton        # Spam = singleton(Spam)
class Spam:
    def __init__(self,val):
        self.attr = val

bob = Person('Bob',40,10)
print(bob.name,bob.pay())

sue = Person('Sue',50,20)
print(sue.name,sue.pay())

X = Spam(42)
Y = Spam(99)
print(X.attr,Y.attr)
```

现在，当`Person`或`Spam`类稍后用来创建一个实例的时候，
装饰器提供的包装逻辑层把实例构建调用指向了onCall，
它反过来调用getInstance，以针对每个类管理并分享一个单个实例，而不管进行了多少次构建调用。

程序输出如下：
``` bash
Bob 400
Bob 400
42 42
```

在这里，我们使用全局的字典`instances`来保存实例，还有一个更好的解决方案就是

使用Python3中的`nonlocal`关键字，它可以为每个类提供一个封闭的作用域，如下：

``` python
def singleton(aClass):
    instance = None
    def onCall(*args):
        nonlocal instance
        if instance == None:
            instance = aClass(*args)
        return instance
    return onCall
```

当然，我们也可以用类来编写这个装饰器——如下代码对每个类使用一个实例，而不是使用一个封闭作用域或全局表：
``` python
class singleton:
    def __init__(self,aClass):
        self.aClass = aClass
        self.instance = None
    def __call__(self,*args):
        if self.instance == None:
            self.instance = self.aClass(*args)
        return self.instance
```

## From Zhihu

1. [如何理解python装饰器-zhijun liu](https://www.zhihu.com/question/26930016/answer/99243411)

装饰器本质上是一个Python函数，

**它可以让其他函数在不需要做任何代码变动的前提下增加额外功能，**

装饰器的返回值也是一个函数对象。

它经常用于有切面需求的场景，比如：插入日志、性能测试、事务处理、缓存、权限校验等场景。

装饰器是解决这类问题的绝佳设计，有了装饰器，我们就可以抽离出大量与函数功能本身无关的雷同代码并继续重用。

**概括的讲，装饰器的作用就是为已经存在的对象添加额外的功能。**

先来看一个简单例子：
``` python
def foo():
	print('i am foo')
```
现在有一个新的需求，希望可以记录下函数的执行日志，于是在代码中添加日志代码：
``` python
def foo():
	print('i am foo')
	logging.info("foo is running")
```
bar()、bar2()也有类似的需求，怎么做？

再写一个logging在bar函数里？

这样就造成大量雷同的代码，为了减少重复写代码，

我们可以这样做，重新定义一个函数：专门处理日志，日志处理完之后再执行真正的业务代码
``` python
def use_logging(func):
	logging.warn("%s is running" % func.__name__)
	func()

def bar():
	print('i am bar')

use_logging(bar)
```

逻辑上不难理解，但是这样的话，我们每次都要将一个函数作为参数传递给use_logging函数。

而且这种方式已经破坏了原有的代码逻辑结构，

之前执行业务逻辑时，执行运行bar()，但是现在不得不改成`use_logging(bar)`。

那么有没有更好的方式的呢？当然有，答案就是装饰器。

### 简单装饰器 - 无参数装饰器

``` python
def use_logging(func):

	def wrapper(*args, **kwargs):
		logging.warn("%s is running" % func.__name__)
		return func(*args, **kwargs)
	return wrapper

def bar():
	print('i am bar')

bar = use_logging(bar)
bar()
```
函数use_logging就是装饰器，它把执行真正业务方法的func包裹在函数里面，看起来像bar被use_logging装饰了。

在这个例子中，函数进入和退出时，被称为一个横切面(Aspect)，这种编程方式被称为面向切面的编程(`Aspect-Oriented Programming`)。

@符号是装饰器的语法糖，在定义函数的时候使用，避免再一次赋值操作

``` python
def use_logging(func):

	def wrapper(*args, **kwargs):
		logging.warn("%s is running" % func.__name__)
		return func(*args)
	return wrapper

@use_logging
def foo():
	print("i am foo")

@use_logging
def bar():
	print("i am bar")

bar()
```

如上所示，这样我们就可以省去`bar = use_logging(bar)`这一句了，直接调用bar()即可得到想要的结果。

如果我们有其他的类似函数，我们可以继续调用装饰器来修饰函数，而不用重复修改函数或者增加新的封装。这样，我们就提高了程序的可重复利用性，并增加了程序的可读性。

装饰器在Python使用如此方便都要归因于Python的函数能像普通的对象一样能作为参数传递给其他函数，可以被赋值给其他变量，可以作为返回值，可以被定义在另外一个函数内。

### 带参数的装饰器 - 三层定义

装饰器还有更大的灵活性，例如带参数的装饰器：

在上面的装饰器调用中，比如`@use_logging`，该装饰器唯一的参数就是执行业务的函数。

装饰器的语法允许我们在调用时，提供其它参数，比如`@decorator(a)`。

这样，就为装饰器的编写和使用提供了更大的灵活性。
``` python
def use_logging(level):
	def decorator(func):
		def wrapper(*args, **kwargs):
			if level == "warn":
				logging.warn("%s is running" % func.__name__)
			return func(*args)
		return wrapper

	return decorator

@use_logging(level="warn")
def foo(name='foo'):
	print("i am %s" % name)

foo()
```
上面的`use_logging`是允许带参数的装饰器。它实际上是对原有装饰器的一个函数封装，并返回一个装饰器。

我们可以将它理解为一个含有参数的**闭包**。

当我们使用`@use_logging(level="warn")`调用的时候，Python能够发现这一层的封装，并把参数传递到装饰器的环境中。

### 类装饰器

再来看看类装饰器，相比函数装饰器，类装饰器具有灵活度大、高内聚、封装性等优点。使用类装饰器还可以依靠类内部的`__call__`方法，当使用 @ 形式将装饰器附加到函数上时，就会调用此方法。

``` python
class Foo(object):
	def __init__(self, func):
		self._func = func

	def __call__(self):
		print ('class decorator runing')
		self._func()
		print ('class decorator ending')

@Foo
def bar():
	print ('bar')

bar()
```

#### 装饰类方法 method decorator

``` python
def method_friendly_decorator(method_to_decorate):
    def wrapper(self, lie):
        lie = lie - 3 # very friendly, decrease age even more :-)
        return method_to_decorate(self, lie)
    return wrapper


class Lucy(object):

    def __init__(self):
        self.age = 32

    @method_friendly_decorator
    def sayYourAge(self, lie):
        print("I am {0}, what did you think?".format(self.age + lie))

l = Lucy()
l.sayYourAge(-3)
#outputs: I am 26, what did you think?
```
If you’re making general-purpose decorator--one 
you’ll apply to any function or method, no matter its arguments--

then just use `*args, **kwargs`:

``` python
def a_decorator_passing_arbitrary_arguments(function_to_decorate):
    # The wrapper accepts any arguments
    def a_wrapper_accepting_arbitrary_arguments(*args, **kwargs):
        print("Do I have args?:")
        print(args)
        print(kwargs)
        # Then you unpack the arguments, here *args, **kwargs
        # If you are not familiar with unpacking, check:
        # http://www.saltycrane.com/blog/2008/01/how-to-use-args-and-kwargs-in-python/
        function_to_decorate(*args, **kwargs)
    return a_wrapper_accepting_arbitrary_arguments

@a_decorator_passing_arbitrary_arguments
def function_with_no_argument():
    print("Python is cool, no argument here.")

function_with_no_argument()
#outputs
#Do I have args?:
#()
#{}
#Python is cool, no argument here.

@a_decorator_passing_arbitrary_arguments
def function_with_arguments(a, b, c):
    print(a, b, c)

function_with_arguments(1,2,3)
#outputs
#Do I have args?:
#(1, 2, 3)
#{}
#1 2 3 

@a_decorator_passing_arbitrary_arguments
def function_with_named_arguments(a, b, c, platypus="Why not ?"):
    print("Do {0}, {1} and {2} like platypus? {3}".format(a, b, c, platypus))

function_with_named_arguments("Bill", "Linus", "Steve", platypus="Indeed!")
#outputs
#Do I have args ? :
#('Bill', 'Linus', 'Steve')
#{'platypus': 'Indeed!'}
#Do Bill, Linus and Steve like platypus? Indeed!

class Mary(object):

    def __init__(self):
        self.age = 31

    @a_decorator_passing_arbitrary_arguments
    def sayYourAge(self, lie=-3): # You can now add a default value
        print("I am {0}, what did you think?".format(self.age + lie))

m = Mary()
m.sayYourAge()
#outputs
# Do I have args?:
#(<__main__.Mary object at 0xb7d303ac>,)
#{}
#I am 28, what did you think?
```
#### functools.wraps

使用装饰器极大地复用了代码，但是他有一个缺点就是原函数的元信息不见了，比如函数的`docstring、__name__、参数列表`，先看例子：

装饰器
``` python
def logged(func):
	def with_logging(*args, **kwargs):
		print func.__name__ + " was called"
		return func(*args, **kwargs)
	return with_logging
```
函数
``` python
@logged
def f(x):
   """does some math"""
   return x + x * x
```
该函数完成等价于：

``` Python
def f(x):
	"""does some math"""
	return x + x * x
f = logged(f)
```
不难发现，函数`f`被`with_logging`取代了，

当然它的docstring，__name__就是变成了with_logging函数的信息了。

``` python
print f.__name__	# prints 'with_logging'
print f.__doc__	 # prints None
```

这个问题就比较严重的，好在我们有`functools.wraps`，

wraps本身也是一个装饰器，它能把原函数的元信息拷贝到装饰器函数中，这使得装饰器函数也有和原函数一样的元信息了。

``` python
from functools import wraps
def logged(func):
	@wraps(func)
	def with_logging(*args, **kwargs):
		print func.__name__ + " was called"
		return func(*args, **kwargs)
	return with_logging

@logged
def f(x):
	"""does some math"""
	return x + x * x

print f.__name__  # prints 'f'
print f.__doc__   # prints 'does some math'
```

### 内置装饰器 @staticmathod、@classmethod、@property

### 装饰器的顺序
``` Python
@a
@b
@c
def f ():
```
等效于
``` python
f = a(b(c(f)))
```

## how can the decorator be useful

使用decorator打log，timer是个不错的选择
``` python
def benchmark(func):
    """
    A decorator that prints the time a function takes
    to execute.
    """
    import time
    def wrapper(*args, **kwargs):
        t = time.clock()
        res = func(*args, **kwargs)
        print("{0} {1}".format(func.__name__, time.clock()-t))
        return res
    return wrapper


def logging(func):
    """
    A decorator that logs the activity of the script.
    (it actually just prints it, but it could be logging!)
    """
    def wrapper(*args, **kwargs):
        res = func(*args, **kwargs)
        print("{0} {1} {2}".format(func.__name__, args, kwargs))
        return res
    return wrapper


def counter(func):
    """
    A decorator that counts and prints the number of times a function has been executed
    """
    def wrapper(*args, **kwargs):
        wrapper.count = wrapper.count + 1
        res = func(*args, **kwargs)
        print("{0} has been used: {1}x".format(func.__name__, wrapper.count))
        return res
    wrapper.count = 0
    return wrapper

@counter
@benchmark
@logging
def reverse_string(string):
    return str(reversed(string))

print(reverse_string("Able was I ere I saw Elba"))
print(reverse_string("A man, a plan, a canoe, pasta, heros, rajahs, a coloratura, maps, snipe, percale, macaroni, a gag, a banana bag, a tan, a tag, a banana bag again (or a camel), a crepe, pins, Spam, a rut, a Rolo, cash, a jar, sore hats, a peon, a canal: Panama!"))

#outputs:
#reverse_string ('Able was I ere I saw Elba',) {}
#wrapper 0.0
#wrapper has been used: 1x 
#ablE was I ere I saw elbA
#reverse_string ('A man, a plan, a canoe, pasta, heros, rajahs, a coloratura, maps, snipe, percale, macaroni, a gag, a banana bag, a tan, a tag, a banana bag again (or a camel), a crepe, pins, Spam, a rut, a Rolo, cash, a jar, sore hats, a peon, a canal: Panama!',) {}
#wrapper 0.0
#wrapper has been used: 2x
#!amanaP :lanac a ,noep a ,stah eros ,raj a ,hsac ,oloR a ,tur a ,mapS ,snip ,eperc a ,)lemac a ro( niaga gab ananab a ,gat a ,nat a ,gab ananab a ,gag a ,inoracam ,elacrep ,epins ,spam ,arutaroloc a ,shajar ,soreh ,atsap ,eonac a ,nalp a ,nam A
```
