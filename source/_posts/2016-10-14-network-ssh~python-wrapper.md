title: network-ssh~python-wrapper
date: 2016-10-14 14:54:07
category: [Network, SSH]
tags:
  - SSH
  - python
  - wrapper
description: 使用python做一个ssh的马甲，方便快速的传输文件，如果能包含剪贴板就更好了
---
[Toc]


### paramiko 模块

paramiko支持python 2和python 3
Paramiko itself is a pure Python interface around SSH networking concepts.

paramiko是用python语言写的一个模块，遵循SSH2协议，支持以加密和认证的方式，进行远程服务器的连接。
paramiko支持Linux, Solaris, BSD, MacOS X, Windows等平台通过SSH从一个平台连接到另外一个平台。
利用该模块，可以方便的进行ssh连接和sftp协议进行sftp文件传输。

<!-- more -->

#### Installation 

Windows下，pip安装
```
pip install paramiko
```

**NOTE: PyCrypto模块是否需要安装？， cryptography在安装paramiko时会自动依赖安装**

### 参考文档

1. [官方文档paramiko](http://www.paramiko.org/)
2. [python模块paramiko与ssh](http://www.361way.com/python-paramiko-ssh/3984.html)
3. [官方文档cryptography](https://cryptography.io/en/latest/)
