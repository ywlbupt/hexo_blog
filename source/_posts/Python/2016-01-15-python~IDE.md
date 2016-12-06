title: python-IDE
date: 2016-01-15 00:57:52
tags: python
category: [Python,Python_Basic]
description: python开发环境
----


### pyenv管理不同的版本

经常遇到这样的情况：

* 系统自带的Python是2.6，自己需要Python 2.7中的某些特性；
* 系统自带的Python是2.x，自己需要Python 3.x；
此时需要在系统中安装多个Python，但又不能影响系统自带的Python，即需要实现Python的多版本共存。pyenv就是这样一个Python版本管理器。

#### 安装pyenv

    $ git clone git://github.com/yyuu/pyenv.git ~/.pyenv
    $ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
    $ echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
    $ echo 'eval "$(pyenv init -)"' >> ~/.bashrc
    $ exec $SHELL -l

#### 安装Python
查看可安装的版本

    $ pyenv install --list
该命令会列出可以用pyenv安装的Python版本，仅列举几个:

    2.7.8   # Python 2最新版本
    3.4.1   # Python 3最新版本
    anaconda-2.0.1  # 支持Python 2.6和2.7
    anaconda3-2.0.1 # 支持Python 3.3和3.4
其中形如 x.x.x 这样的只有版本号的为Python官方版本，其他的形如 xxxxx-x.x.x 这种既有名称又有版本后的属于“衍生版”或发行版。


#### 安装Python的依赖包

如果我们确定要安装python3.4.3的话，接下来我们就可以安装python了，但是再安装之前，我们必须要安装python所需要的依赖包，这个必须要安装，安装会失败的：

``` bash
sudo apt-get install libc6-dev gcc
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm
```

上面的依赖包搞定之后，我们就可以安装python了：

``` bash
pyenv install 3.4.3 -v
```

该命令会从github上下载python的源代码，并解压到/tmp目录下，然后在/tmp中执行编译工作。若依赖包没有安装，则会出现编译错误，需要在安装依赖包滞后重新执行该命令。

安装完成之后，需要使用如下命令对数据库进行更新：

    pyenv rehash

#### 安装本地下载好的Python安装包

到官网把安装包单独下载好，比如我讲下载好的包 `Python-3.5.1.tar.xz`

放到 `~/Downloads` 目录下，然后指定build缓存目录后，再运行install命令
``` bash
export PYTHON_BUILD_CACHE_PATH=~/Downloads
pyenv install 3.5.1
pyenv rehash
```

> 注，另一种说法
> 如果网络不太好，用 pyenv 下载会比较慢，可以先执行该命令，
> 然后到 ~/.pyenv/cache 目录下查看要下载的文件的文件名，
> 然后自己到官方网站下载，并将文件放在 ~/.pyenv/cache 目录下。
> pyenv 会检查文件的完整性，若确认无误，则不会再重新下载。

#### 查看当前已经安装的python版本

    $ pyenv versions
    * system (set by /home/seisman/.pyenv/version)
    3.4.3
其中星号代表是当前系统正在使用的python版本是系统自带的。

#### 设置全局的python版本

    $ pyenv global 3.4.3
    $ pyenv versions
    system
    * 3.4.3 (set by /home/seisman/.pyenv/version)

从上面，我们可以看出来当前的python版本已经变为了3.4.3。也可以使用pyenv local或pyenv shell临时改变python的版本。

#### 确认python版本

    $ python
    Python 3.4.3 (default, Apr  1 2015, 19:10:43) 
    [GCC 4.8.2] on linux
    Type "help", "copyright", "credits" or "license" for more information.

注意事项：

使用python
输入 python 即可使用新版本的python；
系统自带的脚本会以 /usr/bin/python 的方式直接调用老版本的python，因而不会对系统脚本产生影响；
使用 pip 安装第三方模块时会安装到 ~/.pyenv/versions/3.4.1 下，不会和系统模块发生冲突。
使用 pip 安装模块后，可能需要执行 `pyenv rehash` 更新数据库；

#### 参考链接

1. [python多版本共存之pyenv](http://seisman.info/python-pyenv.html)

### pip包管理命令

1. install                     Install packages.  
2. uninstall                   Uninstall packages.  
3. freeze                      Output installed packages in requirements format.  
4. list                        List installed packages.  
5. show                        Show information about installed packages.  
6. search                      Search PyPI for packages.  
7. wheel                       Build wheels from your requirements.  
8. zip                         DEPRECATED. Zip individual packages.  
9. unzip                       DEPRECATED. Unzip individual packages.  
10. help                       Show help for commands.  

### windows下pip的安装

pip也可以在Windows下安装，使用也和linux下一样简单。


先从下面的地址下载pip源码`source`：

http://pypi.python.org/pypi/pip/

下载下来后解压，打开命令行：

1.  利用cd命令进入pip源码目录，假设源码放在F:\python\pip-6.0.8，则执行下面的命令：

        cd f:\python\pip-6.0.8  

2.  在安装前必须确保python的路径已经加入到系统的PATH环境变量中，如果没有，可以用下面的命令来指定：

        set PATH=%PATH%;"your python install path"  

3.  再运行下面的命令进行安装：

        python setup.py install  
    等待片刻，一堆打印过后即可安装成功。


要安装python包也很简单，只要运行命令：

	python -m pip install <package name>  
如要安装mysql的python支持包，可执行下面的命令：

	python -m pip install mysql  
完整的pip命令如下：

### 其他

#### [python][pip][Pillow]安装
环境：python 3.4.3, Ubuntu 14.04

直接运行命令

    pip install Pillow
如果出现以下错误：

    ValueError: --enable-jpeg requested but jpeg not found, aborting.
说明没有安装Jpeg的底包

安装:

    sudo apt-get install libjpeg-dev
然后继续安装即可

### windows下lxml的安装

背景：在cmd命令下直接使用`pip3 install lxml`会出现缺少VC++依赖库的提示。

可直接下载二进制包安装

作者：深海鱼
链接：[http://www.zhihu.com/question/26857761/answer/69754633]
来源：知乎

1.  安装wheel，命令行运行：`pip3 install wheel`
2.  在这里下载对应的.whl文件，注意别改文件名！[http://www.lfd.uci.edu/~gohlke/pythonlibs/#lxml] 
    Ctrl+F，输入lxml，找到

        Lxml, a binding for the libxml2 and libxslt libraries.
        lxml-3.4.4-cp27-none-win32.whl
        lxml-3.4.4-cp27-none-win_amd64.whl
        lxml-3.4.4-cp34-none-win32.whl
        lxml-3.4.4-cp34-none-win_amd64.whl
        lxml-3.4.4-cp35-none-win32.whl
        lxml-3.4.4-cp35-none-win_amd64.whl
        lxml-3.5.0-cp27-none-win32.whl
        lxml-3.5.0-cp27-none-win_amd64.whl
        lxml-3.5.0-cp34-none-win32.whl
        lxml-3.5.0-cp34-none-win_amd64.whl
        lxml-3.5.0-cp35-none-win32.whl
        lxml-3.5.0-cp35-none-win_amd64.whl
    cp后面是Python的版本号，27表示2.7，amd64表示64位系统，根据你的Python版本选择下载。
3. 进入.whl所在的文件夹，执行命令即可完成安装`pip install 带后缀的完整文件名`

### matplotlib安装

参考链接 ： 
1. [官方文档](http://matplotlib.org/users/installing.html)

** Build requirements **
These are external packages which you will need to install before installing matplotlib.

If you are installing dependencies with a package manager on Linux, you may need to install the development packages (look for a "-de"” postf) in addition to the librari themselves


#### Building on Linux
It is easiest to use your system package manager to install the dependencies.

If you are on Debian/Ubuntu, you can get all the dependencies required to build matplotlib with:

    sudo apt-get build-dep python-matplotlib

This does not build matplotlib, but it does get the install the build dependencies, which will make building from source easier.

然后使用pip install matplotlib即可

#### backend 
I had the same issue, and installing matplotlib using easy_install instead of pip did not solve it. In the end, I found out that the problem was simply that matplotlib could not find any backend for plotting.

I solved it by doing the following (I am using Debian wheezy):

    pip uninstall matplotlib
    sudo apt-get install tcl-dev tk-dev
    pip install matplotlib
