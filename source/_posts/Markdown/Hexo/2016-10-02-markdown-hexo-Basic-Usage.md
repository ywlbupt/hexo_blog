title: markdown-hexo~Basic-Usage
date: 2016-10-02 15:18:23
category: [Markdown, Hexo]
tags:
  - tutorial
description:
-------
[Toc]

1. [hexo官网](https://hexo.io/zh-cn/)

[官网]: https://hexo.io/zh-cn/
[node.js]: wiz://open_document?guid=191fd422-472f-47cf-81c5-5d16e5b1a900&kbguid=&private_kbguid=26cdc8d6-8c7b-46e1-858b-50f63a1c8d67

## Windows下安装

参考[官网]，在安装好[node.js]与`git`之后，在命令行，一键配置即可：
```bash
npm install hexo-cli -g
```

### git clone克隆Hexo的主题Next

在终端窗口下，定位到 Hexo 站点目录下：
```bash
$ cd your-hexo-site
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
$ git clone git@github.com:ywlbupt/Hexo-theme-light_cn.git themes/light_cn
```

### Hexo 站点部署

```bash
$ hexo g #生成静态页面
$ hexo s #开启本地站点Server，（默认端口4000，'ctrl + c'关闭server）
$ hexo d #在_config.yml中配置push至github远程仓库gh-pages分支
```

### Issue: `Local hexo not found in D:\hexo_blog`

1. 背景：
  Hexo搭建博客之后用git已经将所有的source都同步到了git上，在另一台电脑上将源代码clone下来之后，直接执行 hexo server,出现错误
    ```
    ERROR Local hexo not found in D:\hexo_blog
    ERROR Try running: 'npm install hexo --save'
    ```
2. 解决方案：
    ```
    cd D:\hexo_blog
    npm install
    hexo server
    ```
3. 原因 ：
  `.gitignore`文件里面忽略了`node_modules`文件夹，所以这个文件夹没有更新上去。所以用`npm`重新安装即可。

## Basic Usage

### 模板(Scaffold)

在新建文章时，Hexo 会根据 scaffolds 文件夹内相对应的文件来建立文件，例如：

```bash
$ hexo new photo "My Gallery"
```

在执行这行指令时，Hexo 会尝试在 scaffolds 文件夹中寻找 `photo.md`，并根据其内容建立文章，以下是您可以在模版中使用的变量：

* layout  布局
* title   标题
* date  文件建立日期

### 布局(Layout)

Hexo 有三种默认布局：`post`、`page` 和 `draft`，它们分别对应不同的路径，而您自定义的其他布局和 `post` 相同，都将储存到 `source/_posts` 文件夹。
布局 	路径
```
post   source/_posts
page   source
draft   source/_drafts
```
新建草稿
```
:!hexo n[ew] draft title
```

### 自定义 IP

服务器默认运行在 `0.0.0.0`，您可以覆盖默认的 IP 设置，如下：

```bash
$ hexo server -i 192.168.1.1
```


