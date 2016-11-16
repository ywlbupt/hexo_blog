title: ubuntu-cmd~tmux
date: 2016-10-13 07:42:31
category: [Ubuntu, Cmd]
tags:
  - tmux
description: Ubuntu下多终端的应用
----
[Toc]

## 【tmux】多终端操作
@(tmux)
* A user-specific configuration file should be located at `~/.tmux.conf`,
* while a global configuration file should be located at `/etc/tmux.conf`.

<!-- more -->

每当开启一个新的会话时，Tmux 都会先读取 `~/.tmux.conf` 这个文件。该文件中存放的就是对 Tmux 的配置。
小提示：如果你希望新的配置项能够立即生效，那么你可以将下面这一行配置加入到文件 ~/.tmux.conf 中。

```bash
# bind a reload key
bind R source-file ~/.tmux.conf ; display-message "Config reloaded.."
```
这样配置了之后，每当向 ~/.tmux.conf 文件中添加了新的配置，
只需要按下`<c-b> r`就可以重新加载配置并使新的配置生效，从而免去了开启一个新的会话。

### 参考链接
[tmux-archlinux](https://wiki.archlinux.org/index.php/tmux)
[vim-tmux-frendly used](https://gist.github.com/anonymous/6bebae3eb9f7b972e6f0)
[Tmux 速成教程：技巧和调整](http://blog.jobbole.com/87584/)
[tmux使用简介-保存远程服务器工作现场](http://www.tuicool.com/articles/nq6Rby)

### Common Usage

#### Installation

> Fortunately installing tmux is pretty straightforward on most distributions a simple 
> `sudo apt-get install tmux` (Ubuntu and derivatives) or `brew install tmux` (Mac) 
> should be sufficient.

#### Session Operation

【简单启动tmux】
```bash
tmux
```
> Session Handling
> If you’re done with your session you can either get rid of it by simply exiting all the panes inside or you can keep the session in the background for later reuse.

【创建会话】Creat Session
```bash
tmux new -s sessionname
```
【重命名会话】rename session
```
tmux rename-session -t 0 rnsession
```
【恢复会话】Attach Session
```
tmux attach -t 0
```
【detach会话】
```
<c-b> d
```
【选择并切换会话】
```
<c-b> s
```

#### Prefix key前缀

By default, tmux uses `<c-b>`

#### Panes Operation

竖直分割窗口`<c-b> %`
水平分割窗口`<c-b> "`
在分割窗口中导航`<c-b> <arrow key>`
关闭当前分屏`<c-b> x`
将当前panes置于新的窗口中 `<c-b> !`

#### Windows Operation

新建一个窗口`<c-b> c`
关闭当前窗口`<c-b> &`
重命名当前窗口，便于识别`<c-b> ,`
修改当前窗口编号，重编排`<c-b> .`
窗口切换`<c-b> <number>`
通过窗口列表切换窗口`<c-b> w`

窗口的数字可在bash状态栏的左下角查看，`*`号所在的窗口即为当前激活窗口
![](/hexo_blog/images/ubuntu-cmd-tmux-1.jpg)

* 查看会话信息`tmux ls`，
  其中，0和newsession 表示会话窗口名字，3表示该会话有3个窗口
    ```bash
    ywl@ywl-ThinkPad-E420:~$ tmux ls
    0: 3 windows (created Thu Oct 13 07:37:41 2016) [149x39]
    newsession: 1 windows (created Thu Oct 13 08:20:04 2016) [149x39]
    ```

#### Copy mode

* 进入copy mode `<c-b> [`
* 粘贴`<c-b> ]`
* 退出copy mode, vi mode : `q` ; emacs mode `Esc`

在进入了copy mode后，如果设置了vi模式，就可以像在vim中一样用j,k,h,l移动光标了
* 空格键开始选择文本，选定文本后回车键复制文本

【让复制文本的操作更像 Vim】
你还可以设置 Tmux 使用 v 键选择文本，用 y 键复制文本。为此只需要将下面的配置项加入到配置文件 ~/.tmux.conf 中。
```bash
# start selecting text typing 'v' key (once you are in copy mode)
bind-key -t vi-copy v begin-selection
# copy selected text to the system's clipboard
bind-key -t vi-copy y copy-pipe "reattach-to-user-namespace pbcopy"
```

##### 系统剪贴板操作 `shift`

> Tmux by default will "take over" text selection with a mouse, and trigger its own internal selection for use with its own cut and paste.
> 
> If you want to use the OS selection, so that you can use the OS cut and paste, hold down shift while selecting.
> 
> Then cut and paste will work normally.

可以按住`shift`键选择，右键复制粘贴，可用与ssh远程登录Tmux终端中

##### xclip 

Use system clipboard in vi-copy mode in tmux

【Installation】
```bash
sudo apt-get istall xclip
```

【setting】在`~/.tmux.conf`中加入下行

```bash
# y and p as in vim
bind Escape copy-mode
unbind p
bind p paste-buffer
# Copy to Clip
# <c-b> ]进入复制模式，v 开始选择，y复制文本进剪贴板
bind -t vi-copy 'v' begin-selection
# bind -t vi-copy 'y' copy-selection
bind -t vi-copy y copy-pipe "xclip -sel clip -i"
# Paste from Clip
# <c-b> <c-v>进行粘贴，思路是先将剪贴板的内容放进tmux的buffer中，然后粘贴buffer
bind C-p run "tmux set-buffer \"$(xclip -o -sel clipboard)\"; tmux paste-buffer"
```

Now when you copy some text, it will automatically be copied to OS clipboard.


#### Close Panes关闭panes

Either `exit` or `<c-d>`

### 进阶应用

Bash脚本启动

### Common Issues

#### ssh远程登录，vim中颜色显示不正常的解决方法

在`~/.bashrc`中建立别名规则
```
alias tmux="THEM=screen-256color-bce tmux"
```

在`~/.tmux.conf`中，设置默认终端类型
```bash
set -g default-terminal "xterm"
# 或者
set -g default-terminal "screen-256color"
```

在bash 中，执行以下命令让别名生效
```bash
$ source ~/.bashrc 
```

### 附录-tmux.conf

```bash
#Prefix is Ctrl-a
set -g prefix C-a
bind C-a send-prefix
unbind C-b

# Smart pane switching with awareness of Vim splits.
# See: https://github.com/christoomey/vim-tmux-navigator
is_vim="ps -o state= -o comm= -t '#{pane_tty}' \
    | grep -iqE '^[^TXZ ]+ +(\\S+\\/)?g?(view|n?vim?x?)(diff)?$'"
bind-key -n C-h if-shell "$is_vim" "send-keys C-h"  "select-pane -L"
bind-key -n C-j if-shell "$is_vim" "send-keys C-j"  "select-pane -D"
bind-key -n C-k if-shell "$is_vim" "send-keys C-k"  "select-pane -U"
bind-key -n C-l if-shell "$is_vim" "send-keys C-l"  "select-pane -R"
bind-key -n C-\ if-shell "$is_vim" "send-keys C-\\" "select-pane -l"


#Mouse works as expected
# scroll with your mouse wheel
setw -g mode-mouse on
set -g mouse-select-pane on
set -g mouse-resize-pane on
set -g mouse-select-window on

# Use vim keybindings in copy mode
set -g mode-keys vi
# scroll history
set -g history-limit 10000

# y and p as in vim
bind Escape copy-mode
unbind p
bind p paste-buffer
# <c-b> ]进入复制模式，v 开始选择，y复制文本进剪贴板
bind -t vi-copy 'v' begin-selection
# bind -t vi-copy 'y' copy-selection
bind -t vi-copy y copy-pipe "xclip -sel clip -i"
# <c-b> <c-v>进行粘贴，思路是先将剪贴板的内容放进tmux的buffer中，然后粘贴buffer
bind C-p run "tmux set-buffer \"$(xclip -o -sel clipboard)\"; tmux paste-buffer"

# moving between panes with vim movement keys
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

# 将r键设置为加载配置文件，并显示"reloaded!"信息  
bind r source-file ~/.tmux.conf \; display "Reloaded!"  

# For tmux coloer display
#set -g default-terminal "xterm"
set -g default-terminal "screen-256color"

#开启status-bar uft-8支持  
set -g status-utf8 on  

#######################################################################
#                            颜色设置                                 #
#######################################################################

#设置pan前景色  
set -g pane-border-fg green  
#设置pane背景色  
set -g pane-border-bg black  
#设置活跃pane前景色  
set -g pane-active-border-fg white  
#设置活跃pane背景色  
set -g pane-active-border-bg yellow  
#设置消息前景色  
set -g message-fg white  
#设置消息背景色  
set -g message-bg black  
#设置消息高亮显示  
set -g message-attr bright  
#设置status-bar颜色  
set -g status-fg white  
set -g status-bg black  
#设置窗口列表颜色  
setw -g window-status-fg cyan  
setw -g window-status-bg default  
setw -g window-status-attr dim  
#设置当前窗口在status bar中的颜色  
setw -g window-status-current-fg white  
setw -g window-status-current-bg red  
setw -g window-status-current-attr bright  
#设置status bar格式  
set -g status-left-length 40  
set -g status-left "#[fg=green]Session: #S #[fg=yellow]#I #[fg=cyan]#P"  
set -g status-right "#[fg=cyan]%d %b %R"  
set -g status-interval 60  
set -g status-justify centre  
```
