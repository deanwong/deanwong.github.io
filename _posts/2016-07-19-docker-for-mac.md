---
layout: post
title: 使用新版Docker for MAC
description: "use docker for MAC"
modified: 2015-07-19
tags: [Mac, Docker]
comments: true
---

Docker官网出了最新的Docker For Mac.不是以前的基于virtual box的Toolbox了。内心小激动，赶紧下载安装。装完之后，menu bar上会有一个小鲸鱼的图标，可以restart，而且有GUI做一些额外配置，比如最需要的Proxy和文件mount。
![menu bar](/images/14689130500269.png)


阅读[Getting Start](https://docs.docker.com/docker-for-mac/)发现其实Docker for Mac也不是跟Linux一样使用内核原生的container。而是用了一个Mac内嵌的微型VM（HyperKit）替换了原来臃肿的virtual box。

### Docker for MAC vs. Docker toolbox

看到这儿肯定有人问，机器里怎么有两套docker？
确实，就是有两套，如果你装过Toolbox，这两套互不影响都可以work。一个基于virtual box，一个基于HyperKit。可以运行`docker-machine`看看自己是不是已经有了Toolbox了。官方也发布了一篇[《Docker for Mac vs. Docker Toolbox》](https://docs.docker.com/docker-for-mac/docker-toolbox/)，来阐述二者的区别。从我看来，Toolbox迟早会被弃用。

### Hello world
装完之后，必须try一把，上docker hub，发现Nginx也有基于alpine的版本了。速度pull。

```bash
docker pull nginx:alpine-stable
docker run -d -p:43210:80 --name webserver nginx:stable-alpine
```

访问本机的`http://localhost:43210`,可以看到欢迎页面了。

继续登录这个docker container。

```bash
docker exec -ti 0a41832c61af /bin/sh
```
因为alpine只有shell，我们可以装个bash来玩。

```bash
apk add bash
```
重新用bash进入container

```bash
docker exec -ti 0a41832c61af /bin/bash
```

Enjoy!



