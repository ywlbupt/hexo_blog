title: ubuntu-IDE~vim与Terminal的solarized配色
date: 2016-11-18 02:19:07
category: 
tags:
description:
---
[Toc]

参考文档：[Using Solarized colors with vim, Eclipse, and Ubuntu](http://www.xorcode.com/2011/04/11/solarized-vim-eclipse-ubuntu/)

## 终端与Vim的配色问题思考

注意 vim 的终端配色与 GUI 配色是完全不同的，一款 GUI 的配色方案有可能根本不含有终端配色方案。

<!-- more -->

这类问题可以试着这样考虑：

首先，它是否支持终端配色，如果支持，支持什么形式的终端配色？
如果是支持 256 色的终端配色，通常来说要求终端被手动配置为 256 色模式。
如果是支持 16 色的终端配色，通常要求终端仿真软件本身载入一款针对该终端的调色板。我看到他的官方文档只支持 XTerm， putty ，iTerm2，苹果终端这四种终端的预置调色板。所以如果你使用其他的终端软件，要么使用 256 色终端模式，要么就需要在终端软件内自定义配色方案。

解决了上面一个问题之后，再来考虑底色问题。
底色问题，要看这个配色方案是强制指定终端底色否将终端底色设定为 NONE，这种情况下是透明底色，也就是说，底色取决于你终端本身设定的底色，你需要到终端的背景色设定中直接设定底色。

如果你希望进入 vim 之后是另外一种底色，退出 vim 之后是终端背景，那么就必须选择有终端底色设定的配色方案。从目前 solarized 公布的信息上看，我看不出他含有哪些方案。所以你可能需要自己研究一下它的代码。

[链接-作者：pansz](https://www.zhihu.com/question/51713545/answer/130671176)


## Vim Vundle安装 Solarized 

[Github 库地址](https://github.com/altercation/vim-colors-solarized)

vimrc中的配置如下，供参考：

``` vim
syntax enable
set background=dark
set t_Co=16
let g:solarized_termcolors=16'
let g:solarized_termtrans = 1
colorscheme solarized
```


`~/.bashrc`中添加下行
``` bash
export TERM=xterm-256color
```

## 终端使用Solaried 16 配色

1. 在主用户目录下，新建一个文件`solarize.sh`，见附录A [附录A](#solarizedsh)
2. 赋予`solarize.sh`执行的权限，`chmod +x solarize.sh`
3. 执行命令`./solarize.sh dark`配置终端颜色为16色solaried



## 附录A
<span id="solarizedsh">附录A</span>

``` bash
#!/bin/sh
#
# Shell script that configures gnome-terminal to use solarized theme
# colors. Written for Ubuntu 11.10, untested on anything else.
#
# Solarized theme: http://ethanschoonover.com/solarized
# 
# Adapted from these sources:
# https://gist.github.com/1280177
# http://xorcode.com/guides/solarized-vim-eclipse-ubuntu/
 
case "$1" in 
    "dark")
        PALETTE="#070736364242:#D3D301010202:#858599990000:#B5B589890000:#26268B8BD2D2:#D3D336368282:#2A2AA1A19898:#EEEEE8E8D5D5:#00002B2B3636:#CBCB4B4B1616:#58586E6E7575:#65657B7B8383:#838394949696:#6C6C7171C4C4:#9393A1A1A1A1:#FDFDF6F6E3E3"
        BG_COLOR="#00002B2B3636"
        FG_COLOR="#65657B7B8383"
        ;;
    "light")
        PALETTE="#EEEEE8E8D5D5:#D3D301010202:#858599990000:#B5B589890000:#26268B8BD2D2:#D3D336368282:#2A2AA1A19898:#070736364242:#FDFDF6F6E3E3:#CBCB4B4B1616:#9393A1A1A1A1:#838394949696:#65657B7B8383:#6C6C7171C4C4:#58586E6E7575:#00002B2B3636"
        BG_COLOR="#FDFDF6F6E3E3"
        FG_COLOR="#838394949696"
        ;;
    *)
    echo "Usage: solarize [light | dark]"
    exit
    ;;
esac
 
gconftool-2 --set "/apps/gnome-terminal/profiles/Default/use_theme_background" --type bool false
gconftool-2 --set "/apps/gnome-terminal/profiles/Default/use_theme_colors" --type bool false
gconftool-2 --set "/apps/gnome-terminal/profiles/Default/palette" --type string "$PALETTE"
gconftool-2 --set "/apps/gnome-terminal/profiles/Default/background_color" --type string "$BG_COLOR"
gconftool-2 --set "/apps/gnome-terminal/profiles/Default/foreground_color" --type string "$FG_COLOR"
```
