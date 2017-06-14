title: vim~vimscript-常用技巧
date: 2016-09-20 22:50:59
categories: [Vim]
tags:
description:
----
[Toc]

### vimscript function-lists

* confirm({msg} [, {choices} [, {default} [, {type}]]])
  confirm() 提供用户一个对话框，从中可以作出选择。返回选择的序号。第一个选择为 1。
  ```
    confirm("Save changes?", "&Yes\n&No\n&Cancel")
  ```


### vimscript中执行bash命令

> from vimscript plugin "vim-hexo.vim"
```vim
" Run command that build new blog
function! s:Runcmd()
    " 启动 Bash , -c
    " 启动完Bash后从后面的字符串中读取命令并执行，然后退出，各命令以分号隔开
    let s:cmd = "!bash -c 'cd " . g:hexo_blogpath . "; hexo n \'". s:title ."\' | cut -f3- -dt\'"
    execute s:cmd
endfunction
```

### vimscript 自动补全 complete()
* complete({startcol}, {matches})
  设置插入模式补全的匹配。此例并不很有用，但可以说明功能。注意这里返回空串，以免插入零。
	```vim
  inoremap <F5> <C-R>=ListMonths()<CR>

  func! ListMonths()
    call complete(col('.'), ['January', 'February', 'March',
    \ 'April', 'May', 'June', 'July', 'August', 'September',
    \ 'October', 'November', 'December'])
    return ''
  endfunc
	```

### vimscript 伪变量



### 参考手册
1. 插入模式补全 -> `ins-completion`
2. 文件搜索 -> `file-searching`
  * 包含统配符`**`, `*`等的使用
3. 用户定义补全 -> compl-function 
  命令补全可以由用户通过 'completefunc' 选项自定义一个函数来完成。
4. `:h function-list` 查看内置函数列表
5. globpath()
  可以用 "**" 项目来搜索目录树。例如，寻找在 'runtimepath' 和它之下所有目录里的 "README.txt" 文件: >
  ```vim
  :echo globpath(&rtp, "**/README.txt")
  ```
6. 命令行的自动补全`h command-completion-customlist`
  * 示例：`com! -nargs=+ -complete=customlist,FindFileNameComplete F call FindArgFilter(<f-args>)`[github](https://github.com/stegtmeyer/find-complete/blob/master/plugin/find-complete.vim)

7. `strridx`返回字符串`string`中，最后一次出现`/`的位置的后一位，`strpart`用于截取字符串
  ```vim
  strpart(string, strridx(string, "/") + 1)
  ```


