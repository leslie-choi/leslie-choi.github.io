---
title: 五、vue-resource发起get和post以及jsonp请求
date: 2019-01-23 15:13:25
tags: basic
categories: Vue
---

# 步骤

1、导包，在vue.js后面再引入vue-resource.js，依赖于vue,需要注意先后顺序

2、get、post还有jsonp请求

在methods中定义三个函数

get请求

```javascript
getInfo() { // 发起get请求
    //  当发起get请求之后， 通过 .then 来设置成功的回调函数
    this.$http.get('http://vue.studyit.io/api/getlunbo').then(function (result) {
        // 通过 result.body 拿到服务器返回的成功的数据
        // console.log(result.body)
    })
}
```

post请求

```javascript
postInfo() { // 发起 post 请求   application/x-wwww-form-urlencoded
    //  手动发起的 Post 请求，默认没有表单格式，所以，有的服务器处理不了
    //  通过 post 方法的第三个参数， { emulateJSON: true } 设置 提交的内容类型 为 普通表单数据格式
    this.$http.post('http://vue.studyit.io/api/post', {}, { emulateJSON: true }).then(result => {
        console.log(result.body)
    })
}
```

jsonp请求

```javascript
    jsonpInfo() { // 发起JSONP 请求
        this.$http.jsonp('http://vue.studyit.io/api/jsonp').then(result => {
        console.log(result.body)
        })
    }
```

