title: python-requests-高级用法
date: 2016-01-17 15:41:37
tags: python
description:
----

## 会话对象 Session

会话对象让你能够跨请求保持某些参数。它也**会在同一个Session实例发出的所有请求之间保持cookies**。

会话对象具有主要的Requests API的所有方法。

我们来跨请求保持一些cookies:

```
    s = requests.Session()
    
    s.get('http://httpbin.org/cookies/set/sessioncookie/123456789')
    r = s.get("http://httpbin.org/cookies")
    
    print(r.text)
    # '{"cookies": {"sessioncookie": "123456789"}}'
```
会话也可用来为请求方法提供缺省数据。这是通过为会话对象的属性提供数据来实现的:

```
    s = requests.Session()
    s.auth = ('user', 'pass')
    s.headers.update({'x-test': 'true'})
    
    # both 'x-test' and 'x-test2' are sent
    s.get('http://httpbin.org/headers', headers={'x-test2': 'true'})
```

任何你传递给请求方法的字典都会与已设置会话层数据合并。方法层的参数覆盖会话的参数。

从字典参数中移除一个值
有时你会想省略字典参数中一些会话层的键。要做到这一点，你只需简单地在方法层参数中将那个键的值设置为 None ，那个键就会被自动省略掉。

包含在一个会话中的所有数据你都可以直接使用。

## 保持活动状态（持久连接）

归功于`urllib3`，同一会话内的持久连接是完全自动处理的！同一会话内你发出的任何请求都会自动复用恰当的连接！

注意：只有所有的响应体数据被读取完毕连接才会被释放为连接池；所以确保将 stream 设置为 False 或读取 Response 对象的 content 属性。

## 请求与响应对象 header

任何时候调用`requests.*()`你都在做两件主要的事情。其一，你在构建一个 Request 对象， 该对象将被发送到某个服务器请求或查询一些资源。其二，一旦 requests 得到一个从 服务器返回的响应就会产生一个 Response 对象。该响应对象包含服务器返回的所有信息， 也包含你原来创建的 Request 对象。如下是一个简单的请求，从Wikipedia的服务器得到 一些非常重要的信息:

    >>> r = requests.get('http://en.wikipedia.org/wiki/Monty_Python')
如果想访问服务器返回给我们的响应头部信息，可以这样做:

```
    >>> r.headers
    {'content-length': '56170', 'x-content-type-options': 'nosniff', 'x-cache':
    'HIT from cp1006.eqiad.wmnet, MISS from cp1010.eqiad.wmnet', 'content-encoding':
    'gzip', 'age': '3080', 'content-language': 'en', 'vary': 'Accept-Encoding,Cookie',
    'server': 'Apache', 'last-modified': 'Wed, 13 Jun 2012 01:33:50 GMT',
    'connection': 'close', 'cache-control': 'private, s-maxage=0, max-age=0,
    must-revalidate', 'date': 'Thu, 14 Jun 2012 12:59:39 GMT', 'content-type':
    'text/html; charset=UTF-8', 'x-cache-lookup': 'HIT from cp1006.eqiad.wmnet:3128,
    MISS from cp1010.eqiad.wmnet:80'}
```
然而，如果想得到发送到服务器的请求的头部，我们可以简单地访问该请求，然后是该请求的头部:

```
    >>> r.request.headers
    {'Accept-Encoding': 'identity, deflate, compress, gzip',
    'Accept': '*/*', 'User-Agent': 'python-requests/0.13.1'}
```

## 响应内容的编码方式

当你收到一个响应时，Requests会猜测响应的编码方式，**用于在你调用 Response.text 方法时对响应进行解码**。

Requests首先在HTTP头部检测是否存在指定的编码方式，如果不存在，则会使用 charade 来尝试猜测编码方式。

只有当HTTP头部不存在明确指定的字符集，并且 Content-Type 头部字段包含 text 值之时， Requests才不去猜测编码方式。

在这种情况下， RFC 2616 指定默认字符集 必须是 `ISO-8859-1` 。Requests遵从这一规范。  
**如果你需要一种不同的编码方式，你可以手动设置 Response.encoding 属性，或使用原始的 Response.content** 。

## 代理
如果需要使用代理，你可以通过为任意请求方法提供 proxies 参数来配置单个请求:
```
    import requests
    
    proxies = {
      "http": "http://10.10.1.10:3128",
      "https": "http://10.10.1.10:1080",
    }
requests.get("http://example.org", proxies=proxies)
```

你也可以通过环境变量 `HTTP_PROXY` 和 `HTTPS_PROXY` 来配置代理。
```
    $ export HTTP_PROXY="http://10.10.1.10:3128"
    $ export HTTPS_PROXY="http://10.10.1.10:1080"
    $ python
    >>> import requests
    >>> requests.get("http://example.org")
```
若你的代理需要使用HTTP Basic Auth，可以使用 `http://user:password@host/` 语法:
```
    proxies = {
        "http": "http://user:pass@10.10.1.10:3128/",
    }
```

## HTTP动词

Requests提供了几乎所有HTTP动词的功能：GET，OPTIONS， HEAD，POST，PUT，PATCH和DELETE。 以下内容为使用Requests中的这些动词以及Github API提供了详细示例。

我将从最常使用的动词GET开始。HTTP GET是一个幂等的方法，从给定的URL返回一个资源。因而， 当你试图从一个web位置获取数据之时，你应该使用这个动词。**一个使用示例是尝试从Github上获取关于一个特定commit的信息**。

假设我们想获取Requests的commit a050faf 的信息。我们可以 这样去做:
```
    >>> import requests
    >>> r = requests.get('https://api.github.com/repos/kennethreitz/requests/git/commits/a050faf084662f3a352dd1a941f2c7c9f886d4ad')
```
我们应该确认Github是否正确响应。如果正确响应，我们想弄清响应内容是什么类型的。像这样去做:
```
    >>> if (r.status_code == requests.codes.ok):
    ...     print r.headers['content-type']
    ...
    application/json; charset=utf-8
```
可见，**GitHub返回了JSON数据**，非常好，这样就可以使用 `r.json` 方法把这个返回的数据解析成Python对象。
```
    >>> commit_data = r.json()
    >>> print commit_data.keys()
    [u'committer', u'author', u'url', u'tree', u'sha', u'parents', u'message']
    >>> print commit_data[u'committer']
    {u'date': u'2012-05-10T11:10:50-07:00', u'email': u'me@kennethreitz.com', u'name': u'Kenneth Reitz'}
    >>> print commit_data[u'message']
    makin' history
```
到目前为止，一切都非常简单。嗯，我们来研究一下GitHub的API。我们可以去看看文档， 但如果使用Requests来研究也许会更有意思一点。我们可以借助Requests的OPTIONS动词来看看我们刚使用过的url 支持哪些HTTP方法。
```
    >>> verbs = requests.options(r.url)
    >>> verbs.status_code
    500
```
额，这是怎么回事？毫无帮助嘛！原来GitHub，与许多API提供方一样，实际上并未实现OPTIONS方法。 这是一个恼人的疏忽，但没关系，那我们可以使用枯燥的文档。然而，**如果GitHub正确实现了OPTIONS， 那么服务器应该在响应头中返回允许用户使用的HTTP方法**，例如
```
    >>> verbs = requests.options('http://a-good-website.com/api/cats')
    >>> print verbs.headers['allow']
    GET,HEAD,POST,OPTIONS
```
转而去查看文档，我们看到对于提交信息，另一个允许的方法是POST，它会创建一个新的提交。
由于我们正在使用Requests代码库，我们应尽可能避免对它发送笨拙的POST。作为替代，我们来 玩玩GitHub的Issue特性。

本篇文档是回应Issue #482而添加的。鉴于该问题已经存在，我们就以它为例。先获取它。
```
    >>> r = requests.get('https://api.github.com/repos/kennethreitz/requests/issues/482')
    >>> r.status_code
    200
    >>> issue = json.loads(r.text)
    >>> print issue[u'title']
    Feature any http verb in docs
    >>> print issue[u'comments']
    3
```
Cool，有3个评论。我们来看一下最后一个评论。
```
    >>> r = requests.get(r.url + u'/comments')
    >>> r.status_code
    200
    >>> comments = r.json()
    >>> print comments[0].keys()
    [u'body', u'url', u'created_at', u'updated_at', u'user', u'id']
    >>> print comments[2][u'body']
    Probably in the "advanced" section
```
嗯，那看起来似乎是个愚蠢之处。我们发表个评论来告诉这个评论者他自己的愚蠢。那么，这个评论者是谁呢？
```
    >>> print comments[2][u'user'][u'login']
    kennethreitz
```
好，我们来告诉这个叫肯尼思的家伙，这个例子应该放在快速上手指南中。根据GitHub API文档， 其方法是POST到该话题。我们来试试看。
```
    >>> body = json.dumps({u"body": u"Sounds great! I'll get right on it!"})
    >>> url = u"https://api.github.com/repos/kennethreitz/requests/issues/482/comments"
    >>> r = requests.post(url=url, data=body)
    >>> r.status_code
    404
```
额，这有点古怪哈。可能我们需要验证身份。那就有点纠结了，对吧？不对。Requests简化了多种身份验证形式的使用， 包括非常常见的Basic Auth。
```
    >>> from requests.auth import HTTPBasicAuth
    >>> auth = HTTPBasicAuth('fake@example.com', 'not_a_real_password')
    >>> r = requests.post(url=url, data=body, auth=auth)
    >>> r.status_code
    201
    >>> content = r.json()
    >>> print(content[u'body'])
    Sounds great! I'll get right on it.
```
精彩！噢，不！我原本是想说等我一会，因为我得去喂一下我的猫。如果我能够编辑这条评论那就好了！ 幸运的是，GitHub允许我们使用另一个HTTP动词，PATCH，来编辑评论。我们来试试。
```
    >>> print(content[u"id"])
    5804413
    >>> body = json.dumps({u"body": u"Sounds great! I'll get right on it once I feed my cat."})
    >>> url = u"https://api.github.com/repos/kennethreitz/requests/issues/comments/5804413"
    >>> r = requests.patch(url=url, data=body, auth=auth)
    >>> r.status_code
    200
```
非常好。现在，我们来折磨一下这个叫肯尼思的家伙，我决定要让他急得团团转，也不告诉他是我在捣蛋。 这意味着我想删除这条评论。GitHub允许我们使用完全名副其实的DELETE方法来删除评论。我们来清除该评论。
```
    >>> r = requests.delete(url=url, auth=auth)
    >>> r.status_code
    204
    >>> r.headers['status']
    '204 No Content'
```
很好。不见了。最后一件我想知道的事情是我已经使用了多少限额（ratelimit）。查查看，GitHub在响应头部发送这个信息， 因此不必下载整个网页，我将使用一个HEAD请求来获取响应头。
```
    >>> r = requests.head(url=url, auth=auth)
    >>> print r.headers
    ...
    'x-ratelimit-remaining': '4995'
    'x-ratelimit-limit': '5000'
    ...
```
很好。是时候写个Python程序以各种刺激的方式滥用GitHub的API，还可以使用4995次呢。

## Reference Link

1. [高级用法-官方文档](http://docs.python-requests.org/zh_CN/latest/user/advanced.html#advanced)
2. [会话API文档](http://docs.python-requests.org/zh_CN/latest/api.html#sessionapi)
