title: python-lib~os
date: 2016-09-24 09:53:44
category: [Python, Lib]
tags: 
  - python
  - os
description:
---
[Toc]

## os

os负责程序与操作系统的交互，sys负责程序与python解释器的交互。

### 文件目录操作

> python 3.5.2  
> On Unix, path can be of type of `str` or `bytes` (use `fsencode()` and `fsdecode()` to encode bytes paths)  
> On Windows, path must be of type `str`  
> On Unix-based systems, scandir() uses the system's `opendir()` and `readdir()` functions. On Windows, it uses the Win32 `FindFirstFileW` and `FindNextFileW` functions.

* `os.listdir(path='.')` 返回指定路径下的目录与文件
  - 在ver3.5后，使用`os.scandir()`会更加灵活可操作[os.scandir](#scan)
  - _Avaliable : Unix, Windows_
* `os.walk(top, ...)` 递归遍历path路径下的文件
  - 返回一个三元数组，[(root ,dirs, files),...] 其中，`dirs`和`files`为list
* `os.path.split(name)`  分割文件名与目录（事实上，如果你完全使用目录，它也会将最后一个目录作为文件名而分离，同时它不会判断文件或目录是否存在）

#### New in python 3

<span id="scan"></span> 
* `os.scandir(path='.')`，返回可迭代的对象`DirEntry`
  - note: New in ver 3.5
  * `DirEntry`
    - name, path, inode, is_dir, is_file, is_symlink, stat


### os.path 路径操作

* `os.sep` 返回当前系统的路径分隔符，Unix下为`/`
* `os.path.isfile(path)` 判断path是否为文件
* `os.path.exists(path)` 判断path是否存在
* `os.path.join(path, *paths)` 连接路径，会自动加上系统路径分隔符 `os.sep`
* `os.path.relpath(path[, start])` 返回path相对与start的相对路径


* `os.path.expandvars(path)`解析环境变量，linux下支持`$HOME`, `${HOME}`形式，Windows下额外支持`%HOME%`形式
  ```python
  $ s.path.expandvars("$HOME/hexo")
  >> '/home/ywl/hexo'
  ```
* `os.path.normcase(path)`
  * 对于Windows路径，可将斜号替换为反斜号返回
  * 对于Unix or Mac OS X ，返回 `path`  Unchanged
* `os.path.normpath(path)`
  * 可格式化`path`字符串，如将`a/./b`转换为`a/b`等等
