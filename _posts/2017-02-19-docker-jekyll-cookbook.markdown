---
layout:            post
tittle:            “使用Docker快速搭建jekyll个人博客”
subtitle:          "Using Docker to build a Jekyll blog website. It's easy but awesome to a beginner."
date:              2017-02-19
author:            "Luke"
header-img:        "/img/"
catalog:           true
tags:
    -Cookbook
    -Docker
---

>学习docker有些日子啦，本文是一篇特别容易上手的cookbook指南。<br>
使用docker的容器技术，可以很好的规避linux配置的繁琐与组件版本号搭配各种坑。安装好docker后，快速搭建出一个优雅的jekyll服务器，biu出你的个人博客网站。


# UML图示：

## 1. Jekyll服务器的容器结构示意图：

![](/img/in-post/post-cookbook/docker-jekyll/jekyll_server.png)

## 2. Apache服务器容器结构示意图：

![](/img/in-post/post-cookbook/docker-jekyll/apache_server_container.png)

## 3. 流程图

![](/img/in-post/post-cookbook/docker-jekyll/system_flow.png)

# 涉及到的Docker知识

* 编写Dockerfile
* pull取Docker镜像
* Docker卷的创建、挂载与共享
* 创建守护式进程Docker容器
* 绑定Docker容器的端口与宿主机的端口之间的通信

## First Step: Jekyll基础镜像

```Dockerfile
FROM ubuntu:latest
MAINTAINER luke luke.bei.2015@gmail.com

RUN apt-get -yqq update
RUN apt-get -y install gcc ruby ruby-dev make nodejs
RUN gem install --no-rdoc --no-ri jekyll -v 2.5.3

VOLUME /data
VOLUME /var/www/html

WORKDIR /data
ENTRYPOINT ["jekyll", "build", "--destination=/var/www/html"]
~                                                                    
```
__注意：不要遗漏安装`gcc`和指定jekyll的版本号为`2.5.3`，否则报错。__

然后，使用该Dockerfile创建新的镜像——Jekyll基础镜像：

```shell
sudo docker built -t myJekyll .
```

## Second Step: Apache基础镜像

我这里使用的是docker hub上做好的apache镜像，快捷省心，直接pull下来就好了：

```shell
sudo docker pull eboraas/apache
```

## Third Step: 启动Jekyll镜像——容器运行产生博客网站文件

```shell
sudo docker run -v $(pwd):/data --name jekyll_server myJekyll
```

## Awesome Step: 启动Apache镜像——运行博客

```shell
sudo docker run -d -P --volumes-from jekyll_server --name luke_blog eboraas/apache
```
