title: raspberry~开始配置树莓派
date: 2016-11-08 22:43:40
category: [Raspberry]
tags:
description:
---
[Toc]

* [比较权威的官方指导](http://elinux.org/RPi_Easy_SD_Card_Setup)

买的是树莓派3 B，自带WiFi，蓝牙

## 刷系统

1. [官方quick start, noobs](https://www.raspberrypi.org/learning/software-guide/quickstart/)
    * `noobs` offline and network install
    * `etcher.io` 烧写image文件
    * `formatter` 格式化SD卡

为了避免无畏的工作，我直接购买了系统卡，以便快速上手。

在公司简单尝试了下刷系统，如果是在Windows下，其实还挺简单的。


基本思路是：
1.  下对应板子的系统，
    鉴于raspberry Pi官方的系统资源比较多，就选择直接安装它。不捣鼓其他的版本了
2.  在Windows下，下载刷镜像的软件win32disk 
3.  然后一顿傻瓜操作。。。

过几天，我把上回购买的树莓派1 B+拿出来按同样的思路捣鼓一下，变废为宝。


## SSH 登录

对于树莓派3，默认开启SSH服务。（如果树莓派1不开启，就接上显示屏打开配置）

如果没有显示屏，从另一个角度，可以采用端口扫描的方法，扫出树莓派的IP地址。

```bash
# 排除192.168.99.1这个路由器地址，查看哪个端口的port 22是Open状态
$ nmap -v -sP 192.168.99.2-255
```

```bash
$ ssh pi@<ip address>
```
用户名为`pi`，密码默认为`raspberry`

## 软件更新`sudo apt-get install update`

google一下，修改软件源，不然更新太慢了。 

## 安装`Tmux`

试过未更新软件包，直接安装`tmux`，会提示找不到包`tmux`

```bash
$ sudo apt-get install tmux
```

然后将`~/.tmux.conf`(该文件从`Vim_ywl`git库中找)里面的内容拷贝到Raspberry相应的文件中。(拷贝前，可以先安装xclip，利用系统剪贴板方便操作)

## 安装中文输入法-谷歌

```bash
$ sudo apt-get install fcitx fcitx-googlepinyin fcitx-module-cloudpinyin fcitx-sunpinyin
```
安装完毕，重启即可

重启后，需打开`fcitx`配置界面，将Google pinyin添加到输入法选项中。

## 安装Vim

安装`Ctags`
```bash
$ sudo apt-get install exuberant-ctags
```

配置github的ssh登录，克隆`Vim_ywl`库
