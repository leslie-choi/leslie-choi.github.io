---
title: 十一、使用render函数渲染组件
date: 2019-01-24 17:56:12
tags: basic
categories: Vue

---

# render函数基本使用

render函数与methods平级，使用语法

```javascript
  render: function (createElements) { 
  // createElements 是一个 方法，调用它，能够把 指定的 组件模板，渲染为 html 结构
    return createElements(login)
    // 注意：这里 return 的结果，会 替换页面中 el 指定的那个 容器
  }
```

这里的login是自己定义的一个子组件

要注意的是，平时在components中注册组件，然后将该标签放入#app中，并不会覆盖里面的其他内容，而使用render函数，则会覆盖掉页面中el指定的那个容器,即容器内其他标签不起作用

```javascript
<div id="app">
    <p>33333</p>
    <login></login>
</div>
```

