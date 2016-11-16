title: vim-plugin~源码安装vim添加python3的支持
date: 2016-09-27 14:12:30
category:
tags:
description:
----
[Toc]



[Valloric, YCM 权威指导](www.github.com/Valloric/YouCompleteMe/wiki/Building-Vim-from-source)

## 安装依赖包

* for Ubuntu
```bash
sudo apt-get install libncurses5-dev libgnome2-dev libgnomeui-dev \
    libgtk2.0-dev libatk1.0-dev libbonoboui2-dev \
    libcairo2-dev libx11-dev libxpm-dev libxt-dev python-dev \
    python3-dev ruby-dev lua5.1 lua5.1-dev libperl-dev git
```

## 移除已安装的vim 组件

```bash
sudo apt-get remove vim vim-runtime gvim
sudo apt-get remove vim-tiny vim-common vim-gui-common vim-nox
```

可通过`dpkg -l | grep vim`查看残余组件

然后通过
```bash
sudo dpkg -r packagename
```
移除

## 针对Ubuntu 14.04

1. github下载源码
```bash
cd ~
git clone https://github.com/vim/vim.git
```
2. configure
```bash
cd vim
./configure --with-features=huge \
            --enable-multibyte \
            --enable-rubyinterp=yes \
            --enable-pythoninterp=yes \
            --with-python-config-dir=/usr/lib/python2.7/config \
            --enable-python3interp=yes \
            --with-python3-config-dir=/usr/lib/python3.5/config \
            --enable-perlinterp=yes \
            --enable-luainterp=yes \
            --enable-gui=gtk2 --enable-cscope --prefix=/usr
```
其中，--enable-pythoninterp、--enable-rubyinterp、--enable-perlinterp、--enable-luainterp 等分别表示支持 ruby、python、perl、lua 编写的插件，--enable-gui=gtk2 表示生成采用 GNOME2 风格的 gvim，--enable-cscope 支持 cscope，--with-python-config-dir=/usr/lib/python2.7/config/ 指定 python 路径

在Ubuntu64位系统中，python-config-dir=`/usr/lib/python2.7/config-x86_64-linux-gnu`
参考以下：
```bash
./configure --with-features=huge \
            --enable-multibyte \
            --enable-rubyinterp=yes \
            --enable-pythoninterp=yes\
            --with-python-config-dir=/usr/lib/python2.7/config-x86_64-linux-gnu \
            --enable-perlinterp=yes \
            --enable-luainterp=yes \
            --enable-gui=gtk2 --enable-cscope --prefix=/usr

            --enable-python3interp=yes\
            --with-python3-config-dir=/usr/lib/python3.4/config-3.4m-x86_64-linux-gnu \
```

3.  `make VIMRUNTIMEDIR=/usr/share/vim/vim80`
    VIMRUNTIMEDIR不是必须，vim版本可以通过`git log`或者`git tag`查看确认
4.  使用`checkinstall`替代原有`make install`安装
```bash
sudo checkinstall
```
5. 打开vim，查看是否支持python支持 `:echo has("python")`
6. 设置Vim为默认编辑器，实测，前两行有效果，vi可能未装
   可通过`$ update-alternatives --config editor ` 查看
``` bash
sudo update-alternatives --install /usr/bin/editor editor /usr/bin/vim 1
sudo update-alternatives --set editor /usr/bin/vim
sudo update-alternatives --install /usr/bin/vi vi /usr/bin/vim 1
sudo update-alternatives --set vi /usr/bin/vim
```
