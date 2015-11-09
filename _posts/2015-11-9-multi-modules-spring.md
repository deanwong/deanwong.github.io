---
layout: post
title: 如何在multi-module的项目中配置spring
description: ""
modified: 2015-11-9
tags: [Java, Spring, Maven]
comments: true
---

小项目也有可能会分很多小的module开发，毕竟Maven早就支持了Aggregator Project。

{% highlight sh %}
project
|
|--web module
|--serviceA module
|--serviceB module
{% endhighlight %}

在Service比较复杂，比如封装了第三方接口的提供内部服务的情况下，我一般会单开一个module来管理代码。就算哪天这个第三方不用了，也不用去动主要业务代码。而且单独测试也容易的多。

那么想要利用Spring管理bean，比如在web module中注入一个serviceA module的一个服务，就需要给子module配置spring了。

原理来讲，就是让容器在加载主工程时(一般指web module)，可以读到每个子module的spring 文件。

在serviceA module的resources下新建一个`serviceAContext.xml`，内部配置跟一般的spring一样，在我的工程里采用了aop扫描package，所以我写了一段`aop:config`的配置，同时web module也有一段`aop:config`，不同点就是apo扫描的package目录不同，实测下来没有问题。

最后在web module中加载这个context文件即可。

{% highlight xml %}
<import resource="classpath*:serviceAContext.xml" />
{% endhighlight %}

简单吧。