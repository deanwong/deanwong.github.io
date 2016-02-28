---
layout: post
title: 淘宝旅行的日历控件Angular化
description: "datepicker angular"
modified: 2016-02-28
tags: [Angular]
comments: true
---

自己做项目需要一个日历控件，网上搜了半天觉得淘宝旅行的日历最合适

* 起始和结束两个日历控件，并且可以联动
* 支持国内的节假日显示
* 有各类option支持不同的需求，比如三个月时间，结束时间必须在起始时间七天后等等。可定制化程度高。

唯一的问题就是这个控件是基于Kissy的，而我这个练习项目是Angular，那么为了训练下directive的写法，决定把这个控件angular化。

这是测试的结果
![datepicker](https://raw.githubusercontent.com/deanwong/ngDatepicker/master/snapshot1.png)

基本符合要求。

源码已经上传到[Github](https://github.com/deanwong/ngDatepicker)
其中最关键的一点就是directive里的transclusion，我对transclusion的理解还很浅显，看代码:

{% highlight html %}
<datepicker start-date="J_CheckIn_List" ng-start-model="main.arrivalDate" end-date="J_CheckOut_List"
               ng-end-model="main.departureDate">
  <div class="search_item">
    <label>FROM</label>
    <input type="text" id="J_CheckIn_List" placeholder="yyyy-mm-dd"/>
  </div>
  <div class="search_item">
    <label>TO  </label>
    <input type="text" id="J_CheckOut_List" placeholder="yyyy-mm-dd"/>
  </div>
</datepicker>
{% endhighlight %}

datapicker标签中间有一坨原始html，这里的transclusion就是让这坨代码直接输出而不被replace掉。


当然这个控件的核心还是人家[淘宝旅行的](http://fgm.cc/learn/calendar/trip-calendar/demo1.html)

PS:

备忘一下JVM导入密匙的代码
先通过浏览器下载证书，执行
{% highlight bash %}
sudo keytool -import -alias "elongAPI" -keystore /Library/Java/JavaVirtualMachines/jdk1.7.0_75.jdk/Contents/Home/jre/lib/security/cacerts -file /Users/wangding/Downloads/api.elong.com.cer
{% endhighlight %}