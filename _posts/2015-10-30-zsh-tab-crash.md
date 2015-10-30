---
layout: post
title: zsh在Mac上tab补全不work的办法
description: "oh-my-zsh tab auto-complete crash solution"
modified: 2015-10-30
tags: [Mac]
comments: true
---

最近被这个问题折磨了两天。

因为之前装了autojump，后来又卸载了才导致的，这里推荐使用z，因为oh my zsh自带这个插件，编辑`~/.zshrc`文件，在`plugins=(git)`这行中加上`z`变成`plugins=(git z)`，然后`source ~/.zshrc`重新加载配置文件，就好了。

言归正传，对于tab自动补全crash的解决，需要执行以下的命令：

{% highlight html %}
autoload -U compinit && compinit
{% endhighlight %}

如果重启以后又不工作的话，把这条命令加入到 ~/.zshrc 中。