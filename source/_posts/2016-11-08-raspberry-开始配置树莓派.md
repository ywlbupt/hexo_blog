title: raspberry~开始配置树莓派
date: 2016-11-08 22:43:category: [Raspberry]
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

## 启用root账户

树莓派使用的linux是debian系统，所以树莓派启用root和debian是相同的。

debian里root账户默认没有密码，但账户锁定。

当需要root权限时，由默认账户经由sudo执行，Raspberry pi 系统中的Raspbian

**默认用户是pi 密码为raspberry**

重新开启root账号，可由pi用户登录后，在命令行下执行

``` bash
sudo passwd root
```
执行此命令后系统会提示输入两遍的root密码，输入你想设的密码即可，然后在执行

``` bash
sudo passwd --unlock root
```
这样就可以解锁root账户了。

当从root用户切换到pi用户后，我们再次使用`su passwd root`来修改密码，会报错


## SSH 登录

对于树莓派3，默认开启SSH服务。（如果树莓派1不开启，就接上显示屏打开配置）

如果没有显示屏，从另一个角度，可以采用端口扫描的方法，扫出树莓派的IP地址。

```bash
# 排除192.168.99.1这个路由器地址，查看哪个端口的port 22是Open状态
$ nmap -sP 192.168.99.100-254
```
示例输出：
``` bash
Starting Nmap 6.40 ( http://nmap.org ) at 2016-11-28 23:46 CST
Nmap scan report for MI5sPlus-xiaomishouj.lan (192.168.99.103)
Host is up (0.091s latency).
Nmap scan report for chuangmi-plug-m1_miio45324900.lan (192.168.99.105)
Host is up (0.049s latency).
Nmap scan report for zhimi-airpurifier-m1_miio12912092.lan (192.168.99.107)
Host is up (0.11s latency).
Nmap scan report for 192.168.99.137
Host is up (0.00057s latency).
Nmap scan report for raspberrypi.lan (192.168.99.144)
Host is up (0.00022s latency).
Nmap scan report for MI5s-xiaomishouji.lan (192.168.99.153)
Host is up (0.24s latency).
Nmap scan report for ywl-ThinkPad-E420.lan (192.168.99.210)
Host is up (0.00012s latency).
Nmap scan report for ESP_C37984.lan (192.168.99.216)
Host is up (0.058s latency).
Nmap scan report for zhimi-airpurifier-m1_miio12647059.lan (192.168.99.224)
Host is up (0.052s latency).
Nmap done: 155 IP addresses (9 hosts up) scanned in 19.43 seconds
```

或者：
``` bash
$ nmap -v -sT 192.168.99.100-254
```

```bash
$ ssh pi@<ip address>
```
用户名为`pi`，密码默认为`raspberry`

## 修改树莓派hostname

1.  修改主机名，`sudo vim /etc/hostname`
    修改成想要的主机名字
2.  修改hosts文件，`sudo vim /etc/hosts`
    替换原先所有的主机名
3.  重启设备，生效

## 查看用户与用户组信息

``` bash
grep pi /etc/passwd /etc/group /etc/shadow
```

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
