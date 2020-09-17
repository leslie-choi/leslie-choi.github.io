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

# 注意点

**在 React 项目中，如果使用到了 JSX 语法，则必须 `import React from React`**

1. 关键字

由于 JSX 就是 JavaScript，一些标识符比如 class 和 for 不建议作为 XML 的属性名。作为替代，React 使用 className 和 htmlFor 来做对应的属性。

eg:  点击 label 区域，光标自动定位到 input 输入框，即实现光标聚焦功能。

```javascript
<label for="inserArea" class="inserArea">输入内容</label>
{/* 以上代码在 react 中会报错。需要使用以下写法 */}
<label htmlFor="inserArea" className="inserArea">输入内容</label>

<input id="inserArea" />
```

2. if / else

在 JSX 中不能使用 if else 语句，但是可以使用三元运算符表达式替代。

```javascript
ReactDOM.render(
    <div>
      <h1>{i == 1 ? 'True!' : 'False'}</h1>
    </div>,
    document.getElementById('example')
)
```

3. 注释

```javascript
// 单行注释
{/*注释...*/}

// 多行注释
{/*
  *
  */
}
```

JSX　的注释需要写在花括号中，如　`{/*注释...*/}` 。
如果是多行注释，则需要使用

4. 样式

React 推荐使用内联样式。我们可以使用 camelCase 语法来设置内联样式. React 会在指定元素数字后自动添加 px 。
以下实例演示了为 h1 元素添加 myStyle 内联样式：

```javascript
var myStyle = {
    fontSize: 100,
    color: '#FF0000'
}
ReactDOM.render(
    <h1 style = {myStyle}>菜鸟教程</h1>,
    document.getElementById('example')
)
```

5. 数组

JSX 允许在模板中插入数组，数组会自动展开所有成员：

```javascript
var arr = [
  <h1>菜鸟教程</h1>,
  <h2>学的不仅是技术，更是梦想！</h2>,
]
ReactDOM.render(
  <div>{arr}</div>,
  document.getElementById('example')
)
```

会将 h1 和 h2 标签一同渲染出来。

6. 标签名

JSX 中引入组件名必须使用大写开头。

```javascript
import ReactDom from 'react-dom'
ReactDom.render(<App />, document.getElementById('root'))
```

7. 补充点

* Fragment 占位符

在 render 函数，返回值外面必须使用一对标签将其包裹起来，在不使用 div 进行包裹的时候，可以使用 Fragment 占位符，代替 div 标签，在实际的 DOM 中并不会被渲染到页面中。

```javascript
import { Fragment }from 'react'

render() {
  return(
    <Fragment>
      <h1>JSX</h1>
    </Fragment>
  )
}
```

* 默认转义 

不需要转义，则使用 dangerousSetInnerHTML = {{ __html: item }}，但是会涉及到 xss 攻击。

```javascript
class TodoList extends  React.Component {
  constructor(props) {
    super(props)
    this.state = {
      list: ['react', 'vue', 'angular']
    }
  }
  
  render() {
    return (
      <Fragment>
        <h1>TodoList</h1>
        <input 
          className = 'input'
          value = { this.state.inputValue }
          onChange = { this.handleInputChange }
        />
        <button onClick = { this.handleBtnClick }>提交</button>
        <ul>
          {
            this.state.list.map((item, index) => {
              return (
                <li
                  key={ index }
                  dangerousSetInnerHTML = {{ __html: item }}
                >
                {/* 如果不需要转义，则使用 dangerousSetInnerHTML，并且将下一行代码注释即可 */}
                  {item}
                </li>
              )
            })
          }
        </ul>
      </Fragment>
    )
  }
  handleInputChange = (e) => {
    // 使用箭头函数可以解决 this 指向 undefined 的问题
    // 否则需要在调用该方法的时候绑定 this
    // onChange = { this.handleInputChange.bind(this) }
    // 修改 state 中的数据必须使用 setState 方法
    this.setState({
      inputValue: e.target.value
    })
  }
  handleBtnClick = () => {
    this.setState({
      list: [...this.state.list, this.state.inputValue],
      inputValue: ''
    })    
  }
  handleDel = (index) => {
    // 不推荐直接使用 this.state.list.splice(idnex, 1) 修改 state 中的数据
    //  必须使用 react 提供的 setState 进行数据的修改， 表现出 react 的 immutable 的特性
    const list = [...this.state.list]
    list.splice(index, 1)
    this.setState({
      list: list
    })
  }
}
```

# 参考文章

(JSX 这么6？)[https://www.zcfy.cc/article/jsx-can-do-that]