title: linux软件管理
date: 2016-01-16 11:25:08
tags: linux
description:
---


### deb软件包的安装

* deb 是 ubuntu 、debian 的格式。
* rpm 是 redhat 、fedora 、suse 的格式

>   deb是debian发行版的软件包
    ubuntu是基于debian 发行的 所有可以用

进入.deb软件包的文件路径

You need to use `dpkg` package manager from shell/command prompt. 

dpkg is a tool to install, build, remove and manage packages. dpkg itself is controlled entirely via command line parameters. **For example -i use to install .deb file.**

    sudo dpkg -i package.deb
