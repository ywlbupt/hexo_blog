title: ubuntu-cmd~alias
date: 2016-10-03 23:09:47
category: [Ubuntu, Cmd]
tags:
description:
---
[Toc]

## Alias in ubuntu 
@(Ubuntu/Shell)[ubuntu|alias]

* 使用`alias`查看当前系统使用了哪些别名
* h=

```bash
ywl@ywl-ThinkPad-E420:~$ alias
alias alert='notify-send --urgency=low -i "$([ $? = 0 ] && echo terminal || echo error)" "$(history|tail -n1|sed -e '\''s/^\s*[0-9]\+\s*//;s/[;&|]\s*alert$//'\'')"'
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias grep='grep --color=auto'
alias l='ls -CF'
alias la='ls -A'
alias ll='ls -alF'
alias ls='ls --color=auto'
alias rvm-restart='rvm_reload_flag=1 source '\''/home/ywl/.rvm/scripts/rvm'\'''
```

### 设置永久别名 `~/.bashrc`

* 编辑根目录下的`$ vim ~/.bashrc`文件
    ```bash
    alias vimmkd="vim -c \"cd ~/Documents/markdown_note\""
    ```

