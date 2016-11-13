title: ubuntu-IDE~Node.js安装与配置
date: 2016-11-12 22:12:45
category: [Ubuntu, IDE]
tags:
description:
---
[Toc]

[官网]

Node.js是一个Javascript运行环境(runtime)。

> Node.js® is a JavaScript runtime`built on Chrome's V8 JavaScript engine`，Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient. 
> Node.js' package ecosystem, **npm**

[官网]: https://nodejs.org/en/

<!-- more -->

## node linux 下安装

直接安装二进制文件，上官网下载[下载地址](https://nodejs.org/dist/v6.9.1/)

参考：
```bash
$ wget https://nodejs.org/dist/v6.9.1/node-v6.9.1-linux-armv7l.tar.gz
$ tar -vxf node-v4.1.1-linux-x64.tar.gz ~# mv node-v4.1.1-linux-x64 nodejs 
$ vim /etc/profile.d/node.sh 

export NODE_HOME=~/nodejs 
export PATH=$PATH:$NODE_HOME/bin 
export NODE_PATH=$PATH:$NODE_HOME/lib/node_modules 

$ source /etc/profile.d/node.sh
```
### 将node运行目录写到sudo运行环境中
如果运行
```bash
sudo npm -v
```
发现，找不到npm命令，那我们需要把node的运行目录写到sudo运行环境，软链接

```bash
sudo ln -s /home/pi/nodejs/bin/node /usr/bin/node
sudo ln -s /home/pi/nodejs/lib/node /usr/lib/node
sudo ln -s /home/pi/nodejs/bin/npm /usr/bin/npm
sudo ln -s /home/pi/nodejs/bin/node-waf /usr/bin/node-waf
```

### npm镜像源修改

解决npm 的 `shasum check failed for`错误（npm注册国内镜像介绍）

镜像使用方法（三种办法任意一种都能解决问题，建议使用第三种，将配置写死，下次用的时候配置还在）:

1.  通过config命令
    ```bash
    npm config set registry http://registry.cnpmjs.org 
    npm info underscore （如果上面配置正确这个命令会有字符串response）
    ```
2.  命令行指定
    ```bash
    npm --registry http://registry.cnpmjs.org info underscore 
    ```
3.  编辑 `~/.npmrc` 加入下面内容
    ```bash
    registry = http://registry.cnpmjs.org
    ```

[搜索镜像链接](http://cnpmjs.org)

## Windows下安装

### 下载安装

[官网]下载对应的Windows版本，一个后缀是`msi`的文件，点击安装，采用默认的配置。

### `Node.js`环境测试，Test

* 编写测试文件test.js 这里我们先放在D盘根目录下
    ```js
    var http = require("http");
    http.createServer(function(request, response) {
    response.writeHead(200, {"Content-Type": "text/plain"});
    response.write("test nodjs");
    response.end();
    }).listen(8899);
    console.log("nodejs start listen 8899 port!");
    ```
* 设置好浏览器端口信息这里我们用`8899`
* 进入`node.js command prompt` 命令窗口,进入`test.js`目录，然后键入`node test.js`
  
* 然后浏览器打开`http://127.0.0.1:8899/`可以看到输出了`test nodjs`。环境安装配置成功
  
 

