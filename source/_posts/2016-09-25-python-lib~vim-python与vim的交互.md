title: python-lib~vim-python与vim的交互
date: 2016-09-25 13:58:21
category: [Python, Lib]
tags:
  - vim
description:
----
[Toc]

参考文档：
1. [使用python写vim插件](http://selfboot.cn/2014/11/03/vim_plugin_with_python/)
2. Vim中使用命令行`:h python`——使用手册
3. [如何使用 PYTHON 编写 VIM 插件: VIM 的 PYTHON 接口 – 入门篇](http://xwsoul.com/posts/857)

### Windows下，Gvim支持Python的配置

在Gvim下，执行命令`:version | grep python`
![pic alt](/hexo_blog/images/python-vim-versioninfo.png)

里面有`+python/dyn +python3/dyn`，说明其本身是支持python的
 
在Gvim下，执行以下命令行

    :echo has("python3")
    :echo has("python")

如果返回1，表示当前环境支持Python的调用。

如果返回0，请确认一下几步：
1. 确认安装的Gvim和Python是配套的，即
  * 如果Gvim是32位的，那么Python也需要是32位的；同理，也可以都是64位的。
  * 在Gvim中随意执行一条Python/Python3命令，如果返回错误，无法加载`pythonxx.dll`，确认安装的Python版本与pythonxx对应。
  * 在确认安装的python版本与`pythonxx.dll`对应之后，`pythonxx.dll`通常情况下放在`"C:\Windows\SysWOW64"`路径下（而不是`system32`目录下），需要将该路径添加到环境变量**PATH**中。
2. 【公司电脑在这步骤下还是不行，Gvim中执行`:echo has("python3")`OK，但是最后调用时出现错误 】
3. PS:更换了Vim-64Bit之后，调用python35.DLL，安装了Anaconda3 64bit后，能够成功调用了！
  ![](/hexo_blog/images/python-vim-1.png)
  ![](/hexo_blog/images/python-vim-2.png)
  * 调用`py print("Hello")`，会报错
    `Sorry, this command is disabled, the Python's site module could not be loaded`  
    查找了一下，貌似是说这是python2.7.11版本的Bug，回退成python2.7.10就OK了。具体不再细究，说不定以后就修复了。


### python中获取python版本号的方法

三种方法：
```bash
Python 3.5.0 |Anaconda 2.4.0 (64-bit)| (default, Oct 19 2015, 21:57:25) 
[GCC 4.4.7 20120313 (Red Hat 4.4.7-1)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import platform
>>> print(platform.python_version())
3.5.0
>>> print(sys.version)
3.5.0 |Anaconda 2.4.0 (64-bit)| (default, Oct 19 2015, 21:57:25) 
[GCC 4.4.7 20120313 (Red Hat 4.4.7-1)]
>>> print(sys.version_info)
sys.version_info(major=3, minor=5, micro=0, releaselevel='final', serial=0)
```
可用正则表达式将版本号提取出来

在python中
```python
import vim
def initPythonModule():
    if sys.version_info[:2] < (2, 4):
        vim.command('let s:has_supported_python = 0')
```


### Vim中调用Python命令

在vim中可以使用`py[thon] {stmt}`来执行`python`语句`{stmt}`，你可以用`:python print "Hello World!"`来验证

以下方法可在Vim中用来测试哪个 Python 版本可用: >
```vim
if has('python')
  echo '有 Python 2.x'
elseif has('python3')
  echo '有 Python 3.x'
endif
```

返回Vim中调用的Python的版本号：
```vim
py import platform
py print(platform.python_version())
```
经验证，Ubuntu 14.04环境下，使用pyenv将python配置为`pyenv global Anaconda3-2.4.0`，而系统默认版本为2.7.6
此时，在Vim的命令行下调用python返回版本号为`2.7.6`，即实际调用的还是**系统默认的版本**


#### 执行多行命令

语法如下：
```vim
py[thon] << {endmarker} 
{script} 
{endmarker}
```
这样我们就可以执行python脚本{script}中的内容了。`{endmarker}`是一个标记符号，可以是任何内容，不过**`{endmarker}`后面不能有任何的空白字符。**


#### 在Vim脚本中插入Python 脚本

看一个简单的例子，假设下面代码保存为`script_demo.vim`：
```vim
function! Foo() 
python << EOF
class Foo_demo:
    def __init__(self):
        print 'Foo_demo init' 
Foo_demo() 
EOF
endfunction
```
那么在vim中我们先用`:source script_demo.vim`来加载脚本，然后就可以用`:call Foo()`来运行python脚本了
#### 在Vim中调用Python文件
此外，我们还可以将python脚本放到一个单独的.py文件中，然后用`:pyf[ile] {file}`来运行python文件中的程序，要注意这里pyf[ile]后面的所有参数被看做是一个文件的名字。

### 在Python中调用vim模块
vim提供了一个python模块，有趣的是模块名字就叫做vim，我们可以用它来获取vim编辑器里面的所有信息。
ps：该模块非python内置模块，只能在vim中调用python命令来`import`，如
```vim
:py import vim
:py print(vim.eval("12+12"))
:py print(vim.eval("b:var"))
```
或者：
```vim
function! Bar() 
  python << EOF
  import vim
  cur_buf = vim.current.buffer
  print "Lines: {0}".format(len(cur_buf))
  print "Contents: {0}".format(cur_buf[-1])
  EOF
endfunction
```
参考文档[2]


#### 参数的传递，python获取vim中的对象与变量 

```vim
:h python-eval
```
* `vim.eval(str)`
	- 如果 Vim 表达式计算结果是字符串或者数值，那么返回一个字符串。
	- 如果 Vim 表达式计算结果是 Vim 列表，那么返回一个列表
	- 如果 Vim 表达式计算结果是 Vim 字典，那么返回一个字典
* `vim.bindeval(str)`
  - 可用于修改或调用vim对象与变量(包括b:var的读取与修改(List或dict))
    :let b:a = "ywl"
    :python print(vim.bindeval("b:a"))
* `vim.vars`, `vim.vvars`
  - 分别对应全局(g:)和vim(v:)变量，通过字典的方式读取，等价于`vim.bindeval("g:")`，但更快
  - ex: `:python print(vim.vars['ywl_path'])` 等价于 `:python print(vim.bindeval("g:ywl_path"))`

查看下面简单的例子:
```vim
function! Del(number)
python << EOF
import vim
num = vim.eval("a:number")
vim.command("normal gg{0}dd".format(num))
vim.command("w")
EOF
endfunction
```
在python代码中，可以通过`vim.eval("a:number")`来传递vim变量，等价于`let`语句`let num=a:number`
在vim函数中，同样可以通过参数位置来访问，
比如`let arg=a:0`或者是`vim.eval("a:0")`。我们可以使用`"..."`来代替命名参数来定义一个能接收任意数量参数的函数，不过这样只能通过位置来访问。

#### 双向接口

```vim
:h pyeval()
```
* `pyeval()` & `pyeval3()`
	计算 Python 表达式 {expr} 并返回计算结果，转换为 Vim 本身的数据结构。

#### 执行vim中的command

* vim.command(str): 执行vim中的命令str(ex-mode，命令模式下的命令)，返回值为None，比如：
  ```vim
  :py vim.command("%s/aaa/bbb/g")
  ```
  - 也可以用`vim.command("normal "+str)`来执行normal模式下的命令，比如说用以下命令删除当前行的内容：
    ```vim
    :py vim.command("normal "+'dd')
    ```

#### 获取vim设置option

这个示例将由多个指令完成, 主角是 options 但是是各个 vim 对象的 options .

分别执行如下指令:

* 从全局配置获取是否高亮:
  ```vim
  :py vim.options["hls"]
  ```
* 从当前窗口配置获取是否显示行号:
  ```vim
  :py print vim.current.window.options["nu"]
  ```

* 从当前buffer配置获取tab长度
  ```vim
  :py print vim.current.buffer.options["ts"]
  ```

尽管可能所有选项都是在 _vimrc 文件配置的, 但是获取的方式可能还是会根据其生效区块不同而采用了不同的获取方式

