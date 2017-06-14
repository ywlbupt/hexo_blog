title: Git~git时光穿梭机
date: 2016-01-12 01:37:42
tags: git
description: git的各类撤销回滚操作
----

master: 默认开发分支

origin: 默认远程版本库

Head: 默认开发分支，当前版本

Head^: Head的父提交

Head~1 同 Head^

### 版本回退

>   [版本回退-廖雪峰](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013744142037508cf42e51debf49668810645e02887691000)
>   如果要把当前版本回退到上一个版本，可以使用以下命令：
>
        git reset --hard HEAD^

>   现在，你回退到了某个版本，关掉了电脑，第二天早上就后悔了，想恢复到新版本怎么办？找不到新版本的commit id怎么办？
>   在Git中，总是有后悔药可以吃的。当你用`$ git reset --hard HEAD^`回退到add distributed版本时，再想恢复到append GPL，就必须找到append GPL的commit id。Git提供了一个命令`git reflog`用来记录你的每一次命令：
>
        $ git reflog
        ea34578 HEAD@{0}: reset: moving to HEAD^
        3628164 HEAD@{1}: commit: append GPL
        ea34578 HEAD@{2}: commit: add distributed
        cb926e7 HEAD@{3}: commit (initial): wrote a readme file

#### git reset 的参数说明

git reset用于撤销未被提交到远端的改动。除了可以移动当前分支的HEAD，你可以通过不同的标记选择修改 staged snapshot 或者 working directory

*	--soft： 暂存区(staged snapshot)和 工作区(working directory) 都未被改变 (建议在命令行执行后，再输入 git status 查看状态)
*	--mixed： staged snapshot 被更新， working directory 未被更改。【这是默认选项】（建议同上)
*	--hard： staged snapshot 和 working directory 都将回退。
    --hard 很危险，它会直接回退你之前所有的修改，使用前，可以事先保存 commit id.


【这些标记经常和HEAD一起使用。例如，`git reset --mixed HEAD`可撤销所有缓存改动，但是保留他们在工作目录下。`git reset --hard HEAD`可彻底删除没有提交的改动。】

#### 修复已提交文件中的错误

*   创建新提交来修复错误
    创建一个新的，撤消(revert)了前期修改的提交(commit)是很容易的; 只要把出错的提交(commit)的名字(reference)做为参数传给命令: git revert就可以了; 下面这条命令就演示了如何撤消最近的一个提交:

        $ git revert HEAD
这样就创建了一个撤消了上次提交(HEAD)的新提交, 你就有机会来修改新提交(new commit)里的提交注释信息.

### git log

*   git log查看历史版本

        git log --graph --pretty=oneline --abbrev-commit        #用带参数的git log也可以看到分支的合并情况：  

### git commit -a

跳过git stage命令，直接递交修改和删除的文件，新建的文件不受影响，不会被加入暂存区

>   commit -a  
>   Tell the command to automatically stage files that have been modified and deleted, but new files you have not told git about are not affected.

### git diff

当一个文件做了stage，然后又做了一些修改，则：
*   `git diff` 显示当前工作区的文件和stage区文件的差异
*   `git diff --staged` 显示stage区和HEAD的文件的差异
    -   `git diff --cached`==`git diff --staged`: 建议使用 git diff --staged 替代 git diff --cached
*   `git diff HEAD` 显示工作区和上次递交文件HEAD的差异 
    -   `git diff HEAD -- readme.txt`命令可以查看工作区和版本库里面最新版本的区别

### git checkout -- file 文件快照的回退

当一个文件做了git stage，任何时候，都可以回退到stage的状态
    
    git checkout -- hellp.py

git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以一键还原

#### git reset与checkout的区别
当文件加入了 stage 区以后，如果要从stage删除，则使用 reset,此时工作区的文件不做任何修改，

比如：`git reset hello.py`这个命令就是 `git stage hello.py` 的反操作。

当文件加入了 stage 区以后，后来又做了一些修改，  
这时发现后面的修改有问题，想回退到stage的状态，使用 checkout 命令：`git checkout hello.py`


### git commit --ammend 修改已经提交的注释  

对于已经修改提交过的注释，如果需要修改，可以借助`git commit --amend` 来进行。  

例如 在framework/base 里最新的提交就是 resolving the roaming problem，  
我现在需要将其改为 resolving the roaming problem for fixing bug 7732  
在 framework/base 下 输入 `git commit --amend`,就会进入一个文本编辑界面（如下）  
在注释的地方修改 ，保存然后退出，这样注释就修改了，再重新push.

```
resolving the roaming problem 

Change-Id: I7e7daffd2e8a5e65130629dc1c56dee5f5c4b5f7

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# On branch shuai.lu_branch
# Changes to be committed:
#   (use "git reset HEAD^1 <file>..." to unstage)
#
#       modified:   telephony/java/com/android/internal/telephony/gsm/GsmServic$
#
```

### Reference Link
1. [时光机穿梭-廖雪峰](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013743858312764dca7ad6d0754f76aa562e3789478044000)
2. [知乎回答-彭勇](https://www.zhihu.com/question/19946553/answer/13759819)
