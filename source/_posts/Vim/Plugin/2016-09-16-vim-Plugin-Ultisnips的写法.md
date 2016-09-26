title: "vim-plugin~Ultisnips的写法"
date: 2016-09-16 16:41:49
category: [Vim, Plugin]
tags: 
    - vim
    - snippets
Description:
---
[Toc]

### 参考链接 ###
1. [Author Sirver 给出的例子](https://github.com/SirVer/ultisnips/tree/master/doc/examples)

### 基本格式 ###

开始部分的格式如下：  
The start line takes the followning format:  
```snippets
snippet tab_trigger [ "description" [ options ] ]
snippet "tab_trigger" [ "description" [ options ] ]
```
其中，`description`和`option`可选  
`tab_trigger`带引号的写法可嵌入空格

【option】  
* `b` : Beginning of a line
   只有`tab_trigger`处于行首式出发`snippet`
* `i` : the snippet can be triggered in the middle of a word.
* `w` : 作为一个单独的词时触发， 例如`date`
* `r` : 正则表达式拓展，python regular expression，正则表达式的pattern 需要用引号包起来。

`endsnippet`表示snippets 的结束

基本格式：
${0} 代表 tab 最终停留的位置， 

```snippet
snippet <tigger> "注解" <flag>
${1:name}
${1/(\w+).*/${1}/}
endsnippet
```

支持直接使用`shell script`，也可以通过`!v`嵌入`vimscript`或者`!p`嵌入`python`

#### Visual的用法 ####
1.  The ${VISUAL} placeholder can contain default text to use when the snippet 
    has been triggered when not in Visual mode. The syntax is: >  
    `${VISUAL:default text}`

2.  The ${VISUAL} placeholder can also define a transformation 
    (see |UltiSnips-transformations|). The syntax is: >  
    `${VISUAL:default/search/replace/option}.`

```snippets
snippet t
<tag>${VISUAL:inside text/should/is/g}</tag>
endsnippet
```

Start with this line of text: > ` this should be cool `

Position the cursor on the word `"should"`, then press the key sequence: 
1.  viw (visual mode -> select inner word). 
2.  Then press <Tab>, type "t" and press <Tab> again. 

The result is: > `this <tag>is</tag> be cool`

If you expand this snippet while not in Visual mode 
(e.g., in Insert mode type `t<Tab>`), you will get: >  
   `<tag>inside text</tag>`

#### 插入时间的数写法-vimscript ####

```bash
# 使用shell script
LastUpdate : `date +%Y-%m-%d\ %H:%M`
# 使用`vimscript`
LastUpdate : `!v strftime("%Y-%m-%d %H:%M:%S")`
```

#### Python 脚本 ####

通过`global !p`直接嵌入`python`脚本

python中的特殊变量
```
fn      - The current filename
path    - The complete path to the current file
t       - The values of the placeholders, t[1] is the text of ${1}, etc.
snip    - UltiSnips.TextObjects.SnippetUtil object instance. Has methods
            that simplify indentation handling.
context - Result of context condition. See |UltiSnips-context-snippets|.
```

关于snip实例，方法见[snip](#snip)


### 附录－python对象snip的方法列表 ###
*<span id="snip">snip</span>*

参考UltiSnips的Help文档

The 'snip' object provides the following methods: >

    snip.mkline(line="", indent=None):
        Returns a line ready to be appended to the result. If indent
        is None, then mkline prepends spaces and/or tabs appropriate to the
        current 'tabstop' and 'expandtab' variables.

    snip.shift(amount=1):
        Shifts the default indentation level used by mkline right by the
        number of spaces defined by 'shiftwidth', 'amount' times.

    snip.unshift(amount=1):
        Shifts the default indentation level used by mkline left by the
        number of spaces defined by 'shiftwidth', 'amount' times.

    snip.reset_indent():
        Resets the indentation level to its initial value.

    snip.opt(var, default):
        Checks if the Vim variable 'var' has been set. If so, it returns the
        variable's value; otherwise, it returns the value of 'default'.

The 'snip' object provides some properties as well: >

    snip.rv:
        'rv' is the return value, the text that will replace the python block
        in the snippet definition. It is initialized to the empty string. This
        deprecates the 'res' variable.

    snip.c:
        The text currently in the python block's position within the snippet.
        It is set to empty string as soon as interpolation is completed. Thus
        you can check if snip.c is != "" to make sure that the interpolation
        is only done once. This deprecates the "cur" variable.

    snip.v:
         Data related to the ${VISUAL} placeholder. The property has two
         attributes:
             snip.v.mode   ('v', 'V', '^V', see |visual-mode| )
             snip.v.text   The text that was selected.

    snip.fn:
        The current filename.

    snip.basename:
        The current filename with the extension removed.

    snip.ft:
        The current filetype.

    snip.p:
        Last selected placeholder. Will contain placeholder object with
        following properties:

