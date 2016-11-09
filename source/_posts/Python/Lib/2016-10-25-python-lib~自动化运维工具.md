title: python-lib~自动化运维工具
date: 2016-10-25 10:48:19
category: [Python, Lib]
tags:
  - ansible
  
description:
---
[Toc]

目前市场上主流的开源自动化配置管理工具有puppet、chef、ansible、saltstack。到底选择哪一个好？
我为什么选择Saltstack，考虑方式很简单，主要基于如下两个方面：
第一、语言的选择（puppet/chef vs ansible/saltstack）
开源技术，不看官网不足以熟练，不懂源码不足以精通
Puppet、Chef基于Ruby开发，ansible、saltstack基于python开发的
本人热衷于python（后期可做二次开发），排除Puppet、Chef
第二、速度的选择 (ansible vs saltstack)
管理配置讲究的是更快更稳
ansible基于SSH协议传输数据，Saltstack使用消息队列zeroMQ传输数据

### Install

```bash
pip install ansible
```

### rsync文件同步模块

[详解rsync算法--如何减少同步文件时的网络传输量](http://taohui.org.cn/rsync_algorithm.html)

* md5，哈希算法，把任意长度额字符串转化为固定的128字符，
  * 相同的字符串不可能计算出不同的MD5值
  * MD5碰撞是有可能的：两个不同的字符串有可能计算出相同的MD5值，但几率非常小，可以忽略不计
  * rsync中，同一个文件按1k分割成多块，计算每一个块的MD5值。
* alder32校验和，把1K块数据映射成32位整型数字
  * 相同的字符串不可能计算出不同的Alder32
  * 计算速度较MD5快得多，成本小；
  * 当有了从0-1024长度的校验和后，计算出1-1025或者2-1026等其他校验和非常方便，只要少量运算即可
  * 碰撞率比MD5高多了

### Ubuntu Install rsync

[Linux命令大全-rsync命令](http://man.linuxde.net/rsync)

```bash
sudo apt-get install rsync
```
但是需要注意的是必须在服务器A和B上都安装rsync，
其中A服务器上是以服务器模式运行rsync，
而B上则以客户端方式运行rsync。
这样在web服务器A上运行rsync守护进程，在B上定时运行客户程序来备份web服务器A上需要备份的内容。

#### 语法

```bash
rsync [OPTION]... SRC DEST 
rsync [OPTION]... SRC [USER@]host:DEST 
rsync [OPTION]... [USER@]HOST:SRC DEST 
rsync [OPTION]... [USER@]HOST::SRC DEST 
rsync [OPTION]... SRC [USER@]HOST::DEST 
rsync [OPTION]...  rsync://[USER@]HOST[:PORT]/SRC [DEST] 
```

对应于以上六种命令格式，rsync有六种不同的工作模式：
1. 拷贝本地文件。当`SRC`和`DES`路径信息都不包含有单个冒号":"分隔符时就启动这种工作模式。如：`rsync -a /data /backup` 
2. 使用一个远程shell程序(如`rsh`、`ssh`)来实现将本地机器的内容拷贝到远程机器。当DST路径地址包含单个冒号`":"`分隔符时启动该模式。如：`rsync -avz *.c foo:src` 
3. 使用一个远程shell程序(如`rsh`、`ssh`)来实现将远程机器的内容拷贝到本地机器。当SRC地址路径包含单个冒号`":"`分隔符时启动该模式。如：`rsync -avz foo:src/bar /data` 
4. 从远程rsync服务器中拷贝文件到本地机。当`SRC`路径信息包含`"::"`分隔符时启动该模式。如：`rsync -av root@192.168.78.192::www /databack` 从本地机器拷贝文件到远程rsync服务器中。
5. 当`DST`路径信息包含`"::"`分隔符时启动该模式。如：`rsync -av /databack root@192.168.78.192::www` 
6. 列远程机的文件列表。这类似于rsync传输，不过只要在命令中省略掉本地机信息即可。如：`rsync -v rsync://192.168.78.192/www`
  
* `-a` 归档模式，表示以递归方式传输文件，并保持所有文件属性，等于-rlptgoD
* `-z` 在传输文件时做压缩处理
* `-v` 详细模式输出，信息回显

#### Section Name
-v, --verbose 详细模式输出
-q, --quiet 精简输出模式
-c, --checksum 打开校验开关，强制对文件传输进行校验
**-a, --archive 归档模式，表示以递归方式传输文件，并保持所有文件属性，等于-rlptgoD**
-r, --recursive 对子目录以递归模式处理
-R, --relative 使用相对路径信息
-b, --backup 创建备份，也就是对于目的已经存在有同样的文件名时，将老的文件重新命名为~filename
可以使用--suffix选项来指定不同的备份文件前缀
--backup-dir 将备份文件(如~filename)存放在在目录下
-suffix=SUFFIX 定义备份文件前缀
**-u, --update 仅仅进行更新，也就是跳过所有已经存在于DST，并且文件时间晚于要备份的文件，不覆盖更新的文件**
-l, --links 保留软链结
-L, --copy-links 想对待常规文件一样处理软链结
--copy-unsafe-links 仅仅拷贝指向SRC路径目录树以外的链结
--safe-links 忽略指向SRC路径目录树以外的链结
-H, --hard-links 保留硬链结
-p, --perms 保持文件权限
-o, --owner 保持文件属主信息
-g, --group 保持文件属组信息
-D, --devices 保持设备文件信息
-t, --times 保持文件时间信息
-S, --sparse 对稀疏文件进行特殊处理以节省DST的空间
-n, --dry-run现实哪些文件将被传输
-w, --whole-file 拷贝文件，不进行增量检测
-x, --one-file-system 不要跨越文件系统边界
-B, --block-size=SIZE 检验算法使用的块尺寸，默认是700字节
**-e, --rsh=command 指定使用rsh、ssh方式进行数据同步`-e ssh`**
--rsync-path=PATH 指定远程服务器上的rsync命令所在路径信息
-C, --cvs-exclude 使用和CVS一样的方法自动忽略文件，用来排除那些不希望传输的文件
**--existing 仅仅更新那些已经存在于DST的文件，而不备份那些新创建的文件**
**--delete 删除那些DST中SRC没有的文件**
--delete-excluded 同样删除接收端那些被该选项指定排除的文件
--delete-after 传输结束以后再删除
--ignore-errors 及时出现IO错误也进行删除
--max-delete=NUM 最多删除NUM个文件
--partial 保留那些因故没有完全传输的文件，以是加快随后的再次传输
--force 强制删除目录，即使不为空
--numeric-ids 不将数字的用户和组id匹配为用户名和组名
--timeout=time ip超时时间，单位为秒
-I, --ignore-times 不跳过那些有同样的时间和长度的文件
--size-only 当决定是否要备份文件时，仅仅察看文件大小而不考虑文件时间
--modify-window=NUM 决定文件是否时间相同时使用的时间戳窗口，默认为0
-T --temp-dir=DIR 在DIR中创建临时文件
--compare-dest=DIR 同样比较DIR中的文件来决定是否需要备份
-P 等同于 --partial
--progress 显示备份过程
**-z, --compress 对备份的文件在传输时进行压缩处理**
--exclude=PATTERN 指定排除不需要传输的文件模式
--include=PATTERN 指定不排除而需要传输的文件模式
--exclude-from=FILE 排除FILE中指定模式的文件
--include-from=FILE 不排除FILE指定模式匹配的文件
--version 打印版本信息
--address 绑定到特定的地址
--config=FILE 指定其他的配置文件，不使用默认的rsyncd.conf文件
--port=PORT 指定其他的rsync服务端口
--blocking-io 对远程shell使用阻塞IO
-stats 给出某些文件的传输状态
--progress 在传输时现实传输过程
--log-format=formAT 指定日志文件格式
--password-file=FILE 从FILE中得到密码
--bwlimit=KBPS 限制I/O带宽，KBytes per second
-h, --help 显示帮助信息
