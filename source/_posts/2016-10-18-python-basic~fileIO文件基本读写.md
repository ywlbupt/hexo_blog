title: python-basic~fileIO文件基本读写操作
date: 2016-10-18 08:34:17
category: [Python, Basic]
tags:
  - codecs
description:
----
[Toc]

# 【python】【file】文件的读写基本操作

### 【读写模式】字符创直接读取
* 'r' open for reading (**default**) (只读模式)
* 'r+'  'r+' opens the file for both reading and writing. (可读可写)
* 'w' open for writing, truncating the file first (只写模式，若文件存在，则会覆盖前文件)
* 'w+' open for writing and reading, truncating the file first 
* 'x' open for exclusive creation, failing if the file already exists (如果文件存在，打开失败，抛出Error)
* 'a' open for writing, appending to the end of the file if it exists(若文件存在，则将新行加在文件的末尾)

### 【编码模式】：二进制模式/文本模式
* 'b' binary mode
* 't' text mode (default)
* '+' open a disk file for updating (reading and writing)
* 'U' universal newlines mode (deprecated, 不赞成)
* For binary read-write access, 
    * the mode 'w+b' opens and truncates the file to 0 bytes. 
    * 'r+b' opens the file without truncation.

## File open() & ｗrite() & read()
```python
with open('./file_example.txt','a') as f :
    f.write("hello world!\r")
with open('./file_example.txt','r') as f :
    print(f.read())
```

<!-- more -->

### open(), 获取一个文件对象；
  打开了文件之后，处理完我们需要的操作之后，最后一部是调用f.close()来关闭文件
  如果文件不存在，将抛出 IOError 的异常，此时可能不会调用 f.close()来释放资源
  可以使用 try...finally来保证，或者是with...as 语句来自动调用

```python
try:
    f = open('./file_example.txt', 'r')
    print(f.read())
finally:
    if f:
        f.close()
```
或者是

```
with open('./file_example.txt','r') as f :
    print (f.read())
```

#### python对不同平台换行符的处理

> In text mode (mode argument 't'), the default when reading is to convert platform-specific line endings (`\n` on Unix, `\r\n` on Windows) to just `\n`. 
> When writing in text mode, the default is to convert occurrences of `\n` back to platform-specific line endings.

#### python打开文件的编码方式

在text模式下，如果没有指定编码方式（specified encoding），则默认采用 平台的编码来解码（platform-dependent encoding）
```python
with open ("./temp.txt",'r', encoding='utf-8') as fd :
    pass
```

#### codecs.open()

考虑到**共存**问题，可以使用`codecs`库来兼容

* 对于**python 2** 
  * 使用`open()`打开文件会有一些问题，open打开文件只能写入`str`类型，不管字符串是什么编码方式的。
    ```python
    fr = open("test.txt", 'a')
    line1="我爱祖国"
    fr.write(line1)
    ```
    这样是完全可以的，因为 line1 在python2中是字符串类型。
  * 但有时我们会遇到写入文件时编码不统一的情况(utf-8, gbk)，一般会转换成unicode，此时写入文件就有问题了：  
    ```python
    line2=u"我爱祖国"
    fr.write()
    ```
    会报错，抛出`UnicodeEncodeError`的错误
  * 对于上面的错误，可以采用先将`line2`编码成str类型，过程如下：  
    `input(utf-8) -> decode -> unicode -> encode -> output(gbk)`
  * 代替这个繁琐的操作的方法是采用`codecs.open()`，
    ```python
    import  codecs 
    fw = codecs.open('test.txt', 'a', encoding = 'utf-8')
    fw.write(line2)  # line2为unicode类型
    ```
    > 这种方法可以指定一个编码打开文件，使用这个方法打开的文件读取返回的将是unicode。写入时，如果参数 是unicode，则使用open()时指定的编码进行编码后写入；
    > 如果是str，则先根据源代码文件声明的字符编码，解码成unicode后再进行前述操作。
    > 相对内置的open()来说，这个方法比较不容易在编码上出现问题。

### f.seek()方法移动文件指针

* `f.seek(0)`指针指向文件起始位置

### f.write()方法写入文件

* f.write() 写入后不换行，下次写入接着写
* f.writeline() 写入后换行，下次会写在下一行

可以反复调用write()来写入文件，但是务必要调用f.close()来关闭文件。

当我们写文件时，操作系统往往不会立刻把数据写入磁盘，而是放到内存缓存起来，空闲的时候再慢慢写入。只有调用close()方法时，操作系统才保证把没有写入的数据全部写入磁盘。忘记调用close()的后果是数据可能只写了一部分到磁盘，剩下的丢失了。

所以，还是用with语句来得保险：

```python
with open('./file_example.txt','w') as f :
    f.write("hello world!")
```

#### 修改特定行

参考文档：
1. [知乎-使用mmap模块读取大文件](https://www.zhihu.com/question/27028777)

Python编辑文件的时候本身并不具备修改文件的功能，我们只能采用读取文件内容，然后再写入文件。

* 对于小文件，可以全部读出后再覆盖写入，以`r+`模式`open()`
* 对于大文件，可新建一个文件，
  * 一边 `f.readline()` 
  * 一边修改`line.replace("old_text", "new_text")`
  * 并`f.writeline()`写入


TODO
 
### f.read()方法

* f.read()可以一次性读取文件的内容
* f.read(size) 方法，每次最多读取 size 个字节的内容
* f.readlines() : 如果是配置文件，使用 f.readlines()会更加方便.
  * 一次性读取整个文件，返回行的列表
* f.readline() : 每次读取一行，适用于较大的文件

```python
for line in f.readlines():
    print(line.rstrip()) # 去掉末尾的 '\n'
```
