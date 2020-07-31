---
title: $listener 和 $attrs 的使用
date: 2020-07-30 08:43:20
tags: basic
categories: Vue
---

#  $listener 和 $attrs 的使用

背景：假设有一个 father 组件，father 组件中引入了 child 组件，child 组件中又引入了 son 组件。那么 father 组件和 child 组件之间如何通信呢？

解决方案：

1. 万金油方案 Vuex，使用 Vuex 虽然能够实现数据之间的共享，但是项目中如果组件之间共享状态比较少，或者说项目比较小，那么使用 Vuex 确实没太大必要。
2. 将 child 组件当成一个中间站，然后利用父子组件之间 prop 和 $emit，一层一层向上或者向下传递信息，虽然可行但是使用麻烦并且可维护性差。
3. 自定义一个Vue 中央数据总线（todo），但是多人协作麻烦

针对这种情况，在vue2.4中，为了解决该需求，引入了$attrs 和$listeners，新增了inheritAttrs 选项。

**父组件**

```vue
<template>
  <div>
      <h1>Father</h1>
      <child :temp="tempdata" @tempFn="fatherFn" prop='$attrs不会传递child组件中定义的props5值'/>
  </div>
</template>

<script>
import Child from './Child.vue'
export default {
  components: {
      Child
  },
  props: {},
  data() {
    return {
      tempdata: 'i am father data'
    };
  },
  watch: {},
  computed: {},
  methods: {
    fatherFn: function() {
      console.log('call father function!')
    }
  },
  created() {},
  mounted() {}
}
</script>
```

**子组件**

```vue

<template>
  <div>
      <h1>child</h1>
      <son v-bind="$attrs" v-on="$listeners"></son>
  </div>
</template>

<script>
import Son from './Son'
export default {
  name: "Child",
  components: {
    Son
  },
  props: {},
  data() {
    return {
    };
  },
  watch: {},
  computed: {},
  methods: {},
  created() {},
  mounted() {
    console.log('this.$attrs',this.$attrs)  //  {prop: '$attrs不会传递child组件中定义的props5值',temp: 'i am father data'}
    console.log('this.$listeners',this.$listeners)  //  {tempFn: fn}
    this.$emit('tempFn')   //   子组件调用父组件方法
  }
};
</script>

```

**孙组件**

```vue

<template>
  <div>
    <h1>son</h1>
    <span>{{ $attrs.temp }}--------</span>
  </div>
</template>

<script>
export default {
  components: {},
  props: {},
  data() {
    return {
    };
  },
  watch: {},
  computed: {},
  methods: {},
  created() {},
  mounted() {
    // this.$emit('tempFn')
  }
};
</script>

```

* $attrs：当前组件的属性，通俗的讲也就是在组件标签定义的一系列属性，如input的value，placeholder等，但是不包括在当前组件里面定义的props属性。

* $listeners：当前组件监听的事件，通俗的讲也就是在使用组件的时候在标签中定义的事件，如@input，以及一些自定义事件@tempFn等。








# 参考文章：

(Vue中的$attrs和$listener)[https://www.cnblogs.com/lsboom/p/11365293.html]
(绝对干货~！学会这些Vue小技巧，可以早点下班和女神约会了)[https://juejin.im/post/5eddbaee5188254344768fdc#heading-10]
[VUE中.sync 修饰符](https://blog.csdn.net/weixin_42966484/article/details/89408538)