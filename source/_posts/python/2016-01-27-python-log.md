title: python-loging调试监控
date: 2016-01-21 23:55:37
category: python
tags: 
    - python
description: 
---
## 概述

允许你指定记录信息的级别，有
- debug
- info
- warning
- error

日志级别大小关系为：CRITICAL > ERROR > WARNING > INFO > DEBUG > NOTSET，当然也可以自己定义日志级别

等几个级别，当我们指定`level=INFO`时，`logging.debug`就不起作用了。同理，指定`level=WARNING`后，debug和info就不起作用了。这样一来，你可以放心地输出不同级别的信息，也不用删除，最后统一控制输出哪个级别的信息。

### logging 参数
logging.basicConfig函数各参数:
* filename: 指定日志文件名
* filemode: 和file函数意义相同，指定日志文件的打开模式，'w'或'a'
* format: 指定输出的格式和内容，format可以输出很多有用信息，如上例所示:
     -   %(levelno)s: 打印日志级别的数值
     -   %(levelname)s: 打印日志级别名称
     -   %(pathname)s: 打印当前执行程序的路径，其实就是sys.argv[0]
     -   %(filename)s: 打印当前执行程序名
     -   %(funcName)s: 打印日志的当前函数
     -   %(lineno)d: 打印日志的当前行号
     -   %(asctime)s: 打印日志的时间
     -   %(thread)d: 打印线程ID
     -   %(threadName)s: 打印线程名称
     -   %(process)d: 打印进程ID
     -   %(message)s: 打印日志信息
* datefmt: 指定时间格式，同time.strftime()
* level: 设置日志级别，默认为logging.WARNING
* stream: 指定将日志的输出流，可以指定输出到sys.stderr,sys.stdout或者文件，默认输出到sys.stderr，当stream和filename同时指定时，stream被忽略

logging的另一个好处是通过简单的配置，一条语句可以同时输出到不同的地方，比如console和文件。

``` python
    #-*- coding:utf-8 -*-
    import logging
    
    # 配置日志信息
    logging.basicConfig(level=logging.DEBUG,
    format='%(asctime)s %(name)-12s %(levelname)-8s %(message)s',
    datefmt='%m-%d %H:%M',
    filename='myapp.log',
    filemode='w')
    # 定义一个Handler打印INFO及以上级别的日志到sys.stderr
    console = logging.StreamHandler()
    console.setLevel(logging.INFO)
    # 设置日志打印格式，%(name)-12s 表示12个字符空间，左对齐；%(lineno)3d 表示3个字符空间，右对齐
    formatter = logging.Formatter('%(name)-12s[line:%(lineno)3d]:%(levelname)-8s %(message)s')
    console.setFormatter(formatter)
    # 将定义好的console日志handler添加到root logger
    logging.getLogger('').addHandler(console)
    
    logging.info('Jackdaws love my big sphinx of quartz.')
    
    logger1 = logging.getLogger('myapp.area1')
    logger2 = logging.getLogger('myapp.area2')
    
    logger1.debug('Quick zephyrs blow, vexing daft Jim.')
    logger1.info('How quickly daft jumping zebras vex.')
    logger2.warning('Jail zesty vixen who grabbed pay from quack.')
    logger2.error('The five boxing wizards jump quickly.')
```
以上程序运行结果，
```
    # 输出到控制台：
    root        : INFO     Jackdaws love my big sphinx of quartz.
    myapp.area1 : INFO     How quickly daft jumping zebras vex.
    myapp.area2 : WARNING  Jail zesty vixen who grabbed pay from quack.
    myapp.area2 : ERROR    The five boxing wizards jump quickly.
    # 输出到myapp.log，多了DEBUG的信息输出
    01-27 22:55 root         INFO     Jackdaws love my big sphinx of quartz.
    01-27 22:55 myapp.area1  DEBUG    Quick zephyrs blow, vexing daft Jim.
    01-27 22:55 myapp.area1  INFO     How quickly daft jumping zebras vex.
    01-27 22:55 myapp.area2  WARNING  Jail zesty vixen who grabbed pay from quack.
    01-27 22:55 myapp.area2  ERROR    The five boxing wizards jump quickly.
```

### logging handle 方式

从上例和本例可以看出，logging有一个日志处理的主对象，其它处理方式都是通过addHandler添加进去的。
logging的几种handle方式如下：
 
logging.StreamHandler: 日志输出到流，可以是sys.stderr、sys.stdout或者文件
logging.FileHandler: 日志输出到文件
日志回滚方式，实际使用时用RotatingFileHandler和TimedRotatingFileHandler
logging.handlers.BaseRotatingHandler
logging.handlers.RotatingFileHandler
logging.handlers.TimedRotatingFileHandler
logging.handlers.SocketHandler: 远程输出日志到TCP/IP sockets
logging.handlers.DatagramHandler:  远程输出日志到UDP sockets
logging.handlers.SMTPHandler:  远程输出日志到邮件地址
logging.handlers.SysLogHandler: 日志输出到syslog
logging.handlers.NTEventLogHandler: 远程输出日志到Windows NT/2000/XP的事件日志
logging.handlers.MemoryHandler: 日志输出到内存中的制定buffer
logging.handlers.HTTPHandler: 通过"GET"或"POST"远程输出到HTTP服务器
 
由于StreamHandler和FileHandler是常用的日志处理方式，所以直接包含在logging模块中，而其他方式则包含在logging.handlers模块中

### logging.config模块

通过logging.config模块配置日志
#logger.conf(文件名)
###############################################
```
    [loggers]
    keys=root,example01,example02
    [logger_root]
    level=DEBUG
    handlers=hand01,hand02
    [logger_example01]
    handlers=hand01,hand02
    qualname=example01
    propagate=0
    [logger_example02]
    handlers=hand01,hand03
    qualname=example02
    propagate=0
    ###############################################
    [handlers]
    keys=hand01,hand02,hand03
    [handler_hand01]
    class=StreamHandler
    level=INFO
    formatter=form02
    args=(sys.stderr,)
    [handler_hand02]
    class=FileHandler
    level=DEBUG
    formatter=form01
    args=('myapp.log', 'a')
    [handler_hand03]
    class=handlers.RotatingFileHandler
    level=INFO
    formatter=form02
    args=('myapp.log', 'a', 10*1024*1024, 5)
    ###############################################
    [formatters]
    keys=form01,form02
    [formatter_form01]
    format=%(asctime)s %(filename)s[line:%(lineno)d] %(levelname)s %(message)s
    datefmt=%a, %d %b %Y %H:%M:%S
    [formatter_form02]
    format=%(name)-12s: %(levelname)-8s %(message)s
    datefmt=
```
上例3：
```
    import logging
    import logging.config

    logging.config.fileConfig("logger.conf")
    logger = logging.getLogger("example01")

    logger.debug('This is debug message')
    logger.info('This is info message')
    logger.warning('This is warning message')
```
上例4：
```
    import logging
    import logging.config

    logging.config.fileConfig("logger.conf")
    logger = logging.getLogger("example02")

    logger.debug('This is debug message')
    logger.info('This is info message')
    logger.warning('This is warning message')
```

## Reference Link
1. [调试-廖雪峰](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001431915578556ad30ab3933ae4e82a03ee2e9a4f70871000)
