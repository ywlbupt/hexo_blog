title: git常见问题
date: 2016-01-15 00:05:09
tags: git
description: git常见问题的解决方法
---

### ubuntu下git status的中文乱码解决

只要一行就行了

    git config --global core.quotepath false

对比效果图
![git-status-chinese-messy](images/Git/git-status-chinese-messy.jpg)
