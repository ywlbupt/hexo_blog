title: Hexo基本用法
date: 2016-01-11 01:49:05
tags: Hexo
---

## Hexo常用命令

试一试markdow代码的格式

    hexo new "postName" #新建文章
    hexo new page "pageName" #新建页面
    hexo generate #生成静态页面至public目录
    hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
    hexo deploy #将.deploy目录部署到GitHub

<!-- more -->

### Reference Link

## 为hexo静态html页面添加站内搜索

**For theme Next**

使用 Swiftype 之前需要前往 Swiftype 配置一个搜索引擎。

然后，编辑 站点配置文件，新增 swiftype_key 字段，值为你的 swiftype 搜索引擎的 key。

首先你要去swiftype的网站https://swiftype.com注册一个账号，然后根据指引建立好自己网站对应的索引，
不得不说swiftype很棒，只要输入一个网址它便会自动的抓取这个站点的所有页面，并自动添加关键字。

在站点的 _config.yml中增加

    swiftype_key: your-swiftype-key

在官网后台控制面板中选择 install，可看到自己的 swiftype_key：  
![swiftpe_key](images/Hexo/swiftype-key.png)

### Reference Link
1. [5 分钟快速安装](http://theme-next.iissnan.com/five-minutes-setup.html)
2. [Swiftype站内搜索](https://github.com/iissnan/hexo-theme-next/wiki/Swiftype站内搜索)


## 在hexo中嵌入图片、音乐、视频

实际上，在hexo中，markdown支持html标签，md文件解析为html时原有的html部分会保留。有基于此，我们只要在文中插入符合html规范的代码即可。下面举例说明。


### 图片

代码：

    ![](images/Hexo/xxx.jpg)


### 音乐

以『虾米音乐』为例，歌曲页面有个『转帖』选项，将html代码或javascript代码复制到文中即可。

代码：

    <embed src="http://www.xiami.com/widget/0_3515679/singlePlayer.swf" type="application/x-shockwave-flash" width="257" height="33" wmode="transparent"></embed>

### 视频

嵌入视频的方法和音乐类似，视频网站每个视频页面都会有一个『分享』或『转帖』按钮，点击可以查看代码。

    <iframe height=498 width=510 src="http://player.youku.com/embed/XMjI2MjU3MDMy" frameborder=0 allowfullscreen></iframe>

### 万能

对于有些音乐、视频找不到『转帖』按钮的，可以查看源代码，找到相应的代码块贴在文中。若找不到，说明该文件的确不能放在自己文中了。

### Reference Link
1. [怎样在博文中嵌入图片、音乐、视频？](http://zipperary.com/2013/06/27/media-on-hexo/)



## Reference Link
1. [基本用法(1)](http://www.ituring.com.cn/article/198930)
2. [基本用法(2)](http://www.ituring.com.cn/article/199035)
