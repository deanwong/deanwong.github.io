---
layout: post
title: 拿vue做前端的一些小坑
description: "Vue.JS development tips"
modified: 2015-09-07
tags: [JavaScript, Vue]
comments: true
---

近一个月拿Vue.JS做一个小项目练手。项目本身的server API是现成的，而且都是json格式，所以前端选型自然会选择MV*M的框架。由于做了一段时间的angular，感觉对于这个工期很短的项目angular显得有些庞大了。所以尝试了一下Vue.JS。

一开始上手确实极不适应，跟angular的思想有很多不一样的地方。
1. 最不习惯的就是component的概念，没有了page的概念，所有东西都是component，这个有点像React。
2. 不像angular管的那么宽，router，http之类的都要自己加library。
3. 构建用的是webpact，加上less-loader就可以自动build less代码，这一点很赞。安装好vue-cli之后`vue init`就可以建立一套脚手架。

下面列一些遇到的坑

#### 怎么使用第三方的JS，比如moment

#### 如何集成Less


#### 图片放在哪儿

#### 如果调用需要token的API

#### 登陆超时的调整

#### 调用API时的Loading怎么做

#### data route VS. ready

#### computed有啥用

#### mount local module (vux)

#### SPA nginx 404







