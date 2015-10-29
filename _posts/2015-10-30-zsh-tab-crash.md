---
layout: post
title: zsh在Mac上tab自动补全不起作用的解决办法
description: "oh-my-zsh tab auto-complete crash solution"
modified: 2015-10-30
tags: [Mac]
comments: true
---

这种问题一般是因为装了autojump才导致的，这里推荐使用z，因为oh my zsh自带这个插件，只需要启用即可。

言归正传，对于tab自动补全crash的解决，需要执行以下的命令：

{% highlight html %}
autoload -U compinit && compinit
{% endhighlight %}

如果重启以后又不工作的话，把这条命令加入到 ~/.zshrc 中。