---
title: ajax、axios、fetch的区别
date: 2019-08-21 11:43:20
tags: basic
categories: Ajax
---

**背景**

前端的技术发展速度非常的快，异步请求也是其重要的体现之一，从最早的原生XHR，再到JqueryAjax的统治时代，再到近来，fetch、axios等技术也开始出现并大量投入使用。

# 原生Ajax

Ajax是指一种**创建交互式网页应用**的网页开发技术，并且可以做到无需重新加载整个网页的情况下，能够更新部分网页，也叫作局部更新。是最早出现的发送后端请求技术，隶属于原始js中，核心是使用XMLHttpRequest对象，多个请求之间如果有先后关系的话，就会出现回调地狱。

**使用步骤：**

1. 创建XmlHttpRequest对象
2. 调用open方法设置基本请求信息
3. 设置发送的数据，发送请求（send）
4. 注册监听的回调函数（onreadystatechange）
5. 拿到返回值，对页面进行更新

**状态值：**

0	请求还未初始化，还未调用open()          ----未初始化
1	请求已建立但未发送，还未调用send()       ----载入
2	接受原始响应数据，为解析做准备            ----载入完成
3	正在解析数据                             ----解析
4	响应完成，数据解析完成                     ----解析完成

**优点：可以实现局部刷新，并且原生支持不需要使用任何插件**

**缺点：可能破坏浏览器后退功能，并且会出现回调地狱**

# Jquery Ajax

jquery的ajax就是在原生的ajax的基础上进行了封装，并且添加上了对JSONP的支持。

优点：

* 使用方便

缺点：

* 本身是针对mvc的编程模式，不太适合目前mvvm的编程模式
* 基于原⽣的 XHR 开发， XHR 本身的架构不清晰，已经有了 fetch 的替代⽅案 
* jQuery本身比较大，如果单纯的使用ajax可以自己封装一个，不然会影响性能体验
* 回调地狱

# axios

axios 是 vue 官方推荐使用的一个库，**axios 一个基于 Promise 用于浏览器和 nodejs 的 HTTP 客户端，本质上也是对原生 XHR 的封装，只不过它是 Promise 的实现版本，**不是原生js,使用时需要对其进行安装，符合最新的ES规范，它本身具有以下优点：

* 支持 Promise API
* 客户端支持防止CSRF
* 自动转换JSON数据
* 从 node.js 创建 http 请求
* 拦截请求和响应
* 从浏览器中创建 XMLHttpReques

PS:防止 CSRF :就是让你的每个请求都带一个从 cookie 中拿到的 key, 根据浏览器同源策略，假冒的网站是拿不到你 cookie 中得 key 的，这样，后台就可以轻松辨别出这个请求是否是用户在假冒网站上的误导输入，从而采取正确的策略。

# fetch

fetch 号称是 AJAX 的替代品，是在 ES6 出现的，使用了 ES6 中的 promise 对象。Fetch 是基于 promise 设计的。fetch 不是 ajax 的进一步封装，而是原生 js，没有使用 XMLHttpRequest 对象。

```javascript
fetch(url).then(fun2)
        .then(fun3)
        .....
        .catch(fun)
```

优点：
* 解决回调地狱
* 使用起来更加简洁

缺点：
* API 偏底层，需要封装
* 默认不带 Cookie，需要手动添加
* 浏览器支持情况不是很友好，需要第三方的ployfill
* fetch 没有办法原⽣监测请求的进度，⽽XHR可以

# 总结

ajax 是最早出现发送后端请求的技术，属于原生 js 范畴,核心是使用 XMLHttpRequest 对象,使用较多并有先后顺序的话，容易产生回调地狱。

fetch 号称可以代替 ajax 的技术，是基于 es6 中的 Promise 对象设计的，参数和 jQuery 中的 ajax 类似，它并不是对 ajax 进一步封装，它属于原生 js 范畴。没有使用 XMLHttpRequest 对象。

axios 不是原生 js,使用时需要对其进行安装，客户端和服务器端都可以使用，可以在请求和响应阶段进行拦截，基于 promise 对象。
axios 既提供了并发的封装，也没有 fetch 的各种问题，而且体积也较小，当之无愧现在最应该选用的请求的方式。这大概就是目前官方推荐使用 axios 的原因了吧。

# 参考文章：

[ajax、fetch、axios区别](https://blog.csdn.net/jennyya/article/details/83687622)
[ajax和axios、fetch的区别](https://www.jianshu.com/p/8bc48f8fde75)