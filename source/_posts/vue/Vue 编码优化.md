---
title: vue 编码优化
date: 2020-07-30 08:43:20
tags: basic
categories: Vue
---

# 使用 Object.freeze() 提升性能

背景：在开发过程中，我们经常需要获取一些对象，像users信息，列表items，文章信息等等，但是我们不需要去修改这些信息。我们只是把这些信息展示出来，或者放在vuex中state里面。

Vue默认会对每个数组数据的每一层属性，添加双向数据绑定机制。当数组对象非常庞大时，消耗在这上面的双向数据绑定就越多。因为，在这种场景下，我们可以通过阻止Vue对这些数据添加双向数据绑定来提高一些性能。

根据 MDN 文档，使用 Object.freeze()，可以冻结一个对象，被冻结的对象则不能再被修改。即该对象不能添加、删除已有的属性，不能修改该对象已有属性的可枚举性、可配置性、可写性，以及不能修改已有属性的值。

**此外，冻结一个对象后该对象的原型也不能被修改。**

```javascript

const obj = {
    name: "james"
}

Object.freeze(obj)

obj.prop = 33

console.log(obj.prop)

```

PS: 顺便提一下密封对象和禁止对象属性拓展

1. Object.preventExtensions()，禁止对象拓展，但是可以修改和删除对象的属性。**Object.preventExtensions()仅阻止添加自身的属性。但其对象类型的原型依然可以添加新的属性。**

2. Object.seal()，可以修改对象属性，不可以删除和增加对象属性。

**以上三个方法分别可用Object.isExtensible(), Object.isSealed(), Object.isFrozen() 来检测**

当你把一个普通的 JavaScript 对象传给 Vue 实例的  data  选项，Vue 将遍历此对象所有的属性，并使用  Object.defineProperty  把这些属性全部转为 getter/setter，这些 getter/setter 对用户来说是不可见的，但是在内部它们让 Vue 追踪依赖，在属性被访问和修改时通知变化。

但 Vue 在遇到像 Object.freeze() 这样被设置为不可配置之后的对象属性时，不会为对象加上 setter getter 等数据劫持的方法，使用了 Object.freeze() 之后，减少了 observer 的开销。

由于 Object.freeze()会把对象冻结，所以比较适合展示类的场景，如果你的数据属性需要改变，可以重新替换成一个新的 Object.freeze()的对象。**由于 Object.freeze()会把对象冻结，所以比较适合展示类的场景，如果你的数据属性需要改变，可以重新替换成一个新的 Object.freeze()的对象。**

# requireContext 导入组件(todo)

# .sync 的使用

背景：在 Vue1.x 版本，.sync 还可以进行双向数据绑定，即说明子组件中可以修改父组件的值。但是这与单向数据流的设计理念相违背，所以在 Vue2.0 版本中被舍弃。直到 Vue2.3 版本及以上，重新引进了 .sync 修饰符，这次只是作为语法糖的存在。存在的意义就是让我们手动更新父组件中的值，并且让其的数据改动来源更加明显。

**适用于单纯在子组件中修改父组件中数据的场景**

父组件

```vue

<template>
  <div>
    <input type="button" value="我是父组件中的按钮" @click="show" />
    <!-- <Son @upIsShow="changeIsShow" v-show="isShow" /> -->
    <!-- <Son @update:isShow="changeIsShow" v-show="isShow"/> -->
    <!-- <Son @update:isShow="function(bol){isShow=bol}" v-show="isShow"/> -->
    <!-- <Son @update:isShow="bol=>isShow=bol" v-show="isShow"/> -->
    <Son :isShow.sync="isShow" v-show="isShow" :age.sync="age"/>
    <!-- 当子组件想要更改isShow的值，需要显式触发 this.$emit("update:isShow",false) 事件-->
    <span>{{this.age}}</span>
  </div>
</template>
<script>
import Son from "../components/sync/Son";
export default {
  data() {
    return {
      isShow: false,
      age: 20
    };
  },
  components: {
    Son
  },
  methods: {
    show() {
      this.isShow = true
    },
    changeIsShow(bol) {
      this.isShow = bol
    }
  }
};
</script>

```

子组件

```vue

<template>
  <div>
    我是一个子组件！
    <input type="button" value="点击消失" @click="upIsShow" />
  </div>
</template>

<script>
export default {
  methods: {
    upIsShow() {
      // this.$emit("upIsShow", false)
      this.$emit("update:isShow",false)
      this.$emit("update:age",22)
    }
  }
}
</script>


```

# 动态组件渲染




# 参考文章：

[利用Object.freeze()提升性能](https://juejin.im/post/5d5e89aee51d453bdb1d9b61)
[Object.freeze()提高Vue.js中大型列表的性能【翻译+解读】](https://www.jianshu.com/p/56168a779849)