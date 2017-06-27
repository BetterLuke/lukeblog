---
layout:     post
title:      "Tmux使用手账"
subtitle:   "Tmux is a amazing terminal multiplexer. For a Linux beginner, use it and make my life better."
date:       2017-06-11
author:     "luke"    
header-img: "img/home-bg-art.jpg"
catalog: true
tags:
    - Linux
---

# tmux折腾小记

自从用了WSL这个神奇的东东，就可以优雅的在windows下使用Linux了，自己对于WSL的使用场景还在摸索中，偶尔使用grep，sed，perl练习正则什么的，还有就是git。就是因为知晓了tmux，感觉自己以后如果有ssh服务器类似的任务的话，就可以安心的在windows下干了，毕竟多窗口终端对于此类任务是刚需。

就像图片中看到的那样，默认的bash页面简直是简陋至极：

![](\img\in-post\post-personal-tmux\ugly-cmd.png)

在这里，我不吐槽UI的骨感，只是没有类似原生linux中gnome或者kde多窗口终端页面，对于复杂的终端任务，使用起来就颇有不爽。在WSL中安装tmux就可很好实现多终端窗口的效果，上手体验后，大呼神器。

其中，多窗格可以实现一屏多终端，还有，多窗口的功能类似于桌面系统的虚拟桌面，可以很好的实现负空间理念，更加有效的提高了终端的交互性。还有，会话(session)功能感觉目前还用不着，没有摸索。

整体的一个框架图就是：

![](\img\in-post\post-personal-tmux\tmux-structure.png)

对于tmux，自己的本次掌握的主要是两点：

* 理解tmux的配置文件`.tmux.conf`
* 掌握主要的快捷键

## 实用的tmux配置文件

```bash
#-- base --#

set -g default-terminal "screen-256color"
set -g display-time 3000
set -g history-limit 65535
set -g mouse-select-pane on

#-- bindkeys --#

set -g prefix C-a
unbind C-b
bind a send-prefix

unbind '"'
bind - splitw -v
unbind %
bind | splitw -h

bind k select-pane -t -U
bind j select-pane -t -D
bind h select-pane -t -L
bind l select-pane -t -R

bind C-k resize-pane -U 10
bind C-j resize-pane -D 10
bind C-h resize-pane -L 10
bind C-l resize-pane -R 10

bind C-u swap-pane -U
bind C-d swap-pane -D

bind C-e last
bind q killp

bind '~' splitw htop
bind ! splitw ncmpcpp
bind m command-prompt "splitw 'exec man %%'"
bind @ command-prompt "splitw 'exec perldoc -t -f %%'"
bind * command-prompt "splitw 'exec perldoc -t -v %%'"
bind % command-prompt "splitw 'exec perldoc -t %%'"
bind / command-prompt "splitw 'exec ri -T %% | less'"

#-- statusbar --#

set -g status-right "#[fg=green]#(/usr/bin/uptime)#[default]  #[fg=green]#(cut -d ' ' -f 1-3 /proc/loadavg)#[default]"
set -g status-right-attr bright

set -g status-bg black
set -g status-fg yellow
setw -g window-status-current-attr bright
#setw -g window-status-current-bg red
#setw -g window-status-current-fg white

set -g status-utf8 on
set -g status-interval 1

#set -g visual-activity on
#setw -g monitor-activity on

setw -g automatic-rename on

set -g status-keys vi
setw -g mode-keys vi
```

贴上使用上述配置文件后，更换了prefix的默认组合`Ctrl-b`(需要两个手指，一点都不人工学好不好)为`Ctrl-a`；优化了一些快捷键；美化了statusbar； 最后：WSL的bash就变成了这个样子：

![](\img\in-post\post-personal-tmux\wsl-tmux.png)

> ALT + ENTER 可以让CMD进入全屏模式

## 快捷键常备：

* 复制文本流程：`prefix-[` ==> 进入COPY-MODE ==> 'space空格键启动选中' ==》 `hjkl移动选你所需` ==> `ENTER完成复制并自动退出了COPY-MODE`

* 粘贴：`prefix-]` ==> 粘贴buffer的内容的到光标处

* `tmux show-buffer`：打印出缓存器里最新的内容

* `prefix-#`: list buffer

* `prefix-=`: list buffer plus 可以选中内容的index，然后打印出来

* `prefix-c`: 创建新的窗口，默认名字为一个数字，如果自己想修改名字，可以在终端键入`tmux rename-window -t target_window_name target_window_newname`.

* `prefix-,`: 修改当前窗口的名字。

其他的自己还不熟，如果有需要，就自己查查`man tmux`吧。


# 问题来了

好吧，外貌的问题是解决了，可是在使用的过程中便发现了新的问题：在tmux的面板里，复制的内容不能同步在系统的粘贴版里，而且这个问题不仅仅出现在WSL这样的环境里，像Linux和MacOS都有出现。
看网上资料说，tmux使用了类似vim独立寄存器的原理，具有粘贴版，所以就有了系统的粘贴版和tmux粘贴版是相互隔离的，也就是说，在tmux的copy-mode里复制的内容，只是存在目前的bash运行环境里，不能逃出CMD窗口。

# 解决复制文本这个尴尬的问题

然后，我就在想，要是以后需要复制大段的代码什么的，岂不会现场发难，与其担忧未来出问题，还不如现在就着手解决问题。

经过各种谷歌，在github的某个项目issue里找到了灵感：

* 单个pane情况：如果当前pane是全屏状态，就可以毫无痛点的使用鼠标选择复制，而且还支持CMD的quickedit模式，所以就不存在问题。
* 多pane情况：在多pane的模式下，你就会发现，上面的那种方法就不行了，因为此时的tmux正进入到类似于vim多窗格模式，所有的复制内容都会进入到独立的寄存器中。好吧，解决的方法就是：首先，使用prefix-z使当前的pane最大化为全屏幕，然后按住‘Shift’,就可以激活鼠标复制模式，然后就可以复制自己需要的内容啦，此时，你所复制的内容就会存在于系统的寄存器里。反之向tmux的面板里粘贴东西，就是同样的按住‘Shift’,单击鼠标的右键就可以复制当前剪贴板的文件到tmux的面板中。

至此，一个比较完美的WSL下终端多窗口的方案就是这个样子。

保持分享精神：

![](\img\in-post\post-personal-tmux\github-tmux-issue.png)

# 让终端与Tmux一起启动
很快就发现，如果每次使用的开始都需要在终端输入`tmux`才能激活使用，感觉会很麻烦，最好能够让Tmux和终端一起启动，解决办法就是在你的`.bashrc`文件里添加如下脚本：

```bash
if [[ "$TERM" != "screen-256color" ]]
then
    tmux attach-session -t "$USER" || tmux new-session -s "$USER"
    exit
fi
```
退出，重新打开终端就可以啦！

# 参考链接
* [Tmux 速成教程：技巧和调整](http://blog.jobbole.com/87584/)
* [Windows Subsystem for Linux(WSL)的配置](https://memeda.github.io/%E6%8A%80%E6%9C%AF/2016/08/03/WslBashOnWindowsConfig.html)
: 使用ln创建链接至windows用户目录的思路很赞。
* [一本很实用Tmux的书](https://www.w3cschool.cn/tmux/tji9dozt.html)
