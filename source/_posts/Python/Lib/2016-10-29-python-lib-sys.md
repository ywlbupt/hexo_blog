title: python-lib~sys
date: 2016-10-29 08:36:12
category:
tags:
description:
---
[Toc]

### sys.modules字典

`sys.modules`是一个全局字典，该字典是Python启动后就加载在内存中。

每当程序员导入新的模块，**`sys.modules`都将记录这些模块**。字典sys.modules对于加载模块起到了缓冲的作用。

当某个模块第一次导入，字典`sys.modules`将自动记录该模块。

当第二次再导入该模块时，python会直接到字典中查找，从而加快了程序运行的速度。

字典`sys.modules`具有字典所拥有的一切方法，可以通过这些方法了解当前的环境加载了哪些模块。

```python
import  sys
print sys.modules.keys()
print sys.modules.values()
print sys.modules["os"]
```
```bash
>>> import sys
>>> print(sys.modules["os"])
<module 'os' from '/home/ywl/.pyenv/versions/anaconda3-2.4.0/lib/python3.5/os.py'>
```

### sys Usage

*   使用sys.platform获得当前平台
    `>> sys.platform`  返回当前平台 出现如： "win32" "linux" 等
*   使用`sys.exit(1)` 退出程序
    * 注意 `sys.exit` 并不是立即退出. 而是引发一个 `SystemExit` 异常. 这意味着你可以在主程序中捕获对 `sys.exit` 的调用
    * `sys.exit(0)`返回值为0是正常退出程序，其余值`0~127`均为不正常的退出
*   `sys.path` 模块搜索路径列表

### sys.stdout/stderr 标准输出，错误信息的重定向
*   可处理标准输出/输入(通常缩写为 `stdout` 和 `stderr`) 
    *   `stdout` `stderr`是内建在每一个 UNIX 系统中的管道。..
        *   当print 某些东西时，结果前往 `stdout` 管道；
        *   当程序崩溃并打印出调试信息 (例如 Python 中的 traceback (错误跟踪)) 的时候，信息前往 `stderr` 管道
    *   `stdout`, `stderr`都是类文件对象，都是只写的。通过 `sys.stdout.write(string)`操作

#### 打印重定向

向标准错误写入错误信息是很常见的，所以有一种较快的语法可以立刻导出信息

*   python 2
    ```python
    >>> import sys
    >>> print >> sys.stderr, 'entering function'
    entering function 
    ```
*   python 3
    ```python
    import sys
    print("entering function", file=sys.stderr)
    ```


