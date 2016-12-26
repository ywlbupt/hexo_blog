title: ubuntu-basic~磁盘分区格式化与备份
date: 2016-11-20 14:36:09
category: [Ubuntu, Basic]
tags:
    - fdisk
    - df
    - dd
description:
---
[Toc]

## 衍生操作

1. [使用dd对系统进行备份还原](http://blog.csdn.net/shendl/article/details/7384755)
    * 需要安装了ubuntu live的系统
2. [官方？-Ubuntu的备份与还原-包含MBR](http://wiki.ubuntu.org.cn/Ubuntu%E5%A4%87%E4%BB%BD%E4%B8%8E%E8%BF%98%E5%8E%9F)

## 分区结构

1. 主分区，primary
2. 扩展分区，extend
3. 逻辑分区，Logical

查看系统内设备的所有分区，系统会把整个系统内可以找到的设备的分区都列出来
``` bash
sudo fdisk -l
```

* 一个硬盘主分区至少有1个，最多4个，扩展分区可以没有，最多1个
* 且主分区+扩展分区总共不能超过4个。逻辑分区可以有若干个
* 分出主分区后，其余的部分可以分成扩展分区，一般是剩下的部分全部分成扩展分区，
  也可以不全分，剩下的部分就浪费了。
* 扩展分区不能直接使用，必须分成若干逻辑分区。所有的逻辑分区都是扩展分区的一份
    * 逻辑分区的号码从5开始

### 空间容量计算

* cylinder 柱面
* sectors 扇区
* heads 磁头

每个柱面有63个扇区，每个扇区有255个磁头

一块磁盘的空间大小计算方式为：这块磁盘的总的磁头数量（Heads）*512bytes（因为每个磁头数量为512字节）

### Example

如下图，存在一个主分区`sda1`，一个拓展分区Extend`sda2`，以及一个逻辑分区`sda5`
逻辑分区`sda5`是从拓展分区`sda2`中分出的。且Units号码`start`,`End`在`sda2`里面

``` bash
Command (m for help): p
Disk /dev/sda: 14.5 GiB, 15518920704 bytes, 30310392 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
<F8>I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x20ea20e9

Device     Boot    Start      End  Sectors  Size Id Type
/dev/sda1           2048 16779263 16777216    8G 83 Linux
/dev/sda2       16779264 30310391 13531128  6.5G  5 Extended
/dev/sda5       16781312 30310391 13529080  6.5G 83 Linux
```

在w写入操作之后，会出现以下的提示：
``` bash
Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Re-reading the partition table failed.: Device or resource busy

The kernel still uses the old table. The new table will be used at the next reboot or after you run partprobe(8) or kpartx(8).
```
警告信息，因为含有根目录，磁盘无法卸载，所以内核重新取得分区表信息，
此时系统要求我们重新启动reboot以更新分区表信息。
``` bash
sudo partprobe
```

## 使用磁盘分区，格式化与dd备份的正确姿势

### 各设备挂载名称
* 在linux系统下，通常SD卡挂载的设备名称为`mmcblk0`
    * 其中，mmc为SD卡的前身，SD与MMC的构造差不多
* **U盘的挂载名称与硬盘一致，在磁盘操作的时候不要混淆导致误操作**

### 磁盘分区

真正挂载到系统的不是类似`/dev/mmcblk0`或者`/dev/sda`这种不带后缀的设备。
而是如`/dev/mmcblk0p0` `/dev/sda1` 等这样的磁盘分区partition

对磁盘的操作，可通过`fdisk`来完成，傻瓜式操作或者参考`man fdisk`

### mkfs 格式化磁盘

文件系统 TBD

对于U盘而言，vfat格式可以在linux和windows中操作

分区完后需要对磁盘进行格式化，然后才是重新挂载。

``` bash
mkfs [-t 文件系统格式] 设备文件名
# 参考
sudo mkfs -t vfat /dev/psda1
```

当我们使用`mkfs -t vfat`命令的时候，实际上是调用`mkfs.vfat`命令来进行的格式化操作

### 挂载与卸载分区

* 单一的系统不应该被重复挂载在不同的挂载点（目录）中
* 单一的目录不应该重复挂载着多个文件系统

``` bash
# 一个空目录psda1，将分区挂载在非空目录会导致该目录下的文件暂时消失
sudo mkdir /mnt/psda1
mount /dev/sda1 /mnt/psda1
```

### dd对树莓派进行备份

``` bash
sudo dd if=/dev/mmcblk0  of=/dev/psda1/disk.img
```

在备份的同时，压缩数据，节省空间（注：这里我遇到了gzip读写U盘权限的问题）
``` bash
sudo dd if=/dev/mmcblk0 | gzip>/mnt/psda1/disk.img.gz
```

### 将备份的数据恢复到另一张SD卡中

这时候，需要PC自带读卡器，或者外置读卡器

fdisk，mkfs格式化SD卡后，同样使用 `dd` 系统还原

将gzip压缩的文件还原到SD中，(可在PC，Ubuntu下操作）
``` bash
sudo gizp -dc /mnt/psda1/xxx.img.gz | sudo dd of=/dev/sda
```

### 动态调整分区大小

[ 使用fdisk e2fsck resize2fs调整Linux分区大小](http://blog.csdn.net/azure190/article/details/51044743)

用fdisk 先删除原有分区， 再重建分区， 起始cylinder 绝对不可以改，这样会破坏原分区的数据。

分区建好后， 就可以用e2fsck 先检查一下分区， 再用resize2fs 扩大就可以了。

``` bash
e2fsck -f /dev/sdd2
resize2fs /dev/sdd2
```

### 终极备份还原

https://item.congci.com/-/content/shu-mei-pai-raspberry-pi-sd-ka-xitong-jingxiang-beifen-haiyuan
http://www.tyrantek.com/archives/508/
