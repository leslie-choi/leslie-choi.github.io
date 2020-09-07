---
title: JSX 语法基础
date: 2020-09-07 21:26:36
tags: basic
categories: react
---

# JSX 语法

**JSX 是 React.createElement语法糖，**也是 JavaScript 的语法拓展，可以使用它来进行 UI 显示。简单的说，就是可以在 JavaScript 代码中写 html，类似 XML 语法

在使用的时候，一般是在组件的 render 方法里使用 JSX 进行布局和事件的绑定。

```javascript
class Home extends Component {
  render() {
    return (
      <div onclick = {() => console.log('233')}>
        <h1>JSX 语法</h1>
      </div>
    )
  }
}
```

React 的核心机制之一就是可以创建虚拟 DOM 元素，利用虚拟 DOM 来减少对实际 DOM 的操作，从而提升性能，虚拟 DOM 实质就是一个对象，而 JSX 正是为了创建虚拟 DOM 而存在的语法糖。

# 转换

```javascript
function getGreeting(name) {
  return (
    <div className = "greeting">
      <h1 foo="bar">Hello</h1>
      <h2> Good to see you {name} </h2>
    </div>
  )
}
```

由于目前的浏览器还不支持 JSX，所以在运行之前必须转换为普通的　JavaScript。而这项工作，一般都是由 Babel 进行转译的，编译后的 getGreeting 函数，会被编译成以下代码：

```javascript
function getGreeting(name) {
  return React.createElement (
    "div",
    { className: "greeting" },
    React.createElement("h1", { foo: "bar" }, "Hello!"),
    React.createElement("h2", null, "Good to see you ", name)
  )
}
```


# 参考文章
(JSX 这么6？)[https://www.zcfy.cc/article/jsx-can-do-that]