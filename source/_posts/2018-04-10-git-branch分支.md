title: git~branch分支
date: 2018-04-10 20:57:18
category: [Git]
tags: 
description:
---
[Toc]

万变不离其中 `git branch --help`

### 查看分支 -r|-a

* git branch 查看本地分支
* git branch -r 查看远程分支
* git branch -a 查看远程和本地分支

```bash
m@m-PC MINGW64 ~/hexo_blog (master)
$ git branch -a
  gh-pages
* master
  remotes/origin/HEAD -> origin/gh-pages
  remotes/origin/gh-pages
  remotes/origin/master
  remotes/pi/master
```

### 新建分支
* `git branch [-f] <branchname>`新建分支
* `git checkout -b <branchname>`新建并切换至新分支

### 删除[远程]分支

* `git branch -d <branchname>` 删除本地分支
* `git branch -d -r <branchname>`删除远程分支
    * 删除远程分支后还要推送到服务器上去 `git push origin :<branchname>`

`-D`相当于强制删除

### 重命名分支

* `git branch -m <oldchname> <newbranch>`

如果需要重命名远程分支，正确的做法是删除远程分支，然后将本地分支push到服务器

### git 切换分支的时候 是否需要提交当前修改

官方文档有句话“切换分支的时候最好保持一个清洁的工作区域。
![](/hexo_blog/images/Git/git-staged.png)

有如下几种处理方式：
1. add并且commit，再checkout，提交到当前分支
2. add但不commit，可以stash，然后checkout回来之后stash apply，在commit，提交到当前分支

其背后的原因：一个本地的git repo只有一个工作区和暂存区，但是有多个分支的提交区，而我们的checkout只是将HEAD指针从一个分支切换到另一个分支。

具体参考 git-stashing
