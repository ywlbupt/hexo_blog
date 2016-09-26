title: python-http
date: 2016-01-16 16:18:05
tags: python
description:
---

## [Python][Spider][urllib]HTTP-urlib
@(3.09 - Python)[python, urllib, spider]

注:  
>   python 3.x中urllib库和urilib2库合并成了urllib库。
    其中urllib2.urlopen()变成了`urllib.request.urlopen()`
    urllib2.Request()变成了`urllib.request.Request()`

    from urllib import request

### POST和GET请求

HTTP定义了与服务器交互的不同方法，最基本的方法有4种，分别是**GET，POST，PUT，DELETE**。

一个URL地址，它用于描述一个网络上的资源，而HTTP中的GET，POST，PUT，DELETE就对应着对这个资源的查，改，增，删4个操作。

其中，GET一般用于获取/查询资源信息，而POST一般用于更新资源信息，通常用于我们需要向服务器提交表单的情况。

比如说，我们在百度首页输入“宁哥的小站”，回车，然后地址栏会跳转到搜索结果的列表页。同时可以看到浏览器的地址栏会发生改变，类似于`http://www.baidu.com/s?wd=xxx`的形式。就是说，地址栏从百度首页`http://www.baidu.com/`跳转到了`http://www.baidu.com/s?wd=xxx`  
变化就在于，在最初的url后面会附加相关的字段，**以？分割url和请求的数据**，这些数据就是你要查询字段的编码。而这个过程，就是典型的GET请求的情况。

与之对应的POST请求则显得深藏不露，它在于你必须通过浏览器输入或提交一些服务器需要的数据，才能给你返回完整的界面，这点其实与GET请求情况有相通之处，但是**这个过程浏览器的地址栏是不会发生跳转的**。

POST请求提交的数据是如何传给服务器的呢？大家可以采用一些分析页面的手段来获取上传的数据。实际上，POST请求是将提交的数据放在HTTP包的包体中，这种方式无疑加强了数据的安全性，不像GET请求那样，用户可以通过跳转的url就可以查看出向服务器发送的数据。另外，POST请求除了提交数据外，还可以提交文件，这点也是GET请求做不到的。



### User-Agent

如何让网站们把我们的Python爬虫当成正规的浏览器来访. 因为如果不这么伪装自己, 有的网站就爬不回来了（有的网站专门拦截爬虫程序）. 如果看过理论方面的知识, 就知道我们是要在 GET 的时候将 User-Agent 添加到header里.

urllib提供的功能就是利用程序去执行各种HTTP请求。如果要模拟浏览器完成特定功能，需要把请求伪装成浏览器。伪装的方法是先监控浏览器发出的请求，再根据浏览器的请求头来伪装，User-Agent头就是用来标识浏览器的。

**Fiddler**

### Cookie 

有些网站需要登录后才能访问某个页面，在登录之前，你想抓取某个页面内容是不允许的。那么我们可以保存我们登录的Cookie，然后再抓取其他页面就达到目的了。

Cookie是HTTP消息头中的一种属性，包括：
Cookie名字（Name）Cookie的值（Value），Cookie的过期时间（Expires/Max-Age），Cookie作用路径（Path），Cookie所在域名（Domain），使用Cookie进行安全连接（Secure）。 前两个参数是Cookie应用的必要条件，另外，还包括Cookie大小（Size，不同浏览器对Cookie个数及大小限制是有差异的）。

有了Cookie，我们就可以实现自动登陆网站，甚至能够解决一些验证码登陆的情况。

在Python中，提供了cookielib这个模块，能够将cookie保存到本地，也可以保存在内存中。在第一次用用户名密码登陆网站之后，我们只需要如下创建opener对象，就可以记录cookie：

这样，接下来我们如果继续访问该域名的网站，直接采用`opener.open(需要输入的url)`，就能实现用保存的Cookie直接打开。

本来处理 Cookies 是个麻烦的事情, 不过 Python 的 http.cookiejar 库给了我们很方便的解决方案, 只要在创建 opener 的时候将一个 HTTPCookieProcessor 放进去, Cookies 的事情就不用我们管了

``` python
import http.cookiejar
import urllib.request
def getOpener(head):
    # deal with the Cookies
    cj = http.cookiejar.CookieJar()
    pro = urllib.request.HTTPCookieProcessor(cj)
    opener = urllib.request.build_opener(pro)
    header = []
    for key, value in head.items():
        elem = (key, value)
        header.append(elem)
    opener.addheaders = header
    return opener
```
getOpener 函数接收一个 head 参数, 这个参数是一个字典. 函数把字典转换成元组集合, 放进 opener. 这样我们建立的这个 opener 就有两大功能:
* 自动处理使用 opener 过程中遇到的 Cookies
* 自动在发出的 GET 或者 POST 请求中加上自定义的 Header

### Proxy 代理 ProxyHandler

示例代码
```
proxy_handler = urllib.request.ProxyHandler({'http': 'http://www.example.com:3128/'})
proxy_auth_handler = urllib.request.ProxyBasicAuthHandler()
proxy_auth_handler.add_password('realm', 'host', 'username', 'password')
opener = urllib.request.build_opener(proxy_handler, proxy_auth_handler)
with opener.open('http://www.example.com/login.html') as f:
    pass
```


### 模拟浏览器 POST GET 方式
POST方式：

```
import urllib.parse  
import urllib.request  
 
url="http://liuxin-blog.appspot.com/messageboard/add" 

values={"content":"命令行发出网页请求测试"}  
data=urllib.parse.urlencode(values) 

# 创建请求对象  
req=urllib.request.Request(url,data) 
#获得服务器返回的数据  
response=urllib.request.urlopen(req) 
#处理数据  
page=response.read()  
```

GET方式：

```
import urllib.parse  
import urllib.request  

url="http://www.google.cn/webhp" 

values={"rls":"ig"}  
data=urllib.parse.urlencode(values)  

theurl=url+"?"+data 
#创建请求对象  
req=urllib.request.Request(theurl) 
#获得服务器返回的数据  
response=urllib.request.urlopen(req) 
#处理数据  
page=response.read()  
```

### urllib库

urllib.request module uses **HTTP/1.1** and includes `Connection:close` header in its HTTP requests.

对于Response对象

-   geturl() — return the URL of the resource retrieved, commonly used to determine if a redirect was followed
-   info() — return the meta-information of the page, such as headers, in the form of an email.message_from_string() instance (see Quick Reference to HTTP Headers)
-   getcode() – return the HTTP status code of the response.

geturl()的设置是为了辨别是否有服务器端的网址重定向,而info()[getheaders()]则包含了一系列的信息。
1. info()
2. gerheaders()
3. f.status
4. f.reason

#### urllib.request.urlopen

使用urllib.request库中的`urlopen(url)`函数来打开网页，函数参数url只能接受utf-8编码方式(因为Python脚本文件用utf-8编码)

urlopen一般接受三个参数，它的参数如下：`urlopen(url, data, timeout)`
> 第二三个参数是可以不传送的，data默认为空 None，timeout默认为 `socket._GLOBAL_DEFAULT_TIMEOUT`

> 第一个参数URL是必须要传送的，url参数可以是一个字符串或者是一个Request请求。The URL url, which can be either a string or a Request object.
> 执行urlopen方法之后，返回一个**response对象**，返回信息便保存在这里面。

> the HTTP request will be a **POST** instead of a **GET** when the data parameter is provided.

``` python
from urllib import request
url = "https://api.douban.com/v2/book/2129650"
Urequest = request.Request(url)
with request.urlopen(Urequest, timeout=5) as f:
    print(f.read().decode('utf-8'))
```

#### urllib.request.Request

`urllib.request.Request((url, data=None, headers={}, origin_req_host=None, unverifiable=False, method=None))`
-   `urllib.parse.urlencode()`
    data should be a buffer in the standard application/x-www-form-urlencoded format. The urllib.parse.urlencode() function takes a mapping or sequence of 2-tuples and returns an ASCII string in this format. **It should be encoded to bytes before being used as the data parameter.**
如果要以POST发送一个请求，只需要把参数data以bytes形式传入。POST的表单数据用`urllib.parse.urlencode()`来格式化，该函数接收2-tuple数据，返回ASCII的字符串
    POST data should be bytes or an iterable of bytes. It cannot be of type str。urlencode()返回的是ASCII的字符串，所以在引用的时候应该encode成Bytes类型的。

-   `add_header()`
    给HTTP POST/GET添加请求报头，常用来伪装成浏览器
    *For example, Mozilla Firefox may identify itself as "Mozilla/5.0 (X11; U; Linux i686) Gecko/20071127 Firefox/2.0.0.11", while urllib‘s default user agent string is "Python-urllib/2.6" (on Python 2.6).*

##### urllib.request.(urlopen,Request)示例代码
``` python
from urllib import request, parse

print('Login to weibo.cn...')
email = input('Email: ')
passwd = input('Password: ')
login_data = parse.urlencode([
    ('username', email),
    ('password', passwd),
    ('entry', 'mweibo'),
    ('client_id', ''),
    ('savestate', '1'),
    ('ec', ''),
    ('pagerefer', 'https://passport.weibo.cn/signin/welcome?entry=mweibo&r=http%3A%2F%2Fm.weibo.cn%2F')
])

req = request.Request('https://passport.weibo.cn/sso/login')
req.add_header('Origin', 'https://passport.weibo.cn')
req.add_header('User-Agent', 'Mozilla/6.0 (iPhone; CPU iPhone OS 8_0 like Mac OS X) AppleWebKit/536.26 (KHTML, like Gecko) Version/8.0 Mobile/10A5376e Safari/8536.25')
req.add_header('Referer', 'https://passport.weibo.cn/signin/login?entry=mweibo&res=wel&wm=3349&r=http%3A%2F%2Fm.weibo.cn%2F')

with request.urlopen(req, data=login_data.encode('utf-8')) as f:
    print('Status:', f.status, f.reason)
    for k, v in f.getheaders():
        print('%s: %s' % (k, v))
    print('Data:', f.read().decode('utf-8'))
```

### 常见用法

####  直接将URL保存为本地文件：

```
import urllib.request
url="http://www.xxxx.com/1.jpg"
urllib.request.urlretrieve(url,r"d:\temp\1.jpg")
```
#### 打印HTTP header

``` python
from urllib import request
url = "https://api.douban.com/v2/book/2129650"
Urequest = request.Request(url)
with request.urlopen(Urequest, timeout=5) as f:
    data = f.read()
    print ('Status:', f.status, f.reason)
    # print(f.read().decode('utf-8'))
    for k, v in f.getheaders() :
        print ("%s : %s" % (k, v))
    > print("Data: ", data.decode('utf-8'))
```

## Reference Link
1. [iurllib.request — Extensible library for opening URLs](https://docs.python.org/3.4/library/urllib.request.html?highlight=request#module-urllib.request)
2. [urllib-廖雪峰](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001432688314740a0aed473a39f47b09c8c7274c9ab6aee000)
