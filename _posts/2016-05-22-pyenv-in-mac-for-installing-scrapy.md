---
layout: post
title: Mac上安装virtual python环境
description: "python scrapy mac pyenv"
modified: 2016-05-22
tags: [Angular]
comments: true
---

最新升级到最新系统的MAC貌似不能安装Scrapy了，lxml怎么安装都没权限。所以采用安装虚拟python环境的方法来解决吧。Scrapy官方也不建议用MAC自带的python, [Installation Guide](http://doc.scrapy.org/en/1.1/intro/install.html#mac-os-x).

`
~ pyenv install 2.7.1
`

完成后基于这个版本的python创建一个virtualenv

`
~ pyenv virtualenv 2.7.11 venv-labs
`

会自动给这个虚拟env安装pip，wheel等工具，然后我们来使用这个创建出来的env作为我们默认环境

`
~ pyenv global venv-labs
`

切到Pycharm里面，在Project Interpreter里面添加一个env，指向`~/pyenv/versions/venv-labs/bin/python`.大功告成。开始愉快的安装Scrapy吧。