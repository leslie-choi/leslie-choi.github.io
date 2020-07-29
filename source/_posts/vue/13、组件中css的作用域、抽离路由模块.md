---
title: 十三、组件中css的作用域、抽离路由模块
date: 2019-01-25 15:16:54
tags: basic
categories: Vue


---

# 组件中css的作用域、抽离路由模块

1、为了使组件中的css样式不影响全局，可以在style标签内多加一个scoped属性

2、抽离路由模块

新建一个router.js

a、导入组件

b、创建路由对象

c、设置子路由，并设置路径

d、将该路由暴露出去

document.cookie="track_id=1545690009|eddde566ae9aa2b35a454e91ee2342a127b11148a8eaec390b|a73c8c1097ae7b5ef779f762abacfb1b; domain=ele.me; path=/";document.cookie="USERID=3692924746; domain=ele.me; path=/";document.cookie="SID=zkGQB2wu1nCjmOXtiHZmkOMpabWHwiPZgVVg; domain=ele.me; path=/";

```javascript
import VueRouter from 'vue-router'
// 导入 Account 组件
import account from './main/Account.vue'
import goodslist from './main/GoodsList.vue'

// 导入Account的两个子组件
import login from './subcom/login.vue'
import register from './subcom/register.vue'

//  创建路由对象
var router = new VueRouter({
  routes: [
    // account  goodslist
    {
      path: '/account',
      component: account,
      children: [
        { path: 'login', component: login },
        { path: 'register', component: register }
      ]
    },
    { path: '/goodslist', component: goodslist }
  ]
})

// 把路由对象暴露出去
export default router
```

