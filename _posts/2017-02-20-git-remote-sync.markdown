---
layout: post
title: "在Linux服务器上搭建Git远程仓库"
subtitle: "Setup Remote Repository on Linux."
author: "Luke"
header-img: "img/in-post/post-cookbook/post_bg_git_remote.jpg"
date: 2017-02-20
catelog: true
tags:
    - Cookbook
    - Git
---

>本文的场景：开发PC想把源代码同步到自己的服务器中，未操作前，服务器不含有相关git仓库，但已经包含了git基础软件

# 服务器端

## 配置git

### 添加git用户

```shell
sudo git useradd -m git
```

### 设置密码

```shell
sudo passwd git
```

### 建立git仓库


这里有两个方式：

* 在git用户目录下建立相关仓库
* 在其他用户目录下建立相关仓库

```
mkdir demo_repositpry
cd demo_repositpry
git init demo_repositpry
git config receive.denycurrentbranch ignore
```
>设置receive.denycurrentbranch为ignore是为了实现从开发者PC接受推送的源代码。

本文采用的是后者，因为我的许多软件都装在了自身用户之中，放在git用户里将来权限是一个问题，每次都加sudo感觉不爽，反正不能忍。但为了保证安全，使用`chown`命令改变仓库的用户和用户组（如果有git用户组）为git就可以了。

```shell
sudo chown git:git -R demo_repositpry
```

> 注意：进入仓库内，随便建立一个文件提交一次，比如`.gitignore`文件，这样做的目的是防止在此空仓库目录的后续操作出现 “fatal: This operation must be run in a work tree” 报错。非空仓库目录下不需要此操作。

```shell
cd demo_repositpry
touch .gitignore
git add .
git commit -m "first commit"
```
>注意：commit的message最好写成英文，虽然github支持中文，但自己配置的git服务器使用中文的message会有push失败的可能。

当当当，初始化仓库完成。当然，如果服务器本身包含源代码仓库，只是想在开发者的PC上clone下来的话，方便日后push和pull同步，可以跳过以上步骤。

# 开发者PC

## 配置git

### 方式1：从服务器上clone源代码仓库

```shell
git clone git@domin_name/ip_address:/home/luke/demo_repositpry
```

输入密码完成认证就可以clone过来了，以后继续开发然后保持与服务器的同步，--bingo！完成。

### 方式2：本地源代码push到服务器上远程仓库

>这里假设demo_repositpry的文件夹里包含了项目源代码

```shell
git init demo_repositpry
cd demo_repositpry
git add .
git commit -a -m "commit info"
git remote add server_name/(建议origin)  git@domin_name/ip_address:/home/luke/demo_repositpry
                                  ==>比如git@202.111.0.23:/home/git/demo_repositpry
git push server_name master
```
此时，应该提示，输入服务器端git用户的密码，完成验证后就可以看到正在传送中。。。

传送完成后，我们需要在服务器端进行后续的设置。

### 再次进入服务器端

手动切换源代码仓库master分支到最近一次的commit。
```shell
git update-server-info
git checkout -f
```
此时，一个有觉(lan)悟(ai)的程序员希望看到，如果是web app应用场景，服务器端的git能够自动切换到最新的commit，以保持开发进度同步，如果还能够把提交的源代码自动同步到/var/www/html那自然是极好不过的了:d。此时就是git hook上场的时刻啦。

# 配置服务器端的git hook

是哒，我使用hook来自动化的完成push后的一些操作。比如挂载源代码带docker容器里，实现webapp的自动部署。

这里模拟一个使用场景：我们想给使用Docker创建的Apache容器提供最新的可运行源代码。那么，我们就可以在hooks文件夹中创建psot-receive：

```
demo_repositpry
              -> .git
                  -> hooks
                       -> post-receive


```

然后编写如下内容：

```shell
#!/bin/bash

APP_NAME=your_app_name
APP_DIR=$HOME/$APP_NAME

GIT_WORK_TREE=$APP_DIR git checkout -f

cd $APP_NAME
cp * /var/www/html
git push // 可选设置，如果连接着github
```

之后，本地PC每次push后就可以看到远程hook操作的提示。

# Links
* [Git简单应用：部署代码到服务器](http://blog.csdn.net/bencjl/article/details/53699684)
* [使用git做服务器端代码的部署](http://www.cnblogs.com/shaohuixia/p/5503521.html)
