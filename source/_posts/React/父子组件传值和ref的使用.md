---
title: React 父子组件传值和 ref 的使用
date: 2020-09-08 11:52:23
tags: basic
categories: react
---

# React 父子组件之间传值

eg： TodoList

1. 父组件 TODOList.js

```javascript
import React , { Fragment } from 'react'
import TodoItem from './TodoItem.js'

Class TodoList extends React.component {
  constructor(props) {
    super()
    this.state = {
      list: ['Vue', 'React', 'Angular']
    }
    // 一般绑定 this 的操作放在 constructor 中，可以节省性能
    this.handleDel = this.handleDel.bind(this)
  }

  render() {
    return (
      <Fragment>
        <h1>TodoList</h1>
        <ul>
          { this.getTodoItem() }
        </ul>
      </Fragment>
    )
  }
  getTodoItem = () => {
    return this.state.list.map((item,index) => {
      return (
        <div key = { index }>
          <TodoItem
            content = { item }
            index = { index }
            deleteItem = { this.handleDel }
          />
        </div>
      )
    })
  }
  handleDel = (index) => {
    // 旧用法： this.setState({})
    // prevState 相当于 this.state
    this.setState((prevState) => {
      const list = [...prevState.list]
      list.splice(index, 1)
      return {
        list
      }
    })
  }
}
```

2. 子组件

```javascript
import React from 'react'

class TodoItem extends React.Component {
  constructor(props) {
    super()
    this.state = {}
  }
  render() {
    return (
      <div onClick = { this.handleDelItem }>
        { this.props.content }
      </div>
    )
  }
  handleDelItem = () => {
    // 解构赋值提取
    const { deleteItem, index } = this.props
    deleteItem( index )
  }
}
```

**父组件向子组件传值**

* 引用子组件的时候，添加一个自定义属性 `content`，后面跟上需要传递的值。
* 子组件中，使用 `this.props.content` 就可以取到父组件传递过来的值。

**子组件向父组件传值**

PS：必须符合单向数据流的原则。

* 引用子组件的时候，使用自定义属性并绑定父组件的一个方法，父组件中的方法操作相关的数据。
* 注意在父组件中执行相关的数据操作的方法时，需要给该方法绑定 this。

3. 限制父组件传递数据类型。

在 TodoItem 组件中，使用 propTypes。

```javascript
import PropTypes from 'prop-types'

TodoItem.prototypes = {
  test: PropTypes.string.isRequired,  // isRequired 说明该参数是必须的
  content: PropTypes.oneOfType([PropTypes.string, PropTypes.number]),     // arrayOf 可以传入不同的类型
  deleteItem: PropTypes.func,
  index: PropTypes.number
}

TodoItem.defaultProps = {
  test: 'hello world'
}
```

当 props 或者 state 发生改变的时候，render 函数就会被重新执行。
当父组件的 render 函数被运行的时候，它的子组件的 render 函数都将被重新运行。

传统方案的页面渲染:

1. state 数据
2. JSX 模板
3. 数据 + 模板 结合，生成真实 DOM 来显示
4. state 发生改变
5. 数据 + 模板 结合，生成真实 DOM，直接替换原始的 DOM

缺陷：

* 第一次生成可一个完整 DOM 片段
* 第二次生成了一个完整 DOM 片段
* 第二次的 DOM 替换第一的 DOM，非常耗性能

改进方案：

1. state 数据
2. JSX 模板
3. 数据 + 模板结合，生成真实的 DOM，并不直接替代原始的 DOM
4. state 发生改变
5. 数据 + 模板结合，生成真实 DOM，并不直接替换原始的 DOM
6. 新的 DOM（DocumnentFragment） 和真实的 DOM 做对比，找出差异
7. 上例中找出 input 框发生的变化
8. 只用新的 DOM 中的 input 元素，替换掉老的 DOM 中的 input 元素

缺陷：

* 对比真实 DOM 时也会消耗性能，对于性能的提升并不明显。

现行方案：

1. state 数据
2. JSX 模板
3. 数据 + 模板 生成虚拟 DOM(虚拟 DOM 就是一个 JS 对象，用来描述真实 DOM)(损耗性能较小)
`['div', {id: 'abc'},['span', {}, 'hello world']]`
4.  用虚拟 DOM 的结构生成真实 DOM 来显示
`<div id='abc'><span>hello world</span></div>`
5. state 发生变化
6. 数据 + 模板生成新的虚拟 DOM
`['div', {id: 'abc'},['span', {}, 'bye bye']]`
7. 比较原始虚拟 DOM 和新的虚拟 DOM 的区别，找出区别(极大提升性能)
8. 直接操作　DOM，修改 span 标签的内容

减少了真实 DOM 的创建和真实 DOM 的对比，提高性能。

优点:

1. 提高性能
2. 使得跨端应用得以实现 react native
**虚拟 DOM 的存在,可以在浏览器端被渲染成真实 DOM 节点,也可以在原生应用中被渲染成真实组件**

调用 setState 方法的时候，数据才会发生变化，然后虚拟 dom 才会重新比对。
setState 实际上是一个异步的方法，为了提升 react 底层的性能。(如果调用 3 次 setState 方法，变更三组数据，react 并不会做 3 次虚拟 DOM 的比对，而是整合到一起)

diff 算法主要是同级比对，如果第一级不同但是二三级节点相同，dom 节点并不会被复用。虽然不会复用造成浪费，但是算法简单效率更高。

# Ref 的使用

用来获取 DOM 节点。

```javascript
render() {
  return (
    <Fragment>
      <input
        ref = {(input) => this.myInput = input }
      >
      </input>
    </Fragment>
  )
}
```

`this.myInput.value` 等同于 `e.target.value`

在 React 中不建议过度使用 Ref。在搭配 Ref 和 setState 的过程中，存在问题即：**setState 是异步操作的，所以 ref 获取到 dom 节点有可能不是最新的。如果想要在异步操作之后再执行其他动作，需要在 setState 的第二个参数中传入回调函数。**

```javascript
handleBtnClick() {
  console.log(this.myInput)
  this.setState((prevStatte) => {
    list: [],
    inputValue: ''
  },() => {  })
}
```