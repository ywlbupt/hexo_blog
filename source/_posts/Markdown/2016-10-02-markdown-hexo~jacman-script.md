title: markdown-hexo~jacman-script
date: 2016-10-02 15:08:06
category: [Markdown, Hexo]
tags:
  - hexo
  - jacman
  - javascript
  - ejs引擎
description:
----
[Toc]

### function root_for(path)

如果path为空，赋值根目录符号`/`
判断path是否为包含root的完整路径，
如果是，返回path
如果否，判断path的首字符是否为根目录符号`/`，再与root拼接(前提：root结尾为路径符号`/`)

Locate：`theme/jacman/_partial/header.ejs`
```javascript
function root_for(path){
    path = path || '/'
    var root = config.root;
    if (path.substring(0, root.length) !== root){
    if (path.substring(0, 1) === '/'){
      path = root.substring(0, root.length - 1) + path;
    } else {
      path = root + path;
    }
  }
  return path;
}
```
* Javascript语言
* path为theme中定义的menu (List Item)，且视为字符串
* path.substring(0,{num}),从0的位置截取{num}个字符，返回{num}个字符组成的字符串


