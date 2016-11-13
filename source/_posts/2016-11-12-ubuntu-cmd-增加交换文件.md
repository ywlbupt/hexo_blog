title: ubuntu-cmd~增加交换文件
date: 2016-11-12 00:29:53
category: [Ubuntu, Cmd]
tags:
description:
---
[Toc]

[How To Add Swap on Ubuntu 12.04](https://www.digitalocean.com/community/tutorials/how-to-add-swap-on-ubuntu-12-04)

```bash
pi@raspberrypi:~/Vim_ywl/vimfiles/bundle/YouCompleteMe $ sudo swapon -s
Filename                                Type            Size    Used    Priority
/var/swap                               file            102396  56240   -1
pi@raspberrypi:~/Vim_ywl/vimfiles/bundle/YouCompleteMe $ df
Filesystem     1K-blocks    Used Available Use% Mounted on
/dev/root       15180208 5521064   8982068  39% /
devtmpfs          469536       0    469536   0% /dev
tmpfs             473868       0    473868   0% /dev/shm
tmpfs             473868    6712    467156   2% /run
tmpfs               5120       4      5116   1% /run/lock
tmpfs             473868       0    473868   0% /sys/fs/cgroup
/dev/mmcblk0p1     64456   21256     43200  33% /boot
tmpfs              94776       0     94776   0% /run/user/1000
pi@raspberrypi:~/Vim_ywl/vimfiles/bundle/YouCompleteMe $ cd ~
pi@raspberrypi:~ $ sudo dd if=/dev/zero of=/swapfile bs=1024 count=524288
524288+0 records in
524288+0 records out
536870912 bytes (537 MB) copied, 37.013 s, 14.5 MB/s
pi@raspberrypi:~ $ sudo mkdwap /swapfile
sudo: mkdwap: command not found
pi@raspberrypi:~ $ sudo mkswap /swapfile                                        Setting up swapspace version 1, size = 524284 KiB
no label, UUID=5d28b25a-9422-457a-9276-0d3b2be44cab
pi@raspberrypi:~ $ sudo swapon /swapfile
swapon: /swapfile: insecure permissions 0644, 0600 suggested.
pi@raspberrypi:~ $ swapon -s
Filename                                Type            Size    Used    Priority/var/swap                               file            102396  54988   -1
/swapfile                               file            524284  0       -2
pi@raspberrypi:~ $ sudo vim /etc/fstab
pi@raspberrypi:~ $ echo 10 | sudo tee /proc/sys/vm/swappiness
10
pi@raspberrypi:~ $ echo vm.swappiness = 10 | sudo tee -a /etc/sysctl.conf
vm.swappiness = 10
```

Note: 为了避免出现意外的情况，如下所示使用swapoff命令关闭它，仅在需要使用时，使用步骤5所示的swapon命令，重新启用交换文件。

```bash
$ swapoff /swap_file
```

启用交换文件
```bash
$ swapon /swap_file
```
