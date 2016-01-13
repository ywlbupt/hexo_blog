title: Hexo-Begin-install
date: 2016-01-10 12:45:13
tags: Hexo
description: Hexo环境的搭建
---

## 安装git

comleted

## 安装 node.js

Ubuntu 14.04自带的Node.js版本太老，且我试了下安装并不成功，所以需要添加Node.js PPA安装最新版的Node.js，在终端中执行：
```
sudo add-apt-repository ppa:chris-lea/node.js
sudo apt-get update
sudo apt-get install nodejs
```

安装完成后执行node -v命令，看看Node.js是否安装成功。
```
ywl@ywl-ThinkPad-E420:~/blog_lin$ npm -v
1.4.28
```
一旦Node.js的新版本发布了，你可以直接从包管理器升级，无需从头再次编译安装。

** 在 Windows 环境下安装 Node.js 非常简单，仅须下载安装文件并执行即可完成安装。 **

##  安装 hexo

```
sudo npm install -g hexo
```

安装完成后，在你喜爱的文件夹下（如~/hexo_blog），执行以下指令，Hexo 即会自动在目标文件夹建立网站所需要的所有文件。
```
ywl@ywl-ThinkPad-E420:~/hexo_blog$ hexo init
INFO  Copying data to ~/hexo_blog
INFO  You are almost done! Don't forget to run 'npm install' before you start blogging with Hexo!
```

安装依赖包
```
sudo npm install
```

本地查看
现在我们已经搭建起本地的hexo博客了，执行以下命令，然后到浏览器输入[(localhost:4000)]看看。
```
hexo generate
hexo server
```

好了，至此，本地博客已经搭建起来了，只是本地.
```
hexo init
npm install hexo --save
hexo g
npm install
npm install hexo-deployer-git --save
hexo g
hexo d
_config.yml

deploy:
  type: git
  message: "update"
  repo: 
    github: git@github.com:wanggnim/Gnim.git
branch: gh-pages
```


## 使用 NexT 主题

见参考链接2

## Reference Link
1. [hexo系列教程：（二）搭建hexo博客](http://zipperary.com/2013/05/28/hexo-guide-2/)
2. [NextT主题 5 分钟快速安装](http://theme-next.iissnan.com/five-minutes-setup.html)
