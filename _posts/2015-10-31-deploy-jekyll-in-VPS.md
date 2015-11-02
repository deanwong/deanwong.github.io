---
layout: post
title: VPS部署jekyll
description: "jekyll VPS"
modified: 2015-10-31
tags: [博客, Blog]
comments: true
---

经过一天的折腾，终于实现在VPS上搭建一个基于jekyll的博客来替换wordpress。

Digital Ocean[^1] 的很多文章帮了大忙了
[^1]: <https://www.digitalocean.com>


### 1. 创建用户(optional)
如果觉得可以一直用root的话可以跳过这一步了。
这里是基本的Linux操作，直接贴链接。

[创建用户](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-centos-6)

### 2. 安装ruby
这一步其实也没啥，用rpm安装的ruby还是1.8的。两年前的东西实在太旧了，建议用rvm安装，可以版本切换。

[安装ruby](https://www.digitalocean.com/community/tutorials/how-to-install-ruby-2-1-0-on-centos-6-5-using-rvm)

### 3. 安装jekyll
来到最重要的一步了，大家都知道jekyll是用来编译博客的，所以在一篇博客写完后要把md文件放到VPS上，在执行`jekyll build`生成html的话就太麻烦了。所以得把我们的VPS打造的跟GitHub Pages那样，push之后自动build。
Digital Ocean这篇文章实在太赞，小白应该都能看得懂。

[用Git部署jekyll](https://www.digitalocean.com/community/tutorials/how-to-deploy-jekyll-blogs-with-git)

其中最重要的是那一段脚本，大家仔细研读。

{% highlight bash %}
#!/bin/bash -l
GIT_REPO=$HOME/repos/awesomeblog.git
TMP_GIT_CLONE=$HOME/tmp/git/awesomeblog
PUBLIC_WWW=/var/www/awesomeblog

git clone $GIT_REPO $TMP_GIT_CLONE
jekyll build --source $TMP_GIT_CLONE --destination $PUBLIC_WWW
rm -Rf $TMP_GIT_CLONE
exit
{% endhighlight %}

### 4. 安装nginx
胜利就在眼前，nginx安装很容易的，而且省资源，博主128mb的lowX的服务器也跑的呼呼地。
在Digital Ocean没找到centos6的就拿篇7的看看也一样。

[安装nginx](https://www.digitalocean.com/community/tutorials/how-to-set-up-nginx-server-blocks-on-centos-7)

#### 其他的一些幺蛾子

因为jekyll刚刚升级到3.0，所以出了一个幺蛾子。

最大的坑就是首页`paginater`的posts出不来，不得已就到VPS上去build，发现`jekyll-paginate`这个插件需要加入到`_config.yml`中去了。

build时会报error
> Deprecation: You appear to have pagination turned on, but you haven't included the `jekyll-paginate` gem. Ensure you have `gems: [jekyll-paginate]` in your configuration file.

{% highlight yaml %}
gems :
  - jekyll-paginate
{% endhighlight %}

重启build一把，ok。