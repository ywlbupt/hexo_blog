title: ubuntu-IDE~atom安装
date: 2016-11-24 00:10:19
category: [Ubuntu, IDE]
tags:
description:
---
[Toc]

## 通过PPA安装

``` bash
sudo add-apt-repository ppa:webupd8team/atom
sudo apt-get update
sudo apt-get install atom
```

安装后，如果通过图标打开后出错，那么可以通过以下命令开启 Atom：

``` bash
/opt/atom/atom
```

## 卸载Atom

``` bash
sudo apt-get remove atom
sudo add-apt-repository --remove ppa:webupd8team/atom
```
以上只会卸载该软件，要卸载附加的一些软件包，请使用以下命令卸载多余的软件包：

``` bash
sudo apt-get autoremove
```

