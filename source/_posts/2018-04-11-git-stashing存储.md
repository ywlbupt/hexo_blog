title: git~stashing存储
date: 2018-04-11 11:26:16
category: [Git]
tags:
description:
---
[Toc]

'git stash'就可以将你当前未提交到本地（和服务器）的代码推入到Git的栈中，

这时候你的工作区间和上一次提交的内容是完全一样的，所以你可以放心的修 Bug。

等到修完Bug，提交到服务器上后，再使用`git stash apply`将以前一半的工作应用回来。

也许有的人会说，那我可不可以多次将未提交的代码压入到栈中？答案是可以的。

当你多次使用`git stash`命令后，你的栈里将充满了未提交的代码，这时候你会对将哪个版本应用回来有些困惑，

`git stash list`命令可以将当前的Git栈信息打印出来，你只需要将找到对应的版本号。

例如使用`git stash apply stash@{1}`就可以将你指定版本号为`stash@{1}`的工作取出来。

当你将所有的栈都应用回来的时候，可以使用`git stash clear`来将栈清空。

其他

* `git stash save -u` 会把没有记录到的文件也保存下来(比如你新建了一个文件，但是还没有git add，stash也会把这个文件保存下来)
* `git stash drop <stash@{id}>` 如果不加stash编号，默认的就是删除最新的，也就是编号为0的那个，加编号就是删除指定编号的stash

### git stash apply 与 git stash pop

学习过栈的朋友都知道栈有个特点‘后进先出’。

当我们用git stash把未提交本地仓库的修改保存入栈的时候，用git stash list 可以查看到栈的记录。

如果我们想恢复刚才的改动，可以使用`git stash apply` 或者 `git stash pop`。

虽然都能恢复之前的改动，但是这两者还是有区别的：
* `git stash apply` 只是把栈里面记录的内容运用到当前的版本上，这时候栈里面的记录还保存着。 
* `git stash pop` 也是把栈里面记录的内容运用到当前的版本上，但是它会把出栈的记录删掉，也就是说如果之前栈里面有3个记录，当执行 ‘git stash pop’之后，栈里的记录变成2个，这就是pop（出栈）的特点。
