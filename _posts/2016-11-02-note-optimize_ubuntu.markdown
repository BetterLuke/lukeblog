---
layout:     post
title:      "实用的ubuntu优化手记——系统清理，UI美观提升，zsh终端配置"
subtitle:   "A System Performance Optimization Guide for Ubuntu. Make yours linux more clever, beautiful and effective."
date:       2016-11-02
author:     "luke"    
header-img: "img/post-bg-unix-linux.jpg"
catalog: true
tags:
    - Operations
    - CookBook
---

>记录一下自己对于ubuntu的优化：

## 系统清理篇

### 卸载libreOffice

```shell
sudo apt-get remove libreoffice-common  
```

### 删除Amazon的链接

```shell
sudo apt-get remove unity-webapps-common
```

### 删除不常用的软件

```shell
sudo apt-get remove thunderbird totem rhythmbox empathy brasero simple-scan gnome-mahjongg aisleriot

sudo apt-get remove gnome-mines cheese transmission-common gnome-orca webbrowser-app gnome-sudoku  landscape-client-ui-install  

sudo apt-get remove onboard deja-dup
```

__注意：__ 删除以上软件后，在使用apt-get安装一些其他软件的时候，比如git，会出现无法找到一些包的问题，此时使用`sudo apt-get -f install`就会自动安装下载缺失的包。

## 主题美化篇

![]()

### unity-tweak-tool

```shell
sudo apt-get install unity-tweak-tool
```

对于低版本的ubuntu,有时会出现“schema is missing”导致unity-tweak-tool无法启动，这是 因为缺少相应的组件，此时安装相应的组件就好了:

```shell
sudo apt-get install unity-webapps-common
```

### Flatabulous主题

![](/img/in-post/post-cookbook/ubuntu-beautiful/c-01.png)

Flatabulous是一个美好的词汇。所以如图所示，这套theme真的很美，丑陋的ubunu的真应该好好感谢它，默认的紫色主调背景色真的丑瞎眼，感觉大部分高亮色都不是很适合做长期编码工作者的背景色，毕竟很累眼睛好不好。好吧，敲击几下见键盘安装这个扁平化风格的主题：

```shell
sudo add-apt-repository ppa:noobslab/themes
sudo apt-get update
sudo apt-get install flatabulous-theme
```

该主题有配套的图标，安装方式如下：

```shell
sudo add-apt-repository ppa:noobslab/icons
sudo apt-get update
sudo apt-get install ultra-flat-icons
```

安装完成后，打开unity-tweak-tool软件，修改主题和图标：

* 进入Theme，修改为Flatabulous

* 在此界面下进入Icons栏，修改为Ultra-flat

## 字体
下载文泉译微米黑字体替代，效果会比较好，毕竟是国产字体！

```
sudo apt-get install fonts-wqy-microhei
```

然后通过unity-tweak-tool来替换字体。



## 终端

### 安装zsh：

1. `sudo apt-get install zsh`安装zsh
2. `zsh --version`确认是否安装成功，或者查看系统当前shell`echo &SHELL`
3. `sudo chsh -s /bin/zsh`设置zsh为默认shell
4. 注销重新登录

__注意：__ 如果无法完成修改，就在命令行输入`chsh`手动修改它的值为`/bin/zsh`:



设置完成之后，终端变成如下样式：

![](http://upload-images.jianshu.io/upload_images/76130-7ba784779a598ebe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 安装Oh-My-Zsh:

`wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh`

### 美化终端
#####  1. 省略用户名与系统名：
编辑`./zshrc`添加: `DEFAULT_USER='luke'`

##### 2. 更改主题

编辑`./zshrc`修改： `ZSH_THEME='agnoster'`

可是会显示乱码，这是因为字体没有匹配好，需要下载支持Powerline的字体：

* [下载的地址](https://github.com/powerline/fonts/blob/master/UbuntuMono/Ubuntu%20Mono%20derivative%20Powerline.ttf)
* 下载完成双击安装

打开unity-tweak-tool，设置Monospacefont为刚刚下载的字体，就可以完美支持美化终端中的状态栏了。

效果如图：

![](http://upload-images.jianshu.io/upload_images/76130-9007771f3a1bdc95.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
