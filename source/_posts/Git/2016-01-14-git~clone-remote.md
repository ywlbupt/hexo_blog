title: git clone 远程仓库的切换
date: 2016-01-14 15:11:16
tags: git
description: 
----
## clone 某特定分支

    git clone -b <branch> <remote_repo>

或者：

git clone只能clone远程库的master分支，无法clone所有分支，解决办法如下：

<!-- more -->

1. 找一个干净目录，假设是git_work
2. `cd git_work`
3. `git clone https://github.com/ywlbupt/hexo_blog.git` ,这样在git_work目录下得到一个`hexo_blog`子目录
4. cd hexo_blog
5. `git branch -a`，列出所有分支名称如下：

        remotes/origin/dev
        remotes/origin/release
6. `git checkout -b dev origin/dev`，作用是checkout远程的dev分支，在本地起名为dev分支，并切换到本地的dev分支
7. `git checkout -b release origin/release`，作用参见上一步解释
8. `git checkout dev`，切换回dev分支，并开始开发。

## git fetch 远程仓库至本地，并重新命名

    git fetch origin master:tmp
    git diff tmp 
    git merge tmp
