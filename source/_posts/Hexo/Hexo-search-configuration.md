title: Hexo-search-configuration
date: 2016-01-11 00:50:37
tags:
---

## 为hexo静态html页面添加站内搜索

**For theme Next**

使用 Swiftype 之前需要前往 Swiftype 配置一个搜索引擎。

然后，编辑 站点配置文件，新增 swiftype_key 字段，值为你的 swiftype 搜索引擎的 key。

首先你要去swiftype的网站https://swiftype.com注册一个账号，然后根据指引建立好自己网站对应的索引，
不得不说swiftype很棒，只要输入一个网址它便会自动的抓取这个站点的所有页面，并自动添加关键字。

在站点的 _config.yml中增加
```
swiftype_key: your-swiftype-key
```

在官网后台控制面板中选择 install，可看到自己的 swiftype_key：  
![swiftpe_key](images/Hexo/swiftype-key.png)




## Reference Link
1. [5 分钟快速安装](http://theme-next.iissnan.com/five-minutes-setup.html)
2. [Swiftype站内搜索](https://github.com/iissnan/hexo-theme-next/wiki/Swiftype站内搜索)
