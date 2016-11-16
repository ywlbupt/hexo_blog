title: ubuntu-IDE_Config~软件管理
date: 2016-10-03 23:19:34
category: [Ubuntu, IDE]
tags:
  - deb
description:
----
[Toc]

### 源码安装，以vim 为例

If you want to be able to easily uninstall vim use checkinstall.

```bash
sudo apt-get install checkinstall
cd ~/vim
sudo checkinstall
```
Otherwise, you can use make to install.

```bash
cd ~/vim
sudo make install
```

通过checkinstall安装的软件，可以通过`dpkg -r packagename`来删除软件

### deb软件包的安装

针对本地软件包，不解决依赖关系 

* deb 是 ubuntu 、debian 的格式。
* rpm 是 redhat 、fedora 、suse 的格式

>   deb是debian发行版的软件包
>   ubuntu是基于debian 发行的 所有可以用

进入`.deb`软件包的文件路径

You need to use `dpkg` package manager from shell/command prompt. 

**`dpkg`** is a tool to install, build, remove and manage packages. dpkg itself is controlled entirely via command line parameters. **For example -i use to install .deb file.**


```bash
sudo dpkg -i packagename.deb
```

使用dpkg安装的软件包，，可以通过
```bash
sudo dpkg -r packagename
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


