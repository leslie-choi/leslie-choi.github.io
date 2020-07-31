---
title: 一、Vue的增删改
date: 2019-01-22 16:59:35
tags: basic
categories: Vue
---

# 一、增

1. 在id的输入框内利用v-model绑定id属性，name属性同样利用v-model进行绑定

2. 在添加按钮上面利用v-on:click='add()'绑定点击事件，触发增加方法

3. 实现添加的方法

获取id和name，组织出一个对象，然后把这个对象添加到data中的list中

在Vue中，已经实现了数据的双向绑定，所以每次修改了data中的数据，Vue会默认监听数据的改动，然后应用到页面上

```javascript
var car = { id: this.id, name: this.name, ctime: new Date() }
this.list.push(car)
this.id = this.name = ''
```

# 二、删

1. 添加一个a标签，阻止默认的事件，绑定del(item.id)方法传入参数

2. 根据id删除数据，要先找到这一项的索引

3. 找到索引之后，调用数组的splice方法

```javascript
    var index = this.list.findIndex(item => {
        if (item.id == id) {
        return true;
        }
    })

    // console.log(index)
    this.list.splice(index, 1)
```

# 三、查

1. 根据关键字进行数据的搜索，在input标签中使用v-model绑定keywords属性

2. foreach  some  filter findIndex  都是属于数组的新方法，会对数组的每一项进行遍历

ES6中，为字符串提供了一个新方法，叫做  String.prototype.includes('要包含的字符串')

如果包含，则返回 true ，否则返回 false

```javascript
    return this.list.filter(item => {
        if (item.name.includes(keywords)) {
        return item
        }
    })
```

![vue增删改](D:\leslie-choi.github.io\source\_posts\vue\images\vue增删改.png)

# 四、动态改变样式

通过v-bind指令，绑定一个对象。对象里面可以设置对应的类名。通过true和false控制。
然后再在style中声明对应类名的样式。

```javascript
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <div :class = "obj">
      showObj
    </div>
    <button @click="changClass()">click</button>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      obj: {
        'obj1': true,
        'obj2': false
      }
    }
  },
  methods: {
    changClass () {
      this.obj.obj1 = false
      this.obj.obj2 = true
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.obj1 {
  font-size: 22px;
  color: red;
}
.obj2 {
  font-size: 28px;
  color: blue;
}
</style>


```