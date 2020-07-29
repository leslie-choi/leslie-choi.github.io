---
title: 三、Vue自定义属性绑定
date: 2019-01-22 21:33:35
tags: basic
categories: Vue
---

# Vue的自定义属性绑定

1. 全局语法

使用Vue.directive()定义全局指令 v-focus

参数1是指令的名称，定义的时候指令的名称前面，不需要加 v-前缀，但是调用的时候需要要加上v-前缀调用

参数2是一个对象，在这个对象上有一些指令相关的函数，在特定阶段执行相关操作

```javascript
Vue.directive('focus',{
    //注意： 在每个 函数中，第一个参数，永远是 el ，表示 被绑定了指令的那个元素，这个 el 参数，是一个原生的JS对象
    bind:function(el){
        el.focus()
        // 每当指令绑定到元素上的时候，会立即执行这个 bind 函数，这个时候元素还没有被插			入到dom中去，只执行一次
    },
    inserted:function(el){
        // inserted 表示元素 插入到DOM中的时候，会执行 inserted 函数【触发1次】
        el.focus()
    },
    updated:function(el){
    // 当VNode更新的时候，会执行 updated， 可能会触发多次
    }
}
```

2. 通过指令得到自定义参数值

```javascript
Vue.directive('color',{
    bind:function(el,binding){
        el.style.color = binding.value
    }
    //上面的binding是形参，可以自定义，但是需要上下一致
    //使用value取出来的值是green，使用expression取出来的值是'green'
})
```

取值方法：v-color="'green'"

3. 使用自定义私有指令

在new Vue中

```javascript
directives:{
    'fontweight':{
    bind:function(el,binding){
        el.style.fontWeight = binding.value
        }
    }
}
```

在标签中使用指令<p v-fontweight="900"> hhh </p>

4. 自定义私有属性的简写

```javascript
directives:{
    'fontsize':function(el,binding){
        el.style.fontSize = parseInt（binding.value） + 'px'
        }
}
```

调用的方法：

​	v-fontsize="50"或者v-fontsize="'30ox'"

5. 注意点

1）和JS行为有关的操作，最好在 inserted 中去执行，防止JS行为不生效，只有该事件执行完毕之后才会将元素插入到dom中

2） 样式，只要通过指令绑定给了元素，不管这个元素有没有被插入到页面中去，这个元素肯定有了一个内联的样式。 将来元素肯定会显示到页面中，这时候，浏览器的渲染引擎必然会解析样式，应用给这个元素，使用bind去执行

