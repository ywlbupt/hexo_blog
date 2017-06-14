title: Git~Git_SSH_Key生成与SSH无密码登录
date: 2016-12-03 11:00:19
category: [Git]
tags:
    - ssh
description:
---
[Toc]

## [SSH登录]Windows下Git SSH Key 生成步骤
 
* [git官方指导](https://help.github.com/articles/generating-an-ssh-key/)
 
git是*分布式的代码管理*工具，远程的代码管理是基于ssh的，所以要使用远程的git则需要ssh的配置。
 
github的ssh配置如下：
 
###  设置git的`user name`和`email`：
```
$ git config --global user.name "ywl"
$ git config --global user.email "ywlbupt@163.com"
```
###  生成SSH密钥过程： 

查看是否已经有了ssh密钥：

```bash
cd ~/.ssh
$ ls -al ~/.ssh 
# Lists the files in your .ssh directory, if they exist
```    

如果没有密钥则不会有此文件夹，有则备份删除 

###  生成密钥：
``` bash
$ ssh-keygen -t rsa -b 4096 -C "ywlbupt163.com"
```
按3个回车，密码为空。
 
``` bash
m@m-PC MINGW64 /c/Vim_ywl (master)
$ ssh-keygen -t rsa -b 4096 -C "ywlbupt163.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/m/.ssh/id_rsa):
Created directory '/c/Users/m/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/m/.ssh/id_rsa.
Your public key has been saved in /c/Users/m/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:jJ6we2YsHSKOswtM4ahWHb+t0giqPZfcp/f6NdCZQdI ywlbupt163.com
The key's randomart image is:
+---[RSA 4096]----+
|          ...    |
|           oE    |
| .   .      .    |
|o . . oo   . +   |
|.o .....S . +    |
|+ o..+..o  .     |
|o=.oo*=o .  o    |
|=oo =+B.+  . .   |
|=+.o.=o=.+o      |
+----[SHA256]-----+
```
最后得到了两个文件：id_rsa和id_rsa.pub

###  添加密钥到`ssh-agent`

ssh-add 文件名需要之前输入密码。

[全面-官方指导](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/#adding-your-ssh-key-to-the-ssh-agent)

* Ensure ssh-agent is enabled:
    ```bash
    m@m-PC MINGW64 /c/Vim_ywl (master)
    # start the ssh-agent in the background
    eval "$(ssh-agent -s)"
    Agent pid 5228
    ```
* Add your SSH key to the ssh-agent.
    ```bash
    $ ssh-add ~/.ssh/id_rsa
    ```
###  在github上添加ssh密钥

这要添加的是“id_rsa.pub”里面的公钥。 打开`https://github.com/` 登录，然后添加ssh。

``` bash
m@m-PC MINGW64 /c/Vim_ywl (master)
# 复制id_rsa.pub的内容至剪切板
$ clip < ~/.ssh/id_rsa.pub
```
测试：
 
``` bash
ssh git@github.com
The authenticity of host ‘github.com (207.97.227.239)’ can’t be established.
RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added ‘github.com,207.97.227.239′ (RSA) to the list of known hosts.
ERROR: Hi tekkub! You’ve successfully authenticated, but GitHub does not provide shell access
Connection to github.com closed.
```
###  测试SSH是否生效

**`ssh -T git@github.com`**

``` bash
m@m-PC MINGW64 /c/Vim_ywl (master)
$ ssh -T git@github.com
Hi ywlbupt! You've successfully authenticated, but GitHub does not provide shell
access.
```
 
###  将远程仓库的url从Http修改至SSH
 
The `git remote set-url` command changes an existing remote repository URL.
* HTTP url's Format
    `https://github.com/USERNAME/OTHERREPOSITORY.git`
* SSH url's Format
    `git@github.com:USERNAME/OTHERREPOSITORY.git`
 
``` bash
m@m-PC MINGW64 /c/Vim_ywl (master)
$ git remote -v
origin  https://github.com/ywlbupt/Vim_ywl.git (fetch)
origin  https://github.com/ywlbupt/Vim_ywl.git (push)
 
m@m-PC MINGW64 /c/Vim_ywl (master)
$ git remote set-url origin git@github.com:ywlbupt/Vim_ywl.git
 
m@m-PC MINGW64 /c/Vim_ywl (master)
$ git remote -v
origin  git@github.com:ywlbupt/Vim_ywl.git (fetch)
origin  git@github.com:ywlbupt/Vim_ywl.git (push)
 
m@m-PC MINGW64 /c/Vim_ywl (master)
$ git push origin master
```

## ssh 无密码登录设置

ssh 无密码登录要使用公钥与私钥。linux下可以用用ssh-keygen生成公钥/私钥对，下面我以CentOS为例。

有机器A(192.168.1.155)，B(192.168.1.181)。现想A通过ssh免密码登录到B。

### 在A机下生成公钥/私钥对。
``` bash
[chenlb@A ~]$ ssh-keygen -t rsa -P ''
```

-P表示密码，-P '' 就表示空密码，也可以不用-P参数，这样就要三车回车，用-P就一次回车。
它在/home/chenlb下生成.ssh目录，.ssh下有id_rsa和id_rsa.pub。

### 把A机下的`id_rsa.pub`复制到B机下

在B机的.ssh/authorized_keys文件里，我用scp复制。
``` bash
[chenlb@A ~]$ scp .ssh/id_rsa.pub chenlb@192.168.1.181:/home/chenlb/id_rsa.pub 
chenlb@192.168.1.181's password:
id_rsa.pub                                    100%  223     0.2KB/s   00:00
```

由于还没有免密码登录的，所以要输入密码。

### B机把从A机复制的id_rsa.pub添加到.ssh/authorized_keys文件里。
``` bash
[chenlb@B ~]$ cat id_rsa.pub >> .ssh/authorized_keys
[chenlb@B ~]$ chmod 600 .ssh/authorized_keys
```

`authorized_keys`的权限要是600。

### A机登录B机。

``` bash
[chenlb@A ~]$ ssh 192.168.1.181
The authenticity of host '192.168.1.181 (192.168.1.181)' can't be established.
RSA key fingerprint is 00:a6:a8:87:eb:c7:40:10:39:cc:a0:eb:50:d9:6a:5b.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.1.181' (RSA) to the list of known hosts.
Last login: Thu Jul  3 09:53:18 2008 from chenlb
[chenlb@B ~]$
```

第一次登录是时要你输入yes。

现在A机可以无密码登录B机了。

小结：登录的机子可有私钥，被登录的机子要有登录机子的公钥。这个公钥/私钥对一般在私钥宿主机产生。上面是用rsa算法的公钥/私钥对，当然也可以用dsa(对应的文件是id_dsa，id_dsa.pub)

想让A，B机无密码互登录，那B机以上面同样的方式配置即可。

### 可通过别名alias快捷登录B机
