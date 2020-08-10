<!--
 * @Description: 
 * @Author: 小鬼
 * @Date: 2020-08-07 18:08:08
 * @LastEditTime: 2020-08-10 15:02:30
 * @LastEditors: 小鬼
-->
---
title: vue 编码规范
date: 2020-08-05 20:43:20
tags: basic
categories: Vue
---

# Vue 项目结构规范

```javascript

|-- build                       # 构建相关
|-- mock                        # 项目 mock 模拟数据
|-- plop-templates              # 基本模板
|-- public                      # 静态资源
|   |-- favicon.ico             # favicon 图标
|   |-- index.html              # html 模板
|-- src                         # 源代码
|-- |-- api                     # 所有请求
|   |-- assets                  # 主题 字体等静态资源
|   |-- static                  # 存放类包
|   |-- components              # 全局公用组件
|   |-- directive               # 全局指令
|   |-- filters                 # 全局 filter
|   |-- icons                   # 项目所有 svg icons
|   |-- lang                    # 国际化 language
|   |-- layout                  # 全局 layout
|   |-- router                  # 路由
|   |-- store                   # 全局 store 管理
|   |-- styles                  # 全局样式
|   |-- utils                   # 全局公用方法
|   |-- vendor                  # 公用 vendor
|   |-- views                   # views 所有页面
|   |-- App.vue                 # 入口页面
|   |-- main.js                 # 入口文件 加载组件 初始化等
|   |-- permission.js           # 权限管理
|-- tests                       # 测试
|-- .env.xxx                    # 环境变量配置
|-- .eslintrc.js                # eslint 配置项
|-- .babelrc                    # babel-loader 配置
|-- .travis.yml                 # 自动化 CI 配置
|-- vue.config.js               # vue-cli 配置
|-- postcss.config.js           # postcss 配置
|-- package.json                # package.json

```

**static 目录和 assets 目录的区别**

1. assets 中资源会 webpack 构建压缩到你代码中，而 static 文件直接引用。

2. static 中长存放类包、插件等第三方的文件，assets 里放属资源文件比如自己资源图片、css 文件、js 文件。

3. 引入资源的方式static文件夹可以使用~/static/方式引入, assets文件夹可以使用 ~@/assets 方式引入。

**static 必须使用绝对路径引用这些文件；assets目录中的文件会被webpack处理解析为模块依赖，只支持相对路径形式。**

# Vue 命名规范

1. 文件夹命名规范

尽量使用连线符号

2. Vue 文件命名规范

* 大驼峰（PascalCase）命名。
* 组件注册同上。
* 在 html 模板中只能使用 kebab-case（短横线），因为 html 对大小写不敏感。
* 基础组件名，应该全部以一个特定的前缀开头（如 Base、App、V 等等）

```javascript

// HelloWorld.vue
components: {
  HelloWorld,
  BaseTable
}

<hello-world>
<base-table>

```

3. 组件名为多个单词

组件名应该始终是多个单词的，根组件 App 除外。

```javascript

// good
export default {
  name: 'TodoItem'
}

// bad
export default {
  name: 'Todo'
}

```

4. 组件中的 data 必须是一个函数

当在组件中使用 data 属性的时候 (除了 new Vue 外的任何地方，因为不会被复用，不存在引用对象的问题)，它的值必须是返回一个对象的函数。（**保证组件之间的作用域相互隔离**）

5. prop 的定义必须尽量详细，至少需要指定其类型

```javascript

// good
props: {
  status: String
}

// better
props: {
  status: {
    type: String,
    required: true,
    validator: function (value) {
      return [
        'syncing',
        'error'
      ].indexOf(value) !== -1
    }
  }
}

// bad
// 这样做只有开发原型系统时可以接受
props: ['status']

```

6. 避免 V-for 和 V-if 混用

* 为了过滤一个列表中的项目 (比如 v-for="user in users" v-if="user.isActive")。在这种情形下，请将 users 替换为一个计算属性 (比如 activeUsers)，让其返回过滤后的列表。

* 为了避免渲染本应该被隐藏的列表 (比如 v-for="user in users" v-if="shouldShowUsers")。这种情形下，请将 v-if 移动至容器元素上 (利用事件委托的原理，比如 ul, ol)。

```javascript

// good
<ul v-if="shouldShowUsers">
  <li
    v-for="user in users"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>

// bad
<ul>
  <li
    v-for="user in users"
    v-if="shouldShowUsers"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>

```

7. 单例组件名（todo）

8. 紧密耦合的组件名

和父组件紧密耦合的子组件应该以父组件名作为前缀命名。

如果一个组件只在某个父组件的场景下有意义，这层关系应该体现在其名字上。因为编辑器通常会

按字母顺序组织文件，所以这样做可以把相关联的文件排在一起。

```javascript

// good

components/
|- TodoList.vue
|- TodoListItem.vue
|- TodoListItemButton.vue
components/
|- SearchSidebar.vue
|- SearchSidebarNavigation.vue

```

9. 多个属性单词分行撰写

```javascript

// good
<img
  src="https://vuejs.org/images/logo.png"
  alt="Vue Logo"
>
<MyComponent
  foo="a"
  bar="b"
  baz="c"
/>

// bad
<img src="https://vuejs.org/images/logo.png" alt="Vue Logo">
<MyComponent foo="a" bar="b" baz="c"/>

```

10. scoped 中的元素选择器

元素选择器应该避免在 scoped 中出现。

**在 scoped 样式中，类选择器比元素选择器更好，因为大量使用元素选择器是很慢的。**

```javascript

// good

<template>
  <button class="btn btn-close">X</button>
</template>

<style scoped>
.btn-close {
  background-color: red;
}
</style>

// bad

<template>
  <button>X</button>
</template>

<style scoped>
button {
  background-color: red;
}
</style>

```

# 参考文章

[Vue前端开发规范](https://juejin.im/post/6844903593896574990)