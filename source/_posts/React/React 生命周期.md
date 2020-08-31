---
title: React 生命周期函数
date: 2020-08-31 11:26:36
tags: basic
categories: react
---

# React 生命周期函数

**React 生命周期分为三种状态：初始化（挂载） -> 更新 -> 销毁**

## 初始化(挂载)

当组件实例被创建并插入 DOM 中的时候，其生命周期的调用顺序如下：

* constructor ()  常用

* static getDerivedStateFromProps ()

* render () 常用/必须

* componentDidMount ()  常用

**UNSAFE_componentWillMount()在 v17.0 之后将会被抛弃，在新的项目中应该避免使用到**

```javascript
// 1. 组件挂载的时候执行，只会触发一次
constructor(props) {
  super(props)
  this.state = {}
}

// 2. 该生命周期必须使用 static 关键字前缀 在初始挂载和后续的更新都会被调用
// 他应该返回一个对象来更新 state，如果返回 null 则不更新任何函数
static getDerivedStateFromProps() {
  console.log('getDerivedStateFromProps')
  return ''
}

// 3. 返回一个 jsx 对象，然后由 React 库根据返回对象决定如何渲染，并不是返回一个真实的 DOM，这个生命周期函数是必须存在。
render() {
  return (
    <div>render</div>
  )
}

// 4. 这个时候页面已经存在了真实的 dom 节点，所以这个阶段可以发送异步请求，一般用于处理接口请求，或者执行一些 DOM操作。因为有些组件的操作是需要依赖 DOM 操作的，例如动画等。

// 建议：发送 ajax 请求
// 不建议：直接使用 this.setState() 更新状态，这样有可能导致 DOM 的二次渲染，造成性能的浪费。(但是经常会有发送请求之后，在回调函数里面调用 setState 设置 state，这却是不可避免的)
componentDidMount() {

}
```

## 更新

当组件的 props 或 state 发生变化时会触发更新。组件更新的生命周期调用顺序如下：

static getDerivedStateFromProps() -> shouldComponentUpdate() -> render() -> getSnapshotBeforeUpdate() -> componentDidUpdate()

```javascript
// 1. getDerivedStateFromProps()，该方法适用于罕见的用例，即 state 的值在任何时候都取决于 props。
// 会在调用 render 方法之前调用，并且在初始挂载及后续更新时都会被调用。
// 它应返回一个对象来更新 state，如果返回 null 则不更新任何内容。

static getDerivedStateFromProps (props, state) {
}

// 2. shouldComponentUpdate() 根据其返回值，判断 React 组件的输出是否受当前 state 或 props 更改的影响。
// 默认行为是 state 每次发生变化组件都会重新渲染。大部分情况下，你应该遵循默认行为。
// 当 props 或 state 发生变化的时候，shouldComponentUpdate() 会在渲染执行之前被调用，返回值默认是 true。
// 首次渲染或者父组件通过 forceUpdate 重新执行 render，不会调用当前组件的 shouldComponentUpdate。

shouldComponentUpdate(nextProps, nextState) {
}

// 3. render 函数同上
render() {
  return ()
}

// 4. getSnapshotBeforeUpdate()，此方法并不常用。
// 在最近一次渲染输出（提交到 DOM 节点）之前调用。
// 它使得组件能在发生更改之前从 DOM 中捕获一些信息（例如，滚动位置）。
// 此生命周期的任何返回值将作为参数传递给 componentDidUpdate()
// 可能出现在 UI 处理中，如需要以特殊方式处理滚动位置的聊天线程等。
getSnapshotBeforeUpdate() {
}

// 5. componentDidUpdate() 会在更新之后被立即调用。首次渲染不会执行该方法。
// 当组件更新后，可以在此处对 DOM 进行操作。
// 如果你对更新前后的 props 进行了比较，也可以选择在此处进行网络请求。（例如，当 props 未发生变化时，则不会执行网络请求）。
componentDidUpdate() {
}
```

## 销毁

```javascript
componentWillUnmount() {
  // 该生命周期将会在 v17.0 版本中弃用
  // 当 react 组件要从 dom 树上删除掉的时候，这个方法就会被调用。
  // 如果在 componentDidMount 中使用非 React 的方法创造了一些 DOM 元素，如果撒手不管可能会造成内存泄漏，那就需要在 componentWillUnmount中把这些创造的DOM元素清理掉。
  // 应用场景：清理定时器，关闭 socket, 清除监听器等
  clearInterva()
}
```

**注意：不应调用 setState()，因为该组件将永远不会重新渲染。组件实例卸载后，将永远不会再挂载它。**


# 参考文章

(React(v16.8.4)生命周期详解)[https://www.cnblogs.com/hezhi/p/10547186.html]
