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

3. 引入资源的方式static文件夹可以使用~/static/方式引入, assets文件夹可以使用 ~@/assets 方式引入

**static 必须使用绝对路径引用这些文件；assets目录中的文件会被webpack处理解析为模块依赖，只支持相对路径形式。**

# Vue 命名规范

1. 文件夹命名规范

尽量使用连线符号

2. Vue 文件命名规范

* 大驼峰（PascalCase）命名。
* 组件注册同上。
* 在 html 模板中只能使用 kebab-case（短横线），因为 html 对大小写不敏感。

```javascript

// HelloWorld.vue
components: {
  HelloWorld
}

<hello-world>

```