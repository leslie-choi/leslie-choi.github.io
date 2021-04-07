---
title: taro 入门
date: 2020-09-22 10:12:23
tags: taro
categories: JavaScript
---

# 项目初始化

1. 全局安装 tarojs/cli

`yarn global add @tarojs/cli`
`npm install -g @tarojs/cli`

2. 项目依赖安装

`taro init projectName`

然后根据提示选择对应需要安装工具

3. 项目运行

`npm run dev:h5`
`yarn dev:h5`

需要编译成其他端的代码，只需要更改命令后缀即可。比如微信小程序，只需要将 h5 修改为 weapp。
h5 端启动之后可以直接在浏览器端看到效果。
小程序则需要打开对应的开发者工具，打开 dist 目录查看效果。

**我们应该在 taro 源代码中进行维护，而不应该在编译之后的 dist 目录中修改代码。**

# taro 中使用 hooks 语法

```javascript
import Taro, { useState } from '@tarojs/taro'
// import Taro from '@tarojs/taro'
// import React, { useState } from 'react'
// 如果第一句 import 语句显示出 React is not defined，则需要分开引入
```

其他基本使用查看官方文档。

# taro 中子组件的编写和传值


# taro 中的路由配置

```javascript
// 路由跳转
// navigatorTo redirectTo switchTab navigateBack relaunch 
// getCurrentPage
```