title: vim-plugin~fugitive
date: 2016-10-03 23:12:14
category: [Vim, Plugin]
tags:
description:
----
[Toc]

## 【fugitive】git Wrapper in vim

### 与git命令的一一对应

#### 对当前文件的操作

* `Gedit` git编辑文件
    * `Gsplit`, `Gvsplit`, `Gtabedit`
* `Gwrite` -> `git add %` : stage current file into the index
* `Gread` -> `git checkout %` : Revert current file to laste checked in version，将当前文件恢复至上一版本 
    * unstaged current file，与Gwrite相对
* `Glog` -> `git log` 
    * loads all previous revisions of a file into the quickfix list( `:cw 10` 打开quickfix窗口) so you can iterate over them and watch the file evolve!
    * 针对文件，可在`quickfix`窗口中直接 `o`打开，查看历史文件！
* `Gdiff` -> `git diff` : 文件的比较 , resolve a git merge conflict
* `Gremove` -> `git rm %` : Delete current file and corresponding Vim buffer

#### 对仓库的操作

* `Gstatus` -> `git status`  在git status preview的窗口，可进行以下操作：
    * `-`   -> `add` : add a file's changes
    * `-`   -> `reset` (staged file) 
    * `p`   -> `add/reset -- patch`, toggle operation ,as `-`
    * `cc`  -> `Gcommit`
    * `ca`  -> `Gcommit --amend`
    * `cA`  -> `Gcommit --amend --reuse-message=HEAD`
    * `cva` -> `Gcommit --amend --verbose`
    * `cvc` -> `Gcommit --verbose`
    * `D`   -> `Gdiff`
    * `ds`  -> `Gsdiff`
    * `O`   -> `Gtabedit`
    * `o`   -> `Gsplit`
    * `S`   -> `Gvsplit`
    * `dv`  -> `Gvdiff`
    * `U`   -> `checkout`
    * `U`   -> `checkout HEAD` (staged files)
    * `U`   -> `clean` (untracked files)
    * `U`   -> `rm` (unmerged files)
    * `q`   -> close status
    * `r`   -> reload status
* `Gcommit` -> `git commit`
