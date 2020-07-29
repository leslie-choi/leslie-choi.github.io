---
title: 二、Vue过滤器
date: 2019-01-22 17:23:35
tags: basic
categories: Vue
---

# Vue的过滤器

1、全局过滤器

```
`语法Vue.filter('过滤器名',function(传参){})`

```

2、局部过滤器

```javascript
new Vue({
    el:'#app',
    data:{},
    method:{}
    filters:{
        //定义私有过滤器，调用的时候使用就近原则
    }
})
注意全局的过滤器不需要带s，而私有的需要带上s

调用过滤器的方法

`{{字段名 | 过滤器名}}`

`例：{{item.ctime | dateFormat() }}`
```

