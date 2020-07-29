---
title: vue常见面试题
date: 2019-07-27 10:43:20
tags: basic
categories: Vue
---

# MVVM的理解

MVVM 模式，顾名思义即 Model-View-ViewModel 模式,是一个软件架构设计模式。

1. Model 层: 对应数据层模型，它主要做域模型的同步。通过 Ajax/fetch 等 API 完成客户端和服务端业务 Model 的同步。

2. View 层：作为视图模板存在，在 MVVM 里，整个 View 是一个动态模板。除了定义结构、布局外，它展示的是 ViewModel 层的数据和状态。View 层不负责处理状态，View 层做的是 数据绑定的声明、 指令的声明、 事件绑定的声明。

3. ViewModel 层(核心):MVVM 的核心是 ViewModel 层，它就像是一个中转站（value converter），负责转换 Model 中的数据对象来让数据变得更容易管理和使用，该层向上与视图层进行双向数据绑定，向下与 Model 层通过接口请求进行数据交互，起呈上启下作用。View 层展现的不是 Model 层的数据，而是 ViewModel 的数据，由 ViewModel 负责与 Model 层交互，这就完全解耦了 View 层和 Model 层，这个解耦是至关重要的，**因此开发者只需关注业务逻辑，不需要手动操作DOM, 不需要关注数据状态的同步问题，复杂的数据状态维护完全由 MVVM 来统一管理。** 这就完全解耦了 View 层和 Model 层，这个解耦是至关重要的，它是前后端分离方案实施的重要一环。
![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/MVVM.png)

## MVVM的优缺点

**优点**

1. 分离视图（View）和模型（Model）,降低代码耦合，提高视图或者逻辑的重用性。

2. 提高可测试性: ViewModel的存在可以帮助开发者更好地编写测试代码

3. 自动更新dom: 利用双向绑定,数据更新后视图自动更新，让开发者从繁琐的手动更新dom中解放

**缺点**

1. Bug很难被调试: 因为使用双向绑定的模式，当你看到界面异常了，有可能是你View的代码有Bug，也可能是Model的代码有问题。数据绑定使得一个位置的Bug被快速传递到别的位置，要定位原始出问题的地方就变得不那么容易了。另外，数据绑定的声明是指令式地写在View的模版当中的，这些内容是没办法去打断点debug的

2. 一个大的模块中model也会很大，虽然使用方便了也很容易保证了数据的一致性，当时长期持有，不释放内存就造成了花费更多的内存

3. 对于大型的图形应用程序，视图状态较多，ViewModel的构建和维护的成本都会比较高


## MVVM和MVC的区别是什么？与其他框架（jquery）的区别是什么？哪些场景适合？

mvc和mvvm其实区别并不大。都是一种设计思想。主要就是mvc中Controller演变成mvvm中的viewModel。mvvm主要解决了mvc中大量的DOM 操作使页面渲染性能降低，加载速度变慢，影响用户体验。

区别：vue数据驱动，通过数据来显示视图层而不是节点操作。
场景：数据操作比较多的场景，更加便捷

# Vue的生命周期

Vue 实例有一个完整的生命周期，也就是从开始创建、初始化数据、编译模版、挂载Dom -> 渲染、更新 -> 渲染、卸载等一系列过程，我们称这是Vue的生命周期。

1. beforeCreate:挂载元素el和数据对象都是为undefined
dom元素都没有被创建出来，在这个期间我们一般不做事情

2. created：挂载元素el未初始化，但是data和methods都已经被初始化好了（最重要）
数据和data中的数据绑定完成，这里我们可以做初级的数据获取，ajax请求最好放在created里面，因为此时已经可以访问this了，请求到数据就可以直接放在data里面。

3. beforeMount：挂载元素el和data已经初始化，但是此时是虚拟dom节点

4. mounted：Vue实例挂载完成，数据成功渲染（最重要）
官方实例的异步请求是在mounted生命周期中调用的，而实际上也可以在created生命周期中调用(推荐，因为在服务端并没有这个钩子)。

只要执行完整个mounted，就表示整个Vue实例已经完成初始化。此时组件已经脱离了创建阶段，进入运行阶段
运行阶段的两个生命周期函数，这两个事件会根据data数据的改变，有选择性触发0到多次

5. beforeUpdate：表示界面还没有被更新，但是数据被更新了

6. update： 事件执行的时候，页面和data数据已经保持同步，都是最新的

7. beforeDestroy：执行该函数的时候，实例身上的所有data和所有的methods以及过滤器、指令等等都处于可用状态，还没有真正执行销毁的过程

8. 执行到该函数的时候，组件中的而所有数据、方法、指令过滤器都不可用

除了以上8个比较常见的生命周期之外，vue其实还有其他3个生命周期

如果我们在组件中使用了keep-alive用来缓存组件,避免多次加载相应的组件,减少性能消耗，那么这个时候生命周期会多出来两个，并且上面的8个生命周期钩子将会失效。

9. activated：被包裹的组件被激活的状态下使用的生命周期钩子。

10. deactivated：在被包裹的组件停止的时候停用。

11. errorCaptured: 当捕获一个来自子孙组件的错误时被调用。此钩子会收到三个参数：错误对象、发生错误的组件实例以及一个包含错误来源信息的字符串。此钩子可以返回 false 以阻止该错误继续向上传播。

# v-show 与 v-if 有什么区别

* v-if 是真正的条件渲染，因为它会确保在切换过程中会**触发销毁/挂载组件**；也是惰性的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。

* v-show 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 的 “display” 属性进行切换。DOM结构还是一直保留。

所以，v-if 适用于在运行时很少改变条件，不需要频繁切换条件的场景；v-show 则适用于需要非常频繁切换条件的场景。

# Vue 的父组件和子组件生命周期钩子函数执行顺序

Vue 的父组件和子组件生命周期钩子函数执行顺序可以归类为以下 4 部分：

加载渲染过程
父 beforeCreate -> 父 created -> 父 beforeMount -> 子 beforeCreate -> 子 created -> 子 beforeMount -> 子 mounted -> 父 mounted

子组件更新过程
父 beforeUpdate -> 子 beforeUpdate -> 子 updated -> 父 updated

父组件更新过程
父 beforeUpdate -> 父 updated

销毁过程
父 beforeDestroy -> 子 beforeDestroy -> 子 destroyed -> 父 destroyed

# Vue组件之间的通信

* **props/$emit+v-on: 通过props将数据自上而下传递，而通过$emit和v-on来向上传递信息。** （适用于父子组件之间的通信）
* EventBus: 通过EventBus进行信息的发布与订阅
* **vuex: 是全局数据管理库，可以通过vuex管理全局的数据流**
* $attrs/$listeners: Vue2.4中加入的$attrs/$listeners可以进行跨级的组件通信 （适用于 隔代组件通信）
* provide/inject：祖先组件中通过 provider 来提供变量，然后在子孙组件中通过 inject 来注入变量。 provide / inject API 主要解决了跨级组件间的通信问题，不过它的使用场景，主要是子组件获取上级组件的状态，跨级组件间建立了一种主动提供与依赖注入的关系。（适用于 隔代组件通信）

# 单向数据流

单向数据流指只能从一个方向修改数据，姑且我们可以这样理解，如下图所示。一个父组件下有两个子组件1和子组件2，父组件可以向子组件传递数据。假如子组件都获取到了父组件的name，在子组件1中对name重新修改之后，子组件2和父组件中的值并不会发生改变，这正是因为Vue中的机制是单向数据流，子组件不能直接改变父组件的状态。但反过来，如果是父组件中的name修改了，当然两个子组件中的name也就改变了。

![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/FtoC.png)

# Vuex状态管理

Vuex 是单向数据流的一种实现。

![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/dangxiang.png)

一个应用可以看作是由上面三部分组成: View, Actions,State,数据的流动也是从View => Actions => State =>View 以此达到数据的单向流动。但是，当我们的应用遇到多个组件共享状态时，单向数据流的简洁性很容易被破坏：

* 多个视图依赖于同一状态。
* 来自不同视图的行为需要变更同一状态。

当项目较大, 组件嵌套过多的时候,使用传参的方式会变得非常繁琐 多组件共享同一个State会在数据传递时出现很多问题.Vuex就是为了解决这些问题而产生的。
Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。

1. Vuex 的状态存储是响应式的。当 Vue 组件从 store 中读取状态的时候，若 store 中的状态发生变化，那么相应的组件也会相应地得到高效更新。
2. 改变 store 中的状态的唯一途径就是显式地提交 (commit) mutation。这样使得我们可以方便地跟踪每一个状态的变化。
主要包括以下几个模块：

* State：存放基本数据，可以在这里设置默认的初始状态。用 this.$store.state 来访问。
* Getters：允许组件从 Store 中获取数据，mapGetters 辅助函数仅仅是将 store 中的 getter 映射到局部计算属性。
* Mutations：是唯一更改 store 中状态的方法，且必须是同步函数。用 this.$store.commit('xxx', data) 来通知 Mutations 来改状态。
* Actions：用于提交 mutation，而不是直接变更状态，可以包含任意异步操作。用 this.$store.dispatch('xxx', data) 来存触发 Action。
* Modules：允许将单一的 Store 拆分为多个 store 且同时保存在单一的状态树中,模块化vuex。

**完整流程：**

![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/vuex.png)

Vuex原理
一个实例化的Vuex.Store由state, mutations和actions三个属性组成:

* state中保存着共有数据
* 改变state中的数据有且只有通过mutations中的方法,且mutations中的方法必须是同步的
* 如果要写异步的方法,需要些在actions中, 并通过commit到mutations中进行state中数据的更改

# Vue实现双向数据绑定

是采用数据劫持结合发布者-订阅者模式的方式，通过Object.defineProperty()来劫持各个属性的setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调。在vue3.0中通过Proxy代理对象进行类似的操作。

Vue主要通过以下4个步骤实现双向数据绑定

* 实现一个监听器 Observer：对数据对象进行遍历，包括子属性对象的属性，利用 Object.defineProperty() 对属性都加上 setter 和 getter。这样的话，给这个对象的某个值赋值，就会触发 setter，那么就能监听到了数据变化。
* 实现一个解析器 Compile：解析 Vue 模板指令，将模板中的变量都替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，调用更新函数进行数据更新。
* 实现一个订阅者 Watcher：Watcher 订阅者是 Observer 和 Compile 之间通信的桥梁 ，主要的任务是订阅 Observer 中的属性值变化的消息，当收到属性值变化的消息时，触发解析器 Compile 中对应的更新函数。
* 实现一个订阅器 Dep：订阅器采用 发布-订阅 设计模式，用来收集订阅者 Watcher，对监听器 Observer 和 订阅者 Watcher 进行统一管理。

![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/observer.png)
```javascript
<input type="text" name="数据绑定" id="dataBinding">
<span id="data"></span><br>

mounted() {
    this.showData()
},
methods:{
      showData(){
          var obj1 = {};                        //新建一个对象obj1
          Object.defineProperty(obj1,'hello',{  //为obj定义一个名为 hello 的访问器属性 
              set: function(newVal){
                  document.getElementById('dataBinding').value = newVal;
                  document.getElementById('data').innerHTML = newVal;
              },
              get: function(){
                  return
              }
          });
          document.addEventListener('keyup',function(e){
              obj1.hello = e.target.value;
          });
          console.log(obj1);
      }
}
```

# computed和watch有什么区别?

* **computed:**

computed是计算属性,也就是计算值,它更多用于计算值的场景
computed具有缓存性,computed的值在getter执行后是会缓存的，只有在它依赖的属性值改变之后，下一次获取computed的值时才会重新调用对应的getter来计算
computed适用于计算比较消耗性能的计算场景

* **watch:**

更多的是「观察」的作用,类似于某些数据的监听回调,用于观察props $emit或者本组件的值,当数据变化时来执行回调进行后续操作
无缓存性，页面重新渲染时值不变化也会执行

例子：

```javascript
<div id="demo">{{ fullName }}</div>

var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  },
  watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  },
  computed: {
    fullName: function () {
      return this.firstName + ' ' + this.lastName
    }
  }
})
```

虽然说methods和computed、watch都可以完成相同的功能，但是需要根据不同的场景选择对应的方法，大多数场景下，使用computed属性，能够在简化代码的同时，提高性能。

小结:
* 当我们要进行数值计算,而且依赖于其他数据，那么把这个数据设计为computed
* 当我们需要在数据变化时执行异步或开销较大的操作时，应该使用 watch，使用 watch 选项允许我们执行异步操作 ( 访问一个 API )，限制我们执行该操作的频率，并在我们得到最终结果前，设置中间状态。这些都是计算属性无法做到的

# computed实现原理

* 初始化data，使用Object.defineProperty将这些属性转化为getter/setter
* 初始化computed，遍历computed里面的每个属性，每一个computed属性都是一个watch实例。每个属性提供的函数作为属性的getter，然后使用Object.defineProterty转化
* Object.defineProterty依赖收集，用于依赖发生变化的时候，触发属性重新计算
* 若出现当前computed计算属性嵌套其他computed计算属性时，先计算其他的依赖收集

# 虚拟DOM

1. 什么是虚拟DOM

虚拟DOM其实就是一个JavaScript对象。通过这个JavaScript对象来描述真实DOM。

2. 为什么要使用虚拟DOM

优点：

* 保证性能下限: 虚拟DOM可以经过diff找出最小差异,然后批量进行patch,这种操作虽然比不上手动优化,但是比起粗暴的DOM操作性能要好很多,因此框架的虚拟 DOM至少可以保证在你不需要手动优化的情况下，依然可以提供还不错的性能，即保证性能的下限；真实DOM的操作，一般都会对某块元素的整体重新渲染,采用虚拟DOM的话，当数据变化的时候，只需要局部刷新变化的位置就好了。

* 无需手动操作DOM: 我们不再需要手动去操作 DOM，只需要写好 View-Model 的代码逻辑，框架会根据虚拟 DOM 和 数据双向绑定，极大提高开发效率

* 跨平台: 虚拟DOM本质上是JavaScript对象,而DOM与平台强相关,相比之下虚拟DOM可以进行更方便地跨平台操作,例如服务器渲染、移动端开发等等

缺点:

* 无法进行极致优化: 在一些性能要求极高的应用中虚拟DOM无法进行针对性的极致优化,比如VScode采用直接手动操作DOM的方式进行极端的性能优化

# 虚拟DOM的实现原理

* 用JS对象描述出DOM树的结构，然后在初始化构建中，用这个描述树去构建真正的DOM，并实际展现到页面中

* 使用diff算法，当有数据状态变更时，重新构建一个新的JS的DOM树，通过新旧对比DOM树的变化diff，**并记录两棵树差异**

* 使用patch算法，将两个虚拟 DOM 对象的差异应用到真正的 DOM 树，视图也就更新了。所以虚拟dom的本质可以理解为在JS和DOM之间做了一个缓存。 

# 如何理解Vue的响应式系统？

响应式系统简述:

* 任何一个 Vue Component 都有一个与之对应的 Watcher 实例。
* Vue 的 data 上的属性会被object.defineProperty()添加 getter 和 setter 属性。
* 当 Vue Component render 函数被执行的时候, data 上会被 触碰(touch), 即被读, **getter 方法会被调用** , 此时 Vue 会去记录此 Vue component 所依赖的所有 data。(这一过程被称为依赖收集)
* data 被改动时（主要是用户操作）, 即被写, setter 方法会被调用, 此时 Vue 会去通知所有依赖于此 data 的组件去调用他们的 render 函数进行更新。

![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/vueWatcher.png)

# 谈谈你对 keep-alive 的了解？

keep-alive 是 Vue 内置的一个组件，可以使被包含的组件保留状态，避免重新渲染 ，其有以下特性：

* 一般结合路由和动态组件一起使用，用于缓存组件；
* 提供 include 和 exclude 属性(另外还有max参数，设置最大缓存组件的个数)，两者都支持字符串或正则表达式， include 表示只有名称匹配的组件会被缓存，exclude 表示任何名称匹配的组件都不会被缓存 ，**其中 exclude 的优先级比 include 高；**
* 对应两个生命周期钩子函数 activated 和 deactivated ，当组件被激活时，触发钩子函数 activated，当组件被移除时，触发钩子函数 deactivated。

# 组件中的data为什么是一个函数?

```javascript
// data
data() {
  return {
	message: "子组件",
	childName:this.name
  }
}

// new Vue
<div id="app">
  {{ message }}
</div>

var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})

```

因为**组件是用来复用的，且 JS 里对象是引用关系，如果组件中 data 是一个对象，那么这样作用域没有隔离，子组件中的 data 属性值会相互影响**，如果组件中 data 选项是一个函数，那么每个实例可以维护一份被返回对象的独立的拷贝，组件实例之间的 data 属性值不会互相影响；而 **new Vue 的实例，是不会被复用的**，因此不存在引用对象的问题。

# vue-ssr 服务端渲染

ssr是Server-Side Rendering的简写，即由服务端负责渲染页面直出，亦即同构应用。程序的大部分代码都可以在服务端和客户端运行。在服务端vue组件渲染为html字符串，然后直接返回给客户端，在客户端生成dom和操作dom。

能在服务端渲染为html字符串得益于vue组件结构是基于vnode的。vnode是dom的抽象表达，它不是真实的dom，它是由js对象组成的树，每个节点代表了一个dom。因为vnode所以在服务端vue可以把js对象解析为html字符串。同样在客户端vnode因为是存在内存之中的，操作内存总比操作dom快的多（操作DOM会导致重排和重绘，重排会占用、消耗CPU; 重绘会占用、消耗GPU），每次数据变化需要更新dom时，新旧vnode树经过diff算法，计算出最小变化集，大大提高了性能。

**服务端渲染 SSR 的优缺点如下：**

(1) 服务端渲染的优点：
* 更好的 SEO： 因为 SPA 页面的内容是通过 Ajax 获取，而搜索引擎爬取工具并不会等待 Ajax 异步完成后再抓取页面内容，所以在 SPA 中是抓取不到页面通过 Ajax 获取到的内容；而 SSR 是直接由服务端返回已经渲染好的页面（数据已经包含在页面中），所以搜索引擎爬取工具可以抓取渲染好的页面
* 更快的内容到达时间（首屏加载更快）： SPA 会等待所有 Vue 编译后的 js 文件都下载完成后，才开始进行页面的渲染，文件下载等需要一定的时间等，所以首屏渲染需要一定的时间；SSR 直接由服务端渲染好页面直接返回显示，无需等待下载 js 文件及再去渲染等，所以 SSR 有更快的内容到达时间

(2) 服务端渲染的缺点：
* 更多的开发条件限制： **例如服务端渲染只支持 beforCreate 和 created 两个钩子函数**，这会导致一些外部扩展库需要特殊处理，才能在服务端渲染应用程序中运行；并且与可以部署在任何静态文件服务器上的完全静态单页面应用程序 SPA 不同，服务端渲染应用程序，需要处于 Node.js server 运行环境；
* 更多的服务器负载：在 Node.js  中渲染完整的应用程序，显然会比仅仅提供静态文件的  server 更加大量占用CPU 资源 (CPU-intensive - CPU 密集)，因此如果你预料在高流量环境 ( high traffic ) 下使用，请准备相应的服务器负载，并明智地采用缓存策略。（在同构应用中，如果需要获取数据最好在mounted生命周期中异步加载数据，因为服务端渲染只支持 beforCreate 和 created 两个钩子函数，如果放在created生命周期中，有可能会影响SSR的构建）

# vue-router 路由模式有几种？

vue-router 有 3 种路由模式：hash、history、abstract

* hash:  使用 URL hash 值来作路由。支持所有浏览器，包括不支持 HTML5 History Api 的浏览器；
* history :  依赖 HTML5 History API 和服务器配置。具体可以查看 HTML5 History 模式；
* abstract :  支持所有 JavaScript 运行环境，如 Node.js 服务器端。如果发现没有浏览器的 API，路由会自动强制进入这个模式.

# router的实现原理

**更新视图而不重新请求页面**,vue-router在实现单页面前端路由时，提供了两种方式：Hash模式和History模式；根据mode参数来决定采用哪一种方式。

1. Hash模式：

vue-router 默认 hash 模式 —— 使用 URL 的 hash 来模拟一个完整的 URL，于是当 URL 改变时，页面不会重新加载。 hash（#）是URL 的锚点，代表的是网页中的一个位置，单单改变#后的部分，浏览器只会滚动到相应位置，不会重新加载网页，也就是说hash 出现在 URL 中，但**不会被包含在 http 请求中，对后端完全没有影响**，因此改变 hash 不会重新加载页面；同时每一次改变#后的部分，都会在浏览器的访问历史中增加一个记录，使用”后退”按钮，就可以回到上一个位置；所以说Hash模式通过锚点值的改变，根据不同的值，渲染指定DOM位置的不同数据。hash 模式的原理是 **onhashchange 事件(监测hash值变化)**，可以在 window 对象上监听这个事件，从而进行页面的跳转和渲染

2. History模式：

由于hash模式会在url中自带#，如果不想要很丑的 hash，我们可以用路由的 history 模式，只需要在配置路由规则时，加入"mode: 'history'，不过这种模式需要后台再进行配置，如果后台配置不正确，访问页面则会返回404错误。HTML5 提供了 History API 来实现 URL 的变化。其中做最主要的 API 有以下两个：history.pushState() 和 history.repalceState()。这两个 API 可以在不进行刷新的情况下，操作浏览器的历史纪录。唯一不同的是，前者是新增一个历史记录，后者是直接替换当前的历史记录，如下所示：

history 路由模式的实现主要基于存在下面几个特性：

* pushState 和 repalceState 两个 API 来操作实现 URL 的变化 ；
* 我们可以使用 popstate  事件来监听 url 的变化，从而对页面进行跳转（渲染）；
* history.pushState() 或 history.replaceState() 不会触发 popstate 事件，这时我们需要手动触发页面跳转（渲染）。

3. vue-router的使用方式

* npm i vue-router -S下载路由
* 在main.js中引入 import VueRouter from 'vue-router';
* 安装插件Vue.use(VueRouter);
* 创建路由对象并配置路由规则 let router = new VueRouter({routes:[{path:'/home',component:Home}]});
* 将其路由对象传递给Vue的实例，options中加入 router:router
* 在app.vue中留坑 <router-view></router-view>
（router-view即页面中需要切换的部分）

4. hash 和 history 两种模式的优缺点

**hash优点：**

* 兼容性比较好，兼容性达到了ie8
* 绝大数框架的框架都基本支持hash路由方式
* 除了会发送ajax和资源加载之外不会发送其他请求
* 不需要在服务端进行任何设置和开发

**hash缺点：**

* 服务端无法准确捕获路由的信息
* 对于需要锚点功能的需求会与当前路由机制发生冲突
* 对于需要重定向的操作，后端无法获取url全部内容，导致后台无法得到url数据，典型的例子就是微信公众号的oauth验证

**history优点：**

* 当发生路由重定向时不会丢失url数据 ，后端也可以拿到这个数据
* 当然，绝大多数框架一样实现了 browser路由的方式
* 后端可以准确追踪到路由
* 可以使用history.state获取路由的信息

**history缺点：**

* 兼容性不如hash 。兼容性只到ie10

* 需要后端支持，每次返回html文档

# 路由导航守卫

有的时候，我们需要通过路由来进行一些操作，比如最常见的登录权限验证，当用户满足条件时，才让其进入导航，否则就取消跳转，并跳到登录页面让其登录。

为此我们有很多种方法可以植入路由的导航过程：**全局的, 单个路由独享的, 或者组件级的**

## 全局守卫

* router.beforeEach 全局前置守卫 进入路由之前
* router.beforeResolve 全局解析守卫(2.5.0+) 在beforeRouteEnter调用之后调用
* router.afterEach 全局后置钩子 进入路由之后

使用方法：

```javascript
    // main.js 入口文件
    import router from './router'; // 引入路由
    router.beforeEach((to, from, next) => { 
      next();
    });
    router.beforeResolve((to, from, next) => {
      next();
    });
    router.afterEach((to, from) => {
      console.log('afterEach 全局后置钩子');
    });
```

to,from,next 这三个参数：

to和from是将要进入和将要离开的路由对象,路由对象指的是平时通过this.$route获取到的路由对象。

next:Function 这个参数是个函数，且必须调用，否则不能进入路由(页面空白)。

* next() 进入该路由。

* next(false): 取消进入路由，url地址重置为from路由地址(也就是将要离开的路由地址)。

* next 跳转新路由，当前的导航被中断，重新开始一个新的导航。

## 路由独享守卫

beforeEnter

如果你不想全局配置守卫的话，你可以为某些路由单独配置守卫

```javascript
    const router = new VueRouter({
      routes: [
        {
          path: '/foo',
          component: Foo,
          beforeEnter: (to, from, next) => { 
            // 参数用法什么的都一样,调用顺序在全局前置守卫后面，所以不会被全局守卫覆盖
            // ...
          }
        }
      ]
    })
```

## 路由组件内的守卫

* beforeRouteEnter 进入路由前
* beforeRouteUpdate (2.2) 路由复用同一个组件时
* beforeRouteLeave 导航离开该组件的对应路由时调用，我们用它来禁止用户离开，比如还未保存草稿，或者在用户离开前，将setInterval销毁，防止离开之后，定时器还在调用。

## 完整的路由导航解析流程

完整的路由导航解析流程(不包括其他生命周期)：

* 触发进入其他路由。
* 调用要离开路由的组件守卫beforeRouteLeave
* 调用局前置守卫：beforeEach
* 在重用的组件里调用 beforeRouteUpdate
* 调用路由独享守卫 beforeEnter。
* 解析异步路由组件。
* 在将要进入的路由组件中调用beforeRouteEnter
* 调用全局解析守卫 beforeResolve
* 导航被确认。
* 调用全局后置钩子的 afterEach 钩子。
* 触发DOM更新(mounted)。
* 执行beforeRouteEnter 守卫中传给 next 的回调函数


# 触发钩子的完整顺序

将路由导航、keep-alive、和组件生命周期钩子结合起来的，触发顺序，假设是从a组件离开，第一次进入b组件：

1. beforeRouteLeave:路由组件的组件离开路由前钩子，可取消路由离开。
2. beforeEach: 路由全局前置守卫，可用于登录验证、全局路由loading等。
3. beforeEnter: 路由独享守卫
4. beforeRouteEnter: 路由组件的组件进入路由前钩子。
5. beforeResolve:路由全局解析守卫
6. afterEach:路由全局后置钩子
7. beforeCreate:组件生命周期，不能访问this。
8. created:组件生命周期，可以访问this，不能访问dom。
9. beforeMount:组件生命周期
10. deactivated: 离开缓存组件a，或者触发a的beforeDestroy和destroyed组件销毁钩子。
11. mounted:访问/操作dom。
12. activated:进入缓存组件，进入a的嵌套子组件(如果有的话)。
13. 执行beforeRouteEnter回调函数next。

# SPA单页面应用的优缺点

SPA分为2种。 第一种，SPA即指水疗、芳香按摩、沐浴、去死角等等。现代SPA主要透过人体的五大感官功能，即听觉（疗效音乐）、味觉（花草茶、健康饮食）、触觉（按摩）、视觉等达到全方位的放松，将精、气、神三者合一，实现身心的放松。

第二种，spa指的是single page application，就是只有一张Web页面的应用。单页应用程序 (SPA) 是加载单个HTML 页面并在用户与应用程序交互时动态更新该页面的Web应用程序，它在加载页面时，不会加载整个页面，而是只更新某个指定的容器中内容。 浏览器一开始会加载必需的HTML、CSS和JavaScript，所有的操作都在这张页面上完成，都由JavaScript来控制。因此，对单页应用来说模块化的开发和设计显得相当重要。

## 优点:

1. 用户体验好,快,内容的改变不需要重新加载整个页面
2. 基于上面一点,spa相对于服务器压力小
3. 良好的前后端分离
4. 同一套后端程序代码,不用修改就可以用web界面,手机,平板等多种客户端

## 缺点:

1.不利于seo：右键点击SEO应用查看源码，所有的页面和业务逻辑都封装在app.js中，对于搜索引擎来说，只能够抓取到app.js中的代码，有的搜索引擎会执行js查看页面内容，有的却无法执行，所以无法知道页面的具体内容，故而对SEO不友好。

2.初次加载耗时相对增多

3.导航不可用,如果一定要导航需要自行实现前进,后退

而前端路由router就是管理SPA单页面应用的路径管理器。vue的单页面应用是基于路由和组件的，路由用于设定访问路径，并将路径和组件映射起来。传统的页面应用，是用一些超链接来实现页面切换和跳转的。在vue-router单页面应用中，则是路径之间的切换，也就是组件的切换。路由模块的本质 就是建立起url和页面之间的映射关系。



# Vue 中的 key 有什么作用？

**key 是为 Vue 中 vnode 的唯一标记，通过这个 key，我们的 diff 操作可以更准确、更快速。没有设置key时，有些节点不会复用，而是直接创建新的，删除旧的。**可以用于 diff 中 sameVnode 算法的判断。

首先讲一下diff算法的处理方法，对操作前后的dom树同一层的节点进行对比，一层一层对比，如下图：

![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/key1.png)

当某一层有很多相同的节点时，也就是列表节点时，Diff算法的更新过程默认情况下也是遵循以上原则。

比如以下的情况，想要在B和C之间添加一个F
![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/key2.png)

在没有绑定key的时候，diff算法执行起来是这样子的

![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/key3.png)

即把C更新成F，D更新成C，E更新成D，最后再插入E，这样导致了节点没有得到复用，也造成了误差
所以我们需要使用key来给每个节点做一个唯一标识，Diff算法就可以正确的识别此节点，找到正确的位置区插入新的节点

![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/key4.png)


**准确: 如果不加key,那么vue会选择复用节点(Vue的就地更新策略),导致之前节点的状态被保留下来,会产生一系列的bug**

**快速: key的唯一性可以被Map数据结构充分利用,相比于遍历查找的时间复杂度O(n),Map的时间复杂度仅仅为O(1)**

在以下例子中做动态改变的时候，尽量不要使用 index 作为循环的 key，如果你用 index 作为 key，比如在删除第二项的时候，index 就会从 0，1，2 变为 0，1（而不是 0，2），那么仍有可能引起更新错误
diff算法默认使用“就地复用”的策略，是一个首尾交叉对比的过程
用index作为key和不加key是一样的，都采用“就地复用”的策略，所以不推荐使用index作为key，因为增加删除某一个项可能会导致其他index动态变化，那么有可能引起更新错误,所以必须要用唯一的键值表示key

```html
<ul>
  <li> key:0 ，id:201401,name:chen</li>
  <li> key:1 , id:201402,name:sun</li>    <!--如果我们要在中间插入一条数组{id:201404,name:zhou}-->
  <li> key:2 , id:201403,name:wang</li>
</ul>

<ul>
  <li>key:0 ，id:201401,name:chen  </li>
  <li>key:1 ，id:201404,name:zhou  </li>
  <li>key:2 , id:201402,name:sun  </li>
  <li>key:3 , id:201403,name:wang  </li>
</ul>

```
当我们在中间插入新元素的时候 新元素的key值理所应当变成了index=1，key值也就变成了1 而原本index==1的li元素的index就变成了2，原本index==2的元素key值就变成了3 。
这样就导致虚拟dom的diff算法在做比较的时候发现，key值为1，2，3的元素和原来的key值为1，2，3的元素对比的时候发现二者不一样，**diff算法就会重新渲染这三个元素**，原本key值为1，2的元素内容没有发生变化，但是因为key值使用的是index所以还需要重新渲染，这就失去了虚拟dom在性能上的优势

# Vue中的diff算法

vue和react的虚拟DOM的Diff算法大致相同，其核心是基于两个简单的假设：

1. 两个相同的组件产生类似的DOM结构，不同的组件产生不同的DOM结构。
2. 同一层级的一组节点，他们可以通过唯一的id进行区分。

基于以上这两点假设，使得虚拟DOM的Diff算法的复杂度从O(n^3)降到了O(n)
使用传统的 Diff 算法通过循环递归遍历节点进行对比，其复杂度要达到O(n^3)，其中 n 是节点总数，效率十分低下

优化后的算法：
* Tree Diff（层级的比较）
* Component Diff（组件的比较）
* Element Diff（元素/节点的比较）


Vue中的diff算法，就是用来比较虚拟DOM和需要渲染的真实DOM之间的差异。所以下面这张图可以很好的解释这个过程。**比较只会在同个层级之间进行，不会跨层级比较**

```html
  <!-- 之前 -->
  <div>           <!-- 层级1 -->
    <p>            <!-- 层级2 -->
      <b> aoy </b>   <!-- 层级3 -->   
      <span>diff</Span>
    </P> 
  </div>

  <!-- 之后 -->
  <div>            <!-- 层级1 -->
    <p>             <!-- 层级2 -->
        <b> aoy </b>        <!-- 层级3 -->
    </p>
    <span>diff</Span>
  </div>
```
最理想的就是将`<span>`直接移动到`<p>`的后边，这是最优的操作。但是实际的diff操作是移除`<p>`里的`<span>`，然后再创建一个新的`<span>`插到`<p>`的后边。
因为新加的`<span>`在层级2，旧的在层级3，属于不同层级的比较。

diff算法的过程其实就是调用patch函数，就像是打补丁一样修改真实DOM。
patch函数有两个参数，vnode和oldVnode，也就是新旧两个虚拟节点。

## **sameVnode**

在这之前，我们先了解完整的vnode都有什么属性，举个一个简单的例子:

```javascript
  // body下的 <div id="v" class="classA"><div> 对应的 oldVnode 就是
  {
    el:  div  //对真实的节点的引用，本例中就是document.querySelector('#id.classA')
    tagName: 'DIV',   //节点的标签
    sel: 'div#v.classA'  //节点的选择器
    data: null,       // 一个存储节点属性的对象，对应节点的el[prop]属性，例如onclick , style
    children: [], //存储子节点的数组，每个子节点也是vnode结构
    text: null,    //如果是文本节点，对应文本节点的textContent，否则为null
  }
```

来到patch的第一部分，sameVnode函数就是看这两个节点是否值得比较：
判断两个Vnode节点是否是同一个节点，需要满足下面的条件
* key相同
* tag（当前节点的标签名）相同
* isComment（是否为注释节点）相同
* 是否data（当前节点对应的对象，包含了具体的一些数据信息，是一个VNodeData类型，可以参考VNodeData类型中的数据信息）都有定义
* 当标签是 input 的时候，type必须相同

当两个VNode的tag、key、isComment都相同，并且同时定义或未定义data的时候，且如果标签为input则type必须相同。这时候这两个VNode则算sameVnode，可以直接进行patchVnode操作。

## **patchVnode**

patchVnode的规则是这样的：

1. 如果新旧VNode都是静态的，同时它们的key相同（代表同一节点），并且新的VNode是clone或者是标记了once（标记v-once属性，只渲染一次），那么只需要替换elm以及componentInstance即可。

2. 新老节点均有children子节点，则对子节点进行diff操作，调用updateChildren，这个updateChildren也是diff的核心。

3. 如果老节点没有子节点而新节点存在子节点，先清空老节点DOM的文本内容，然后为当前DOM节点加入子节点。

4. 当新节点没有子节点而老节点有子节点的时候，则移除该DOM节点的所有子节点。

5. 当新老节点都无子节点的时候，只是文本的替换。

## **updateChildren**

首先代码很复杂，所以在这里直接上图。

![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/diff1.png)

首先，在新老两个VNode节点的左右头尾两侧都有一个变量标记，在遍历过程中这几个变量都会向中间靠拢。当oldStartIdx > oldEndIdx或者newStartIdx > newEndIdx时结束循环。

首先，在新老两个VNode节点的左右头尾两侧都有一个变量标记，在遍历过程中这几个变量都会向中间靠拢。当oldStartIdx > oldEndIdx或者newStartIdx > newEndIdx时结束循环。

索引与VNode节点的对应关系： oldStartIdx => oldStartVnode oldEndIdx => oldEndVnode newStartIdx => newStartVnode newEndIdx => newEndVnode

在遍历中，如果存在key，并且满足sameVnode，会将该DOM节点进行复用，否则则会创建一个新的DOM节点。

首先，oldStartVnode、oldEndVnode与newStartVnode、newEndVnode两两比较一共有2*2=4种比较方法。

### sameVnode(oldStartVnode, newStartVnode)或者sameVnode(oldEndVnode, newEndVnode)

当新老VNode节点的start或者end满足sameVnode时，也就是sameVnode(oldStartVnode, newStartVnode)或者sameVnode(oldEndVnode, newEndVnode)，直接将该VNode节点进行patchVnode即可。

![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/diff2.png)

### sameVnode(oldStartVnode, newEndVnode)

如果oldStartVnode与newEndVnode满足sameVnode

这时候说明oldStartVnode已经跑到了oldEndVnode后面去了，进行patchVnode的同时还需要将真实DOM节点移动到oldEndVnode的后面

![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/diff3.png)

### sameVnode(oldEndVnode, newStartVnode)

如果oldEndVnode与newStartVnode满足sameVnode

这说明oldEndVnode跑到了oldStartVnode的前面，进行patchVnode的同时真实的DOM节点移动到了oldStartVnode的前面

![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/diff4.png)

### sameVnode(?, newStartVnode)

则通过createKeyToOldIdx会得到一个oldKeyToIdx，里面存放了一个key为旧的VNode，value为对应index序列的哈希表。从这个哈希表中可以找到是否有与newStartVnode一致key的旧的VNode节点，如果同时满足sameVnode，patchVnode的同时会将这个真实DOM（elmToMove）移动到oldStartVnode对应的真实DOM的前面。

![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/diff5.png)

当然也有可能newStartVnode在旧的VNode节点找不到一致的key，或者是即便key相同却不是sameVnode，这个时候会调用createElm创建一个新的DOM节点

![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/diff6.png)

到这里循环已经结束了，那么剩下我们还需要处理多余或者不够的真实DOM节点。

### 处理剩余节点

1. 当结束时oldStartIdx > oldEndIdx，这个时候老的VNode节点已经遍历完了，但是新的节点还没有。说明了新的VNode节点实际上比老的VNode节点多，也就是比真实DOM多，需要将剩下的（也就是新增的）VNode节点插入到真实DOM节点中去，此时调用addVnodes（批量调用createElm的接口将这些节点加入到真实DOM中去）。

![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/diff7.png)

2. 同理，当newStartIdx > newEndIdx时，新的VNode节点已经遍历完了，但是老的节点还有剩余，说明真实DOM节点多余了，需要从文档中删除，这时候调用removeVnodes将这些多余的真实DOM删除。

![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/diff8.png)

### 总结：

* 尽量不要跨层级的修改dom
* 设置key可以最大化的利用节点
* diff的效率并不是每种情况下都是最优的


# webpack的plugin与loader区别

**loader：**

用于对模块源码的转换，loader描述了webpack如何处理非javascript模块，并且在buld中引入这些依赖。loader可以将文件从不同的语言（如TypeScript）转换为JavaScript，SCSS转换为CSS

**plugin：**

对于plugin，它就是一个扩展器，它丰富了wepack本身，针对是loader结束后，webpack打包的整个过程，它并不直接操作文件，**而是直接对整个构建过程起作用。，**不仅局限于资源的加载。

# webpack 实现热更新的原理

Webpack 热更新（ Hot Module Replacement，简称 HMR，后续均以 HMR 替代），无需完全刷新整个页面的同时，更新所有类型的模块。

1. Webpack编译期，为需要热更新的 entry 注入热更新代码(EventSource通信，核心是Webpack-hot-middleware 插件的作用，就是提供浏览器和 Webpack 服务器之间的通信机制、且在浏览器端接收 Webpack 服务器端的更新变化。)

2. 页面首次打开后，服务端与客户端通过 EventSource 建立通信渠道，把下一次的 hash 返回客户端

3. 客户端获取到hash，这个hash将作为下一次请求服务端 hot-update.js 和 hot-update.json的hash

4. 修改页面代码后，Webpack 的Complier 类会调用 Watch 方法监听文件变更，当监听到文件修改后，开始编译，编译完成后，发送 build 消息给客户端

5. 客户端获取到hash，成功后客户端构造hot-update.js script链接，然后插入主文档

6. hot-update.js 插入成功后，执行hotAPI 的 createRecord 和 reload方法，获取到 Vue 组件的 render方法，重新 render 组件， 继而实现 UI 无刷新更新。


# 内置的过渡动画

* v-enter：定义进入过渡的开始状态。在元素被插入之前生效，在元素被插入之后的下一帧移除。

* v-enter-active：定义进入过渡生效时的状态。在整个进入过渡的阶段中应用，在元素被插入之前生效，在过渡/动画完成之后移除。这个类可以被用来定义进入过渡的过程时间，延迟和曲线函数。

* v-enter-to: 2.1.8版及以上 定义进入过渡的结束状态。在元素被插入之后下一帧生效 (与此同时 v-enter 被移除)，在过渡/动画完成之后移除。

* v-leave: 定义离开过渡的开始状态。在离开过渡被触发时立刻生效，下一帧被移除。

* v-leave-active：定义离开过渡生效时的状态。在整个离开过渡的阶段中应用，在离开过渡被触发时立刻生效，在过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，延迟和曲线函数。

* v-leave-to: 2.1.8版及以上 定义离开过渡的结束状态。在离开过渡被触发之后下一帧生效 (与此同时 v-leave 被删除)，在过渡/动画完成之后移除。

对于这些在过渡中切换的类名来说，如果你使用一个没有名字的 transition，则 v- 是这些类名的默认前缀。如果你使用了 <transition name="my-transition">，那么 v-enter 会替换为 my-transition-enter。

# vue 3.0 新特性

## 基于 Proxy 的观察者机制

在vue 3.0之前是使用Object.defineProperty，那为什么要使用Proxy替代Object.defineProperty呢，主要是Object.defineProperty存在以下问题：

1. 在Vue中，Object.defineProperty无法监控到数组下标的变化，导致直接通过数组的下标给数组设置值，不能实时响应。Vue 框架是**通过遍历数组 和递归遍历对象**，从而达到利用 Object.defineProperty() 也能对对象和数组（部分方法的操作）进行监听，部分方法如下：

* push()
* pop()
* shift()
* unshift()
* splice()
* sort()
* reverse()

由于只针对了以上7种方法进行了hack处理,所以其他数组的属性也是检测不到的，还是具有一定的局限性。

2. Object.defineProperty只能劫持对象的属性,因此我们需要对每个对象的每个属性进行遍历。Vue里，是通过递归以及遍历data 对象来实现对数据的监控的，如果属性值也是对象那么需要深度遍历,显然如果能劫持一个完整的对象，不管是对操作性还是性能都会有一个很大的提升。

所以使用Proxy的优点如下：

* 可以劫持整个对象，并返回一个新对象
* Proxy可以直接监听对象而非属性
* Proxy可以直接监听数组的变化
* Proxy有多达13种拦截方法,、ownKeys、deleteProperty、has等等是Object.defineProperty不具备的
* Proxy作为新标准将受到浏览器厂商重点持续的性能优化，也就是传说中的新标准的性能红利

**Object.defineProperty的优势**

* 兼容性好,支持IE9

## PWA 支持

当我们选择启用 PWA 功能时，在打包生成的代码时会默认生成 service-worker.js 和 manifest.json 相关文件。熟悉PWA的同学都知道service-worker.js 和 manifest.json 是PWA的重要配置文件

PWA(Progressive web apps, 渐进式Web应用)

* 可以生成桌面小图标，不需要打开浏览器，方便用户访问
* 通过网络缓存提升页面访问速度，达到渐进式的页面甚至离线访问，提升用户体验
* 实现类似app的推送功能，生成系统通知推送给用户

service worker是实现PWA的核心，service worker是一个独立的浏览器线程，不会对当前程序的执行线程造成阻塞，通过service worker可以实现页面离线访问、用户消息推送等功能。
使用 Service Worker的话，传输协议必须为 HTTPS。因为 Service Worker 中涉及到请求拦截，所以必须使用 HTTPS 协议来保障安全。

Service Worker 实现缓存功能一般分为三个步骤：

* 首先需要先注册 Service Worker
* 然后监听到 install 事件以后就可以缓存需要的文件
* 下次用户访问的时候就可以通过**拦截请求的方式**查询是否存在缓存，存在缓存的话就可以直接读取缓存文件，否则就去请求数据
4

PWA确实是当下很热门的技术，因为它提升了web应用的体验，甚至达到原生app体验，但是它的问题就是兼容性问题，相信如果兼容性问题得到解决，这种技术一定会被大面积推广到实际应用。iOS11.3之前都不支持，因此vue cli3脚手架在集成时默认在ios下是关闭的

## 使用图形化界面

你也可以通过 vue ui 命令以图形化界面创建和管理项目：

```javascript
  vue ui
```

## 项目改变

相比 vue-cli 2.X 创建的目录，vue-cli 3.0 创建的目录看不见 webpack 的配置
需要手动配置 webpack：在根目录下新建一个 vue.config.js 文件


## 其他

* 虚拟DOM重写
* 优化 slots 的生成：在 Vue 中，当父组件重新渲染时，其子组件也必须重新渲染。使用Vue 3，可以单独重新渲染父级和子级。
* 静态树提升(Static Tree Hoisting)
* 静态属性提升

# Vue框架怎么实现对象和数组的监听（源码）

如果被问到 Vue 怎么实现数据双向绑定，大家肯定都会回答 通过 Object.defineProperty() 对数据进行劫持，但是  Object.defineProperty() 只能对属性进行数据劫持，不能对整个对象进行劫持，同理无法对数组进行劫持，但是我们在使用 Vue 框架中都知道，Vue 能检测到对象和数组（部分方法的操作）的变化，那它是怎么实现的呢？我们查看相关代码如下：

```javascript
  /**
   * Observe a list of Array items.
   */
  observeArray (items: Array<any>) {
    for (let i = 0, l = items.length; i < l; i++) {
      observe(items[i])  // observe 功能为监测数据的变化
    }
  }

  /**
   * 对属性进行递归遍历
   */
  let childOb = !shallow && observe(val) // observe 功能为监测数据的变化

```
通过以上 Vue 源码部分查看，我们就能知道 Vue 框架是**通过遍历数组 和递归遍历对象**，从而达到利用 Object.defineProperty() 也能对对象和数组（部分方法的操作）进行监听。

# vue自定义指令

使用 Vue.directive(id, [definition]) 定义全局的指令来进行自定义指令

参数1 ：指令的名称，注意，在定义的时候，指令的名称前面，不需要加 v-前缀，但是： 在调用的时候，必须在置顶的名称前加上 v-前缀来进行调用

参数2：是一个对象， 这个对象身上，有一些指令相关的函数，这些函数可以在特定的阶段，执行相关的操作。

```javascript

Vue.directive("focus", {
    // 注意： 在每个函数中， 第一个参数永远是el， 表示被绑定了指令的那个元素，这个el参数，是一个原生的JS对象
    bind: function(el){ // 每当指令绑定到元素上的时候，会立即执行这个bind函数，只执行一次

    },
    inserted: function(el){ // inserted 表示元素插入到DOM中的时候，会执行inserted函数【触发一次】
        el.focus()
    },
    updated: function(el) { // 当VNode更新的时候，会执行updated，可能会触发多次
        
    },
})
```
调用：

```javascript
<!-- 注意： Vue中所有的指令，在调用的时候，都以 v- 开头 -->
<input type="text" class="form-control" v-model="keywords" v-focus>
```
# mixin 和 mixins 的区别

1. Vue.mixin

全局注册混入对象。注意使用！ 一旦使用全局混入对象，将会影响到 所有 之后创建的 Vue 实例。使用恰当时，可以为自定义对象注入处理逻辑。

2. mixins

mixins 选项接受一个混入对象的数组。这些混入实例对象可以像正常的实例对象一样包含选项，他们将在 Vue.extend() 里最终选择使用相同的选项合并逻辑合并。举例：如果你的混入包含一个钩子而创建组件本身也有一个，两个函数将被调用。

Mixin 钩子按照传入顺序依次调用，并在调用组件自身的钩子之前被调用

```javascript

var mixin = {
  created: function () { console.log(1) }
}
var vm = new Vue({
  created: function () { console.log(2) },
  mixins: [mixin]
})
// => 1
// => 2

```

# 如何渲染⼏万条数据并不卡住界⾯

1. 考察了如何在不卡住⻚⾯的情况下渲染数据，也就是说不能⼀次性将⼏ 万条都渲染出来，⽽应该⼀次渲染部分 DOM ，那么就可以通过 requestAnimationFrame 来每 16 ms 刷新⼀次

2. 通过用户的滚动行为来进行渲染

# **参考文章：**

[30 道 Vue 面试题，内含详细讲解（涵盖入门到精通，自测 Vue 掌握程度）](https://juejin.im/post/5d59f2a451882549be53b170)
[解析vue2.0的diff算法](https://github.com/aooy/blog/issues/2)
[VirtualDOM与diff(Vue实现)](https://github.com/answershuto/learnVue/blob/master/docs/VirtualDOM与diff(Vue实现))
