title: python-yaml~python解析yaml文件
date: 2016-09-21 22:23:53
category: [Python,Lib]
tags:
    - yaml
    - json
description:
---
[Toc]

### python的yaml解析

* Library : `pyyaml`
* 安装 : `pip install pyyaml`

#### yaml.load()



#### yaml.dump()

#### python 2 And python 3 Supporting

> In Python 2:
> 
* str objects are converted into !!str, !!python/str or !binary nodes depending on whether the object is an ASCII, UTF-8 or binary string.
* unicode objects are converted into !!python/unicode or !!str nodes depending on whether the object is an ASCII string or not.
* yaml.dump(data) produces the document as a UTF-8 encoded str object.
* yaml.dump(data, encoding=('utf-8'|'utf-16-be'|'utf-16-le')) produces a str object in the specified encoding.
* yaml.dump(data, encoding=None) produces a unicode object.

>In Python 3:
>
* str objects are converted to !!str nodes.
* bytes objects are converted to !!binary nodes.
* For compatibility reasons, !!python/str and !python/unicode tags are still supported and the corresponding nodes are converted to str objects.
* yaml.dump(data) produces the document as a str object.
* yaml.dump(data, encoding=('utf-8'|'utf-16-be'|'utf-16-le')) produces a bytes object in the specified encoding.

#### 参考链接

[官方链接](http://pyyaml.org/wiki/PyYAMLDocumentation)
