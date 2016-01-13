title: Jekyll Ubuntu下的安装
date: 2016-01-02 12:45:13
tags: Jekyll
description: Jekyll Ubuntu 下的安装
---

## Jekyll Ubuntu 下的安装

### 安装思路

安装Ruby Jekyll需要运行在Ruby2.0以上的版本，但是Ubuntu使用apt-get安装的版本是1.9.3，所以需要更高版本来安装 

1. 先安装RVM
2. 用RVM安装Ruby
3. 安装jekyll

### RVM安装

1. 安装RVM（参考 [http://rvm.io/])
2. 载入 RVM 环境（新开 Termal 就不用这么做了，会自动重新载入的）  
    `source ~/.rvm/scripts/rvm`
3. 检查一下是否安装正确：  
    `rvm -v`

### 使用RVM安装Ruby

    rvm install 2.2.0

RVM 装好以后，需要执行下面的命令将指定版本的 Ruby 设置为系统默认版本:

    rvm 2.2.0 --default

确认安装版本信息

    ruby -v

### 安装Jekyll

_更新完Ruby的版本后依然出现了`jekyll requires Ruby version >= 2.0.0.`的错误信息 _

原因：**when you run with sudo it goes to system ruby** 

    ywl@ywl-ThinkPad-E420:~$ sudo ruby -v
    ruby 1.9.3p484 (2013-11-22 revision 43786) [x86_64-linux]

1. 方法一，安装jekyll2, 仅依赖ruby 1.9.3或以上  

        sudo gem install jekyll -v 2.5.3

2. 抛开`sudo`：

        gem install jekyll --pre
        # --pre安装最新版本的jekyll，预览版

安装markdown解析器

    gem install redcarpet/ridscount

---------------

## 参考链接
1. [Ubuntu安装Jekyll异常-RVM安装](http://www.rainstops.com/tool/2015/02/11/jekyll-install.html)
2. [Integrating RVM with gnome-terminal](https://rvm.io/integration/gnome-terminal#integrating-rvm-with-gnome-terminal)

