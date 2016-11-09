title: python-basic~re正则表达式
date: 2016-10-19 17:51:00
category: [Python, Basic]
tags:
description:
----
[Toc]
 
1. [Pythondoc  regex](https://docs.python.org/3/howto/regex.html)
2. [Pythondoc 3p5 re](https://docs.python.org/3.5/library/re.html)
 
### <span id="flag">匹配的规则选项</span>
 
* re.ASCII , A : `\w`,`\b`,`\s`只匹配ASCII字符而不是full unicode。This is only meaningful for Unicode patterns, and is ignored for byte patterns.
* re.IGNORECASE , I : 忽略大小写
* re.MULTILINE , M : 多行模式的匹配
* re.DOTALL , S : dot matches all
    * `.`匹配除换行符`\n`外的人艺字符，如果设置了`re.DOTALL`模式，将包含匹配换行符
* re.VERBOSE , X : 忽略正则表达式中的空白，换行，方便添加注释#，正则表达式的可读性, [例子](#jump)
* re.LOCALE , L : 字符集本地化，用于多语言环境
 
#### `flag`语法
 
* `re.complie(stringPattern [,flag])` : `flag` -> `re.I|re.M`
    * Ex : `re.compile("pattern", re.I|re.M)` 该写法与 `re.complie("(?im)pattern")` 等价
    * Ex : `re.match("(?im)pattern", string )`与`re.match("pattern", string , re.I|re.M)`等价
 
### 常用限定符及贪婪greedy匹配
 
#### 贪婪
 
* `'*'` 匹配0次或多次
    Causes the resulting RE to match 0 or more repetitions of the preceding RE, as many repetitions as are possible. ab* will match ‘a’, ‘ab’, or ‘a’ followed by any number of ‘b’ s.
* `'+'` 匹配1次或多次
    Causes the resulting RE to match 1 or more repetitions of the preceding RE. ab+ will match ‘a’ followed by any non-zero number of ‘b’s; it will not match just ‘a’.
* `'?'` 匹配0次或1次
    Causes the resulting RE to match 0 or 1 repetitions of the preceding RE. ab? will match either ‘a’ or ‘ab’.
* `{m}` 重复每次
* `{m,n}`,`{,n}`,`{m,}` 重复m次到n次；重复0到n次；重复m到多次。
 
#### 非贪婪
* `*?, +?, ??` 尽可能少的匹配
The`'*'`,`'+'`, and `'?'` qualifiers are **all greedy**; they match as much text as possible. Sometimes this behaviour isn’t desired; if the RE `<.*>` is matched against ` b  `, it will match the entire string, and not just ` `. Adding `?` after the qualifier makes it perform the match in non-greedy or minimal fashion; as few characters as possible will be matched. Using the RE `<.*?>` will match only` `.
* `{m,n}?`
 
#### 特殊符号的匹配
* 括号`()`的匹配
  to match the literals '(' or ')', use `\(` or `\)`, or enclose them inside a character class: `[(]` `[)]`.
* 中括号`[]`的匹配
  同`()`，使用`\[ \]`来匹配
 
### group 分组匹配
对立项
* `(...)`对正则表达式进行分组，一个括号一组(group)，在match后能够通过类似`\1`的方式retrieved
* `(?:...)`对正则表达式进行分组，与`(...)`对立，不能retrieved
* `(?...)`，举例说明，`(?aiLmsux)`，该格式适用于在将`flag`写在`pattern`的表达式中。[匹配规则](#flag)
 
- `(?#...)` 正则表达式中的注释。不起作用，be ingored
 
#### 分组的嵌套
 
> Subgroups are numbered from left to right, from 1 upward. Groups can be nested; to determine the number, just count the opening parenthesis characters, going from left to right.
 
分组的顺序，从左到右，按左括号的顺序来引来retrieved
```python
>>> p = re.compile('(a(b)c)d')
>>> m = p.match('abcd')
>>> m.group(0)
'abcd'
>>> m.group(1)
'abc'
>>> m.group(2)
'b'
```
 
#### 扩展匹配`(?...)`
* `(?P<name>...)` 给正则表达式的分组命令，名字为`name`，可通过`m.group('name')`或`\g<name>`引用retrieved
    * `(?P=name)` 使用名字为name的分组
 
注：此匹配的格式为python中的写法
 
#### 前后相匹配
* `(?=...)` : a lookahead assertion，`Isaac(?=Asimov)` 将匹配`IsaacAsimov`中的`Isaac`
* `(?<=...)` : a postive lookbehind assertion，`(?<=abc)def`将匹配`abcdef`中的`def`
* `(?!=...)` : a negetive lookbehind assertion，与`(?=<...)`相反
 
### re Function 查找
|Method/Attribute  |  Purpose |
|----|----|
| match()    | Determine if the RE matches at the beginning of the string.                |
| search()   | Scan through a string, looking for any location where this RE matches.     |
| findall()  | Find all substrings where the RE matches, and returns them as **a list**.  |
| finditer() | Find all substrings where the RE matches, and returns them as **an iterator**. |
 
> match() will only report a successful match which will start at 0; 
> if the match wouldn’t start at zero, match() will not report it.
 
`match()`方法只从字符串的开始位置开始匹配，而`search()`方法会向后搜索。
 
### re Function 引用
|Method/Attribute  |  Purpose |
|----|----|
|group()| Return the string matched by the RE |
|start()| Return the starting position of the match |
|end()  | Return the ending position of the match |
|span() | Return a tuple containing the (start, end) positions of the match |
 
### re Function 修改
|Method/Attribute  |  Purpose |
|----|----|
|split() | Split the string into a list, splitting it wherever the RE matches |
|sub()   | Find all substrings where the RE matches, and replace them with a different string |
|subn()  | Does the same thing as sub(), but returns the new string and the number of replacements |

#### re.split()应用

```python
import re
# 多行模式，^和$匹配每一行的行首与行末
note_sep = re.compile(r"^-{3,}$", re.M)
config = note_sep.split("h1\nh2\nh3\n---\n###beging\n")
```
 
#### re.sub() 正则替换

语法格式：`re.sub(pattern, repl, string, count=0, flags=0)`

* 使用repl替换string中每一个匹配的子串后返回替换后的字符串。 
* 当repl是一个字符串时，可以使用`\id`或`\g<id>`、`\g<name>`引用分组，但不能使用编号0。 
* 当repl是一个方法时，这个方法应当只接受一个参数（Match对象），并返回一个字符串用于替换（返回的字符串中不能再引用分组）。
* count用于指定最多替换次数，不指定时全部替换。 

* 官网例子，将两个横杆替换成一个横杆，一个横杠替换成空格
  ```python
  >>> def dashrepl(matchobj):
  ...     if matchobj.group(0) == '-': return ' '
  ...     else: return '-'
  >>> re.sub('-{1,2}', dashrepl, 'pro----gram-files')
  'pro--gram files'
  ```

```python
import re
 
p = re.compile(r'(\w+) (\w+)')
s = 'i say, hello world!'

print p.sub(r'\2 \1', s)

def func(m):
  return m.group(1).title() + ' ' + m.group(2).title()

print p.sub(func, s)
  
### output ###
# say i, world hello!
# I Say, Hello World!
```


#### re.subn() 

与`re.sub()`干一样的活，返回替换后的字符串与替换的数量的元组

## 附录
 
### re.VERBOSE 正则表达式的可读性
<span id="jump">re.VERBOSE 正则表达式的可读性</span>
 
参考链接[Pythondoc  regex](https://docs.python.org/3/howto/regex.html#using-re-verbose)  
The re.VERBOSE flag has several effects. Whitespace in the regular expression that isn’t inside a character class is ignored. This means that an expression such as dog | cat is equivalent to the less readable dog|cat, but [a b] will still match the characters 'a', 'b', or a space. In addition, you can also put comments inside a RE; comments extend from a # character to the next newline. When used with triple-quoted strings, this enables REs to be formatted more neatly:

```python
pat = re.compile(r"""
  \s*                 # Skip leading whitespace
  (?P<header>[^:]+)   # Header name
  \s* :               # Whitespace, and a colon
  (?P<value>.*?)      # The header's value -- *? used to
                      # lose the following trailing whitespace
  \s*$                # Trailing whitespace to end-of-line
""", re.VERBOSE)
```

This is far more readable than:
```python
pat = re.compile(r"\s*(?P<header>[^:]+)\s*:(?P<value>.*?)\s*$")
```
 
### 部分替换
 
This example matches the word `section` followed by a string enclosed in `{`, `}`, and changes `section` to `subsection:`
 
```bash
>>> p = re.compile('section{ ( [^}]* ) }', re.VERBOSE)
>>> p.sub(r'subsection{\1}','section{First} section{second}')
'subsection{First} subsection{second}'
```
