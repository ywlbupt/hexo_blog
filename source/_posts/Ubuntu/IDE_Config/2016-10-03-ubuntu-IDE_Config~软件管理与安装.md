title: ubuntu-IDE_Config~软件管理
date: 2016-10-03 23:19:34
category: [Ubuntu, IDE_Config]
tags:
  - deb
description:
----
[Toc]

### deb软件包的安装

* deb 是 ubuntu 、debian 的格式。
* rpm 是 redhat 、fedora 、suse 的格式

>   deb是debian发行版的软件包
>   ubuntu是基于debian 发行的 所有可以用

进入`.deb`软件包的文件路径

You need to use `dpkg` package manager from shell/command prompt. 

**`dpkg`** is a tool to install, build, remove and manage packages. dpkg itself is controlled entirely via command line parameters. **For example -i use to install .deb file.**


```bash
sudo dpkg -i package.deb
```

### PPA 安装

```bash
sudo add-apt-repository ppa:tualatrix/ppa
sudo apt-get update
sudo apt-get install ubuntu-tweak
```

安装完之后在Dash面板搜索Ubuntu Tweak即可启动。

### 卸载

要卸载该软件请执行以下命令：

```bash
sudo apt-get remove ubuntu-tweak
```

使用下面的命令去除PPA：

```bash
sudo apt-get install ppa-purge
sudo ppa-purge ppa:tualatrix/ppa
```


