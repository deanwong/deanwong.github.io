---
layout: post
title: vuejs前端开发的一些小坑
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

![建立后就是如图所见的结构](images/14732664332786.jpg)

下面列一些遇到的坑

#### 1.怎么使用第三方的JS，比如moment
我在项目里面使用了`moment`做一些时间处理，如果载入这个`moment.js`，有两个选择
*  传统的写法：在index.html

```html
<script src=".."></script>
```
*  Vue的写法：因为moment也是npm install的，所以在js的入口文件main.js写

```javascript
window.moment = require('moment')
// 任何地方都可以
window.moment.now()
```

#### 2.如何集成Less
需要less-loader

```
npm install less-loader less --save-dev
```
在一个vue文件里可以这样写

```javascript
<style lang="less">
  @import './assets/less/main.less';

  .header-box {
    z-index: 100;
    position: absolute;
    width: 100%;
    left: 0;
    top: 0;
  }
 </style>
```
当然也可以加个scope，表示只在这个vue生效，不会污染到其他的component


```javascript
<style scoped lang="less">
```

#### 3.图片放在哪儿
放在static目录下

```css
background-image: url("/static/images/slider-business-01.jpg");
```
这样引用就可以。

#### 4.如果调用需要token的API
可以使用vue的http.interceptor


```javascript
Vue.http.interceptors.push((request, next) => {
        // ...
        // 可以把Token放到Header里
        // ...
    next((response) => {
        // ...
        // 请求发送后的处理逻辑
        // ...
        // 根据请求的状态，response参数会返回给successCallback或errorCallback
        return response
    })
})
```

#### 5.登陆超时的调整
在call一个service API时，如果超时了，一般会返回一个401或者其他的http code，我们可以通过写一个拦截器来统一处理。

```javascript
Vue.http.interceptors.push((request, next) => {
next((response) => {
    if (response.code === 401) {
      Vue.$vux.alert.show({
        title: '登陆超时',
        content: '点击回到登陆页',
        onHide () {
          window.location.href = '/login'
        }
      })
    }
    return response
  })
})
```
通过这样的一个拦截器就不用把这种代码写在每一个http请求方法里了。还有很多细节可以参考我当时学习的[blog](http://www.cnblogs.com/keepfool/p/5657065.html)

#### 6.调用API时的Loading怎么做
其实还是用interceptor，通过修改isLoading这个变量的值来控制是否显示菊花图。
那么问题来了，isLoading是跨component的，这个时候就需要用到[vuex](https://github.com/vuejs/vuex)了，这是一个应用级别的状态机。
我在`App.vue`中放置一个loading图标

```html
<loading :show="isLoading" position="absolute"></loading>
```
这时候isLoading从vuex中获得

```javascript
vuex: {
      getters: {
        isLoading: (state) => state.isLoading
      }
    }
```
`store.js`里也定义这个state

```javascript
const state = {
  isLoading: false
}
export default new Vuex.Store({
  state
})
```
剩下的就简单了，在interceptor里控制state.isLoading的值

```javascript
Vue.http.interceptors.push((request, next) => {
  // 显示loading
  commit('UPDATE_LOADING', true)
  next((response) => {
    // 不显示loading
    commit('UPDATE_LOADING', false)
    return response
  })
})
```

#### 7.route data hook VS. ready
这个其实有各种各样的讨论，我个人经过实践觉得用route里的data勾子是比较好的，因为从生命周期角度说，data勾子比ready要早。所以一般组件初始化是在data勾子之后，但可能是比ready要早的。某些情况下，组件初始化就需要数据，这种时候大多用data hook。

#### 8.computed有啥用
挺有用的，特别是一些情况判断多的场景下，需要自己揣摩。

#### 9.mount local module (vux)
因为我是用了vux这套组件画UI，而vux是我fork的一个项目，所以就没有用`npm install`来安装，而是直接用本地的版本。
用npm link这个函数

```bash
cd ~/projects/vux    # go into the package directory
npm link                    # creates global link
cd ~/projects/myproject   # go into some other package directory.
npm link vux              # link-install the package
```
这样在myproject就安装了vux了，跟`npm install vux`的效果一样。

#### 10.SPA nginx 404
最后写一个部署的问题，因为SPA这个项目其实都是以`index.html`为入口的，所以如果在nginx配置了一个如 `domain/login` 这样的URL，是会报404的，因为没有login.html。

```nginx
server {
    listen       8000;
    root /h5/dist;
    location / {
        try_files $uri /index.html =404;
    }
    location ^~ /api {
        proxy_pass   http://127.0.0.1:8080;
    }
    
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}
```
所以最重要的就是那一句`try_files`，一旦找不到相应的文件，就把流量引到`index.html`。这时候页面跳转其实是通过`vue-route`实现的，跟nginx就没关系了。就类似WordPress的请求都应该引到`index.php`一样。








