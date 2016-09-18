# WiZero 阅读，思考，实践，不忘初心 #
[Toc]

> version : 1.0  
> Last_update: 2016-09-16  

## TODO ##

TODO:
* python脚本，makefile自动创建目录页面
* 遵循一定的格式去写，就算是自己的[格式](#Regulation)也好，方便日后的文本处理
* Snipppets的使用，减少重复劳动

## 约定成俗的规范 ##
<span id='Regulation'>格式规范</span>

1. 页面内的跳转，锚处不要放在与标题一栏，放在挨着标题的下一行。
2. [Toc]，目录的创建，挨着yaml下一行放置。

## Hexo Usage

### 标签与分类 ###

只有文章支持分类和标签，您可以在 Front-matter 中设置。  
在其他系统中，分类和标签听起来很接近，但是在 Hexo 中两者有着明显的差别：
* 分类具有顺序性和层次性，也就是说 Foo, Bar 不等于 Bar, Foo；
* 而标签没有顺序和层次。
```yaml
categories:
    - Diary
tags:
    - PS3
    - Games
```

### 模板配置 ###

`yaml`头的模板设置在`Hexo`下的`./scaffolds/post.md`中
```yaml
title: {{ title }}
date: {{ date }}
tags:
description:
---
```
以`---`为分割线，下面就是markdown笔记的内容

### Hexo 站点部署 ###

```bash
$ hexo g #生成静态页面
$ hexo s #开启本地站点Server，（默认端口4000，'ctrl + c'关闭server）
$ hexo d #在_config.yml中配置push至github远程仓库gh-pages分支
```
gh-pages分支在`_config.yml`中的配置如下

``` bash
# Deployment
## Docs: http://hexo.io/docs/deployment.html
deploy:
  type: git
  message: "update"
  repository: git@github.com:ywlbupt/hexo_blog.git
  # branch: master
  branch: gh-pages
```

### new 发布新文章 ###

新建一篇文章，如果没有给定layout，则使用`_config.yml`中的`default_layout`参数来代替，如果标题有空格，请使用括号括起来
```bash
$ hexo new [layout] <title>
```
