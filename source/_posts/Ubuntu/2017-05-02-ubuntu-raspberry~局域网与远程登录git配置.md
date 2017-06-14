title: ubuntu-raspberry~局域网与远程登录git配置
date: 2017-05-02 22:36:22
category:
tags:
description:
---
[Toc]

###远程操作

```
$ git remote -v                   #查看远程版本库信息
$ git remote show <remote>        #查看指定远程版本库信息
$ git remote add <remote> <url>   #添加远程版本库
$ git fetch <remote>              #从远程库获取代码
$ git pull <remote> <branch>      #下载代码及快速合并
$ git push <remote> <branch>      #上传代码及快速合并
$ git push <remote> :<branch/tag-name>  #删除远程分支或标签
$ git push --tags                       #上传所有标签
$ git push origin experimental:experiment-by-bob  # 如果这个分支在远程仓库里对应不同的名称（如:experiment-by-bob）,可以这么做： 
```

### 局域网

#### 如果是在局域网配置
```
git remote add pi ssh://git@lopb3:34570/home/git/Vim_ywl.git 
```
* `lopb3`为pi在局域网中的IP地址，已经写在hosts中
* `34570`为PI的SSH端口，直接使用22端口会在某些场合下有问题，这个配置在PI中实现

#### 如果是在外网配置

参考公司电脑，通过putty登录，花生壳等等
```
git remote add pi ssh://git@ywlbupt.nat123.net:15842/home/git/Vim_ywl.git
```
