title: ubuntu-cmd~grep_find
date: 2016-10-03 18:37:18
category: [Ubuntu, Cmd]
tags:
  - grep
  - find
description:
-------
[Toc]

## 【grep|find】查找字符串

### 参考链接 ###
参考链接
* [grep-查找字符串-GNU官方指导](https://www.gnu.org/software/grep/manual/grep.html)
* [find-查找文件-GNU官方指导](https://www.gnu.org/software/findutils/manual/html_mono/find.html)
* `$ man grep`

### Command Line ###

```
$ grep options pattern input_file_names
```

#### Matching options ####

* `-e pattern`, `--regex=pattern`
    * 使用正则表达式
* `-f file`, `--file=file`
    * 查找文件，如果`-e pattern -f file`表示使用正则表达式查找文件
* `-i`, `--ignore-case` 忽略大小写
* `-w`, `--word-regexp` 单词查找
* `-x`, `--line-regexp` 行查找

#### Ouput Control ####

* `-n` 输出行号
* `-c` 计算找到匹配字符串次数
* `-v` 反向选择,显示没有出现pattern的行
* `--color=auto` 高亮关键字
* `-l` 输出包含满足匹配字符串的文件名

.etc

* 使用`alias`简化命令行, 高亮搜索关键字
  在关键字的显示方面，`grep` 可以使用 `--color=auto` 来将关键字部分使用颜色显示。 这可是个很不错的功能啊！但是如果每次使用 grep 都得要自行加上 `--color=auto` 又显的很麻烦～ 此时那个好用的 alias 就得来处理一下啦！你可以在 `~/.bashrc` 内加上这行：`alias grep='grep --color=auto'`再以 `source ~/.bashrc` 来立即生效即可喔！ 这样每次运行 `grep` 他都会自动帮你加上颜色显示啦
  ```bash
  if [ -x /usr/bin/dircolors ]; then
  test -r ~/.dircolors && eval "$(dircolors -b ~/.dircolors)" || eval "$(dircolors -b)"
  alias ls='ls --color=auto'
  #alias dir='dir --color=auto'
  #alias vdir='vdir --color=auto'

  alias grep='grep --color=auto'
  alias fgrep='fgrep --color=auto'
  alias egrep='egrep --color=auto'
  fi
  ```

一些简单明了的例子
* 明确要求搜索子目录：`grep -r`
* 或忽略子目录：`grep -d skip`
* 如果有很多 输出时，您可以通过管道将其转到'less'上阅读：
    ```
    $ grep magic /usr/src/Linux/Documentation/* | less
    ```
* `grep -i pattern files ` :不区分大小写地搜索。默认情况区分大小写，
* `grep -l pattern files ` :只列出匹配的文件名，
* `grep -L pattern files ` :列出不匹配的文件名，
* `grep -w pattern files ` :只匹配整个单词，而不是字符串的一部分(如匹配'magic'，而不是'magical')，
* `grep -C number pattern files`  :匹配的上下文分别显示[number]行，
* `grep pattern1 | pattern2 files`  :显示匹配 pattern1 或 pattern2 的行，
* `grep pattern1 files | grep pattern2` :显示既匹配 pattern1 又匹配 pattern2 的行。
* `grep -n pattern files`  即可显示行号信息
* `grep -c pattern files`  即可查找总行数
* `grep man *` 会匹配'Batman'、'manic'、'man'等，
* `grep '\<man' *` 匹配'manic'和'man'，但不是'Batman'，
* `grep '\<man\>'` 只匹配'man'，而不是'Batman'或'manic'等其他的字符串。
* `^`：指匹配的字符串在行首，
* `$`：指匹配的字符串在行尾，


### Exmaple ###

#### 查找字符串 ####

```bash
# 将/etc/passwd，有出现 root 的行取出
$ grep root /etc/passwd
# 或
$ cat /etc/passwd | grep root
# 同时显示行号
$ grep -n root /etc/passwd
# 将/etc/passwd，将没有出现 root 和nologin的行取出来
$ grep -v root /etc/passwd | grep -v nologin
# 用 dmesg 列出核心信息，再以 grep 找出内含 eth 那行,要将捉到的关键字显色，且加上行号来表示：
$ dmesg | grep -n --color=auto 'eth'
# 同上,把关键字所在行的前两行和后三行也一起显示出来
$ dmesg | grep -n -A3 -B2 --color=auto 'eth'
```

#### 目录递归查找 ####

```bash
$ grep 'energywise' *           #在当前目录搜索带'energywise'行的文件
$ grep -r 'energywise' *        #在当前目录及其子目录下搜索'energywise'行的文件
$ grep -l -r 'energywis' *     #在当前目录及其子目录下搜索'energywise'行的文件，但是不显示匹配的行，只显示匹配的文件
```

#### 正则表达式查找 ####

```bash
# 查找 test 或 taste 这两个单字
$ grep -n 't[ae]st' regular_express.txt
# 反向查找,查找有 oo 的行，但 oo 前面不能有 g
$ grep -n '[^g]oo' regular_express.txt
$ grep -n '[^a-z]oo' regular_express.txt # 非小写字母开头的字符串oo
$ grep -n '^[a-z]' regular_express.txt # 小写字符为行首的行
$ grep -n '^the' regular_express.txt
$ grep -n '^$' regular_express.txt # 找出空白行 
$ grep -n 'go\{2,5\}g' regular_express.txt # 找到g和g中间有2个到5个o的行,因{}在shell中有特殊意义,需转义
```

* egrep (grep -e pattern)
```bash
# 打印包含1个或多个3的行,下面三个表达式等效
$ egrep '3+' testfile
$ grep -E '3+' testfile
$ grep '3\+' testfile      
# 首先含有字符2，其后紧跟着0个或1个点，后面再是0和9之间的数字
$ egrep '2\.?[0-9]' testfile
$ grep -E '2\.?[0-9]' testfile
$ grep '2\.\?[0-9]' testfile
```

