---
title: Vuex的基本使用
date: 2019-04-26 14:53:20
tags: basic
categories: Vue
---

# Vuex的基本使用。

问题的引入：在Vue中，涉及到父子组件之间的传值，我们可以使用**props**和**$emit**方法进行传值，但是兄弟组件之间传值要怎么办呢？所以这个时候就要引入Vuex进行状态管理了，Vuex 是适用于Vue.js应用的状态管理库，为 Vue.js为应用中的所有组件提供集中式的状态存储与操作，保证了所有状态以可预测的方式进行修改。 

## 主要的几种状态

dispatch：操作行为触发方法，是唯一能执行action的方法。

actions：操作行为处理模块。负责处理Vue Components接收到的所有**交互行为**。包含**同步/异步**操作，支持多个同名方法，按照注册的顺序依次触发。向后台API请求的操作就在这个模块中进行，包括触发其他action以及提交mutation的操作。该模块提供了Promise的封装，以支持action的链式触发。

commit：状态改变提交操作方法。对mutation进行提交，是唯一能执行mutation的方法。

mutations：状态改变操作方法。是**Vuex修改state的唯一推荐方法**，其他修改方式在严格模式下将会报错。该方法只能进行同步操作，且方法名只能全局唯一。操作之中会有一些hook暴露出来，以进行state的监控等。

state：页面状态管理容器对象。集中存储Vue components中data对象的零散数据，全局唯一，以进行统一的状态管理。页面显示所需的数据从该对象中进行读取，利用Vue的细粒度数据响应机制来进行高效的状态更新。 

getters：state对象读取方法。直接通过这个获取数据

废话不多说了。直接贴代码8. 

第一步：在main.js中安装（如果项目大的话，需要全局共享处理的数据比较多，则最好对应的每个状态都建立一个js文件，至少不会把所有的代码糅合在一起，看着舒服一点。。）

第二步：导入vuex

第三步：注册vuex到vue中。

第四步：新建一个store实例，然后可以在这个state里面写对应的状态方法。

第五步：**将vuex创建的store挂载到vm实例上，才能全局都访问到这个store数据**。（敲黑板划重点，第一次忘记挂载结果找了好久的错误）

```javascript
// 入口文件
import Vue from 'vue'
// 配置vuex的步骤
// 1. 运行 cnpm i vuex -S 
// 2. 导入包
import Vuex from 'vuex'
// 3. 注册vuex到vue中
Vue.use(Vuex)
// 4. new Vuex.Store() 实例，得到一个 数据仓储对象
var store = new Vuex.Store({
  state: {
    // 大家可以把 state 想象成 组件中的 data ,专门用来存储数据的
    // 如果在 组件中，想要访问，store 中的数据，只能通过 this.$store.state.*** 来访问
    count: 0
  },
  mutations: {
    // 注意： 如果要操作 store 中的 state 值，只能通过 调用 mutations 提供的方法，才能操作对应的数据，不推荐直接操作 state 中的数据，因为 万一导致了数据的紊乱，不能快速定位到错误的原因，因为，每个组件都可能有操作数据的方法；
    increment(state) {
      state.count++
    },
    // 注意： 如果组件想要调用 mutations 中的方法，只能使用 this.$store.commit('方法名')
    // 这种 调用 mutations 方法的格式，和 this.$emit('父组件中方法名')
    subtract(state, obj) {
      // 注意： mutations 的 函数参数列表中，最多支持两个参数，其中，参数1： 是 state 状态； 参数2： 通过 commit 提交过来的参数；
      console.log(obj)
      state.count -= (obj.c + obj.d)
    }
  },
  getters: {
    // 注意：这里的 getters， 只负责 对外提供数据，不负责 修改数据，如果想要修改 state 中的数据，请 去找 mutations
    optCount: function (state) {
      return '当前最新的count值是：' + state.count
    }
    // 经过对比，发现 getters 中的方法， 和组件中的过滤器比较类似，因为 过滤器和 getters 都没有修改原数据， 都是把原数据做了一层包装，提供给了 调用者；
    // 其次， getters 也和 computed 比较像， 只要 state 中的数据发生变化了，那么，如果 getters 正好也引用了这个数据，那么 就会立即触发 getters 的重新求值；
  }
})


import App from './App.vue'

const vm = new Vue({
  el: '#app',
  render: c => c(App),
  store // 5. 将 vuex 创建的 store 挂载到 VM 实例上， 只要挂载到了 vm 上，任何组件都能使用 store 来存取数据
})
```

总结：

1. state中的数据，不能直接修改，如果想要修改，必须通过 mutations
2. 如果组件想要直接 从 state 上获取数据： 需要 this.$store.state.
3. 如果 组件，想要修改数据，必须使用 mutations 提供的方法，需要通过 this.$store.commit('方法的称'， 唯一的一个参数)
4. 如果 store 中 state 上的数据， 在对外提供的时候，需要做一层包装，那么 ，推荐使用 getters, 如果需要使用 getters ,则用 this.$store.getters.