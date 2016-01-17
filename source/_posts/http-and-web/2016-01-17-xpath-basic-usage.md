title: xpath-python-lxml解析xpath
date: 2016-01-17 13:12:16
tags: xpath
description:
---

## python-lxml解析xpath

现在来看如何直接使用lxml（即前文说过的libxml2的一个python binding）处理那个样本页

```
    import codecs
    
    from lxml import etree
    
    
    
    f=codecs.open("raptor.htm","r","utf-8")
    
    content=f.read()
    
    f.close()
    
    tree=etree.HTML(content)
```

Bingo！果然成功。关键就在于etree提供了HTML这个解析函数。之后的事情就好办多了，因为可以直接对HTML使用xpath。
不过这个样本页面中要解析的部分有一个问题就是：那个ul节点没有id，所以只好麻烦一点了。
完整的xpath应该是这样写的（注意，原文件中的TAG有大小写的情况，但是在XPATH里只能用小写）：

    nodes=tree.xpath(u"/html/body/form/div[@id='leftmenu']/h3[text()='档案']/following-sibling::ul[1]")

这句xpath的意思是寻找这样一个东东。
```
    <html>
    
      <body>
    
        <form>
    
          <div id='leftmenu'>
    
            <h3>档案</h3>
    
            <ul><!-- 找到这里 -->
    </ul>
    
          </div>
    
        </form>
    
      </body>
    
    </html>
```
当然更简单的方法就是直接根据id定位：

    nodes=tree.xpath(u"//div[@id='leftmenu']/h3[text()='档案']/following-sibling::ul[1]")

这两种方法返回的结果中，nodes[0]就是那个"档案"的h3节点后面紧跟的第一个ul节点。
之后就可以把每个月份的文本列出（注意，是以上面取得的ul节点为起点）：
```
    nodes=nodes[0].xpath("li/a")
    
    for n in nodes:
    
        print n.text
```

这段的意思是取得这个ul节点下的所有<li><a>节点。之后的循环就是把这些节点的文本内容列出。


### 附录：XPATH的简单语法介绍

XPATH基本上是用一种类似目录树的方法来描述在XML文档中的路径。比如用"/"来作为上下层级间的分隔。第一个"/"表示文档的根节点（注意，不是指文档最外层的tag节点，而是指文档本身）。比如对于一个HTML文件来说，最外层的节点应该是"/html"。

同样的，`..`和`.`分别被用来表示父节点和本节点。

XPATH返回的不一定就是唯一的节点，而是符合条件的所有节点。比如在HTML文档里使用`/html/head/scrpt`就会把head里的所有script节点都取出来。

为了缩小定位范围，往往还需要增加过滤条件。过滤的方法就是用"[""]"把过滤条件加上。比如在HTML文档里使用`/html/body/div[@id='main']`，即可取出body里id为main的div节点。

其中@id表示属性id，类似的还可以使用如@name, @value, @href, @src, @class....

而函数text()的意思则是取得节点包含的文本。比如：`<div>hello<p>world</p>< /div>`中，用`div[text()='hello']`即可取得这个div，而world则是p的text()。

函数position()的意思是取得节点的位置。比如`li[position()=2]`表示取得第二个li节点，它也可以被省略为`li[2]`。

不过要注意的是数字定位和过滤 条件的顺序。比如`ul/li[5][@name='hello']`表示取ul下第五项li，并且其name必须是hello，否则返回空。而如果用 `ul/li[@name='hello'][5]`的意思就不同，它表示寻找ul下第五个name为"hello"的li节点。

此外，`*`可以代替所有的节点名，比如用`/html/body/*/span`可以取出body下第二级的所有span，而不管它上一级是div还是p或是其它什么东东。

而 `descendant::`前缀可以指代任意多层的中间节点，它也可以被省略成一个`/`。比如在整个HTML文档中查找id为"leftmenu"的 div，可以用`/descendant::div[@id='leftmenu']`，也可以简单地使用`//div[@id='leftmenu']`。

至于`following-sibling::`前缀就如其名所说，表示同一层的下一个节点。`following-sibling::*`就是任意下一个节点，而`following-sibling::ul`就是下一个ul节点。

## Reference Link
