---
title: Redux 的基本使用
date: 2020-09-15 21:12:23
tags: React
categories: JavaScript
---

# Redux 的基本使用

1. ReactComponents 组件需要获取 Store 数据的时候，通知 ActionCreators。
2. ActionCreators 通过 dispatch(action) 通知到 Store。
3. Store 中的数据需要通过 Reducers 去查找匹配。
4. Reducers 通知 Store 需要返回给 ReactComponents 的 newState 数据。

# todoList 例子

首先命令行 `yarn add redux`

1. 在 src 目录下新建 store 目录，store 目录下 新建 index.js。

index.js

```javascript
import { createStore } from 'redux'
// reducer 负责管理数据
import reducer from './reducer'

// 创建数据存储的公共仓库
const store = createStore(
  reducer,
  // 使用 redux 开发者工具需要加入下一行
  window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
)

export default store
```

2. store 目录新建 reducer.js 文件。

reducer.js

```javascript
const defaultState = {
  inputValue: '233',
  list: [1,2,3]
}
// reducer 负责存储和管理数据
export default (state = defaultState, action) => {
  return state
}
```

3. todoList 中使用 store 中的数据。

todoList.js

```javascript
// 会默认导入 store 目录下的 index文件
import store from './store'
import { Input, Button, List } from 'antd'

class TodoList extends React.Component {
  constructor(props) {
    super(props)
    // 获取 store 中的 state 数据
    this.state = store.getState()
    // 表示 todoList 这个组件去订阅 store
    store.subscribe(this.handleStoreChange)
  }
  render() {
    return (
      <div style={{ marginTop: '15px'}}>
        <Input
          onChange={this.handleInputChange}
          value={this.state.inputValue}
          />
        <Button
          onClick={this.handleClick}
        >click me</Button>
        <List
          dataSource={this.state.list}
          renderItem={item => (
            <List.Item>
              {item}
            </List.Item>
          )}
        />
      </div>
    )
  }
  // input 输入框改变值之后，便 dispatch 一个 action 到 store
  handleInputChange = (e) => {
    const action = {
      type: 'change_input_value',
      value: e.target.value
    }
    store.dispatch(action)
  }
  // 当感知到 store 中的数据发生变化的时候，那么 state 重新从 store 取一次数据
  // 同步当前组件中 state 和 store 的数据
  handleStoreChange = () => {
    this.setState(store.getState())
  }
  // 点击按钮之后，提交并更新 store 中的数据
  handleClick = () => {
    const action = {
      type: 'add_todo_item'
    }
    store.dispatch(action)
  }
}
```

# TodoList 例子优化

在规模较大的项目中，action 的创建应该由一个专门的 actionCreator 文件负责创建。actionType 的类型也需要由一个专门的 actionType 文件进行管理，方便调试。

1. 在 store 目录下，新建 actionTypes.js 文件

```javascript
export const CHANGE_INPUT_VALUE = 'change_input_value'
export const ADD_TODO_ITEM = 'add_todo_item'
export const DELETE_TODO_ITEM = 'delete_todo_item'
```

2. 在 store 目录下，新建 actionCreators.js 文件

每个方法可以理解为一个 action。

```javascript
import { CHANGE_INPUT_VALUE, ADD_TODO_ITEM, DELETE_TODO_ITEM} from './actionTypes'

export const getInputChangeAction = (value) => ({
  type: CHANGE_INPUT_VALUE,
  value
})
export const getAddItemAction = () => ({
  type: ADD_TODO_ITEM
})
export const getDeleteItemAction = (index) => ({
  type: DELETE_TODO_ITEM,
  index
})
```

3. 在 store 目录下新建 reducer.js

根据 actionType 确认要返回的数据。

```javascript
import { CHANGE_INPUT_VALUE, ADD_TODO_ITEM, DELETE_TODO_ITEM } from './actionTypes'
const defaultState = {
  inputValue: '233',
  list: [1,2,3]
}
// reducer 负责存储和管理数据
// reducer 中的 state 指的是上一次存储的数据，action 指的是用户传递过来的行为
// reducer 有一个原则，就是只能接收 state 不能修改 state
export default (state = defaultState, action) => {
  // 接下来进行类型判断
  if (action.type === CHANGE_INPUT_VALUE) {
    // 深拷贝
    const newState = JSON.parse(JSON.stringify(state))
    newState.inputValue = action.value
    return newState
  }
  if ( action.type === ADD_TODO_ITEM ) {
    const newState = JSON.parse(JSON.stringify(state))
    newState.list.push(newState.inputValue)
    newState.inputValue = ' '
    return newState
  }
  if (action.type === DELETE_TODO_ITEM) {
    const newState = JSON.parse(JSON.stringify(state))
    newState.list.splice(action.index, 1)
    return newState
  }
  return state
}
```

4. src 目录下新建 TodoList.js 文件

```javascript
import React from 'react'
import 'antd/dist/antd.css'
import { Input, Button, List } from 'antd'
import store from './store'
import { getInputChangeAction, getAddItemAction, getDeleteItemAction } from './store/actionCreators'

class TodoList extends React.Component {
  constructor(props) {
    super(props)
    this.state = store.getState()
    // 表示 todoList 这个组件去订阅 store，才能实现页面组件的更新
    // 只要 store 的数据发生改变，就执行传入的方法
    store.subscribe(this.handleStoreChange)
  }
  render() {
    return (
      <div style={{ marginTop: '15px'}}>
        <Input
          onChange={this.handleInputChange}
          value={this.state.inputValue}
          placeholder="input info"
          style={{width:'300px', marginRight: '30px'}}/>
        <Button
          type="primary"
          onClick={this.handleClick}
          style={{marginTop: '10px'}}
        >click me</Button>
        <List
          style={{marginTop: '10px', width: '350px'}}
          bordered
          onClick={this.handleItemDelete}
          dataSource={this.state.list}
          renderItem={item => (
            <List.Item>
              {item}
            </List.Item>
          )}
        />
      </div>
    )
  }
  handleInputChange = (e) => {
    const action = getInputChangeAction(e.target.value)
    store.dispatch(action)
  }
  // 当感知到 store 中的数据发生变化的时候，那么 state 重新从 store 取一次数据
  // 同步当前组件中 state 和 store 的数据
  handleStoreChange = () => {
    this.setState(store.getState())
  }
  handleClick = () => {
    const action = getAddItemAction()
    store.dispatch(action)
  }
  handleItemDelete(index) {
    const action = getDeleteItemAction(index)
    store.dispatch(action)
  }
}
export default TodoList
```

# Redux 中发送异步请求获取数据

1. actionsTypes.js

```javascript
export const INIT_DATA_LIST = 'init_data_list'
```

2. actionCreators.js

```javascript
import { INIT_DATA_LIST } from './actionTypes'

export const getDataList = (data) => ({
  type: INIT_DATA_LIST,
  data
})
```

3. reducer.js

```javascript
import { INIT_DATA_LIST  } from './actionTypes'
const defaultState = {
  inputValue: '',
  list: []
}
export default (state = defaultState, action) => {
  if (action.type === INIT_DATA_LIST) {
    const newState = JSON.parse(JSON.stringify(state))
    newState.list = action.data
    return newState
  }
  return state
}
```

4. todoList.js

在 todoList 中的生命周期

```javascript
import { getDataList } from './store/actionCreators'

componentDidMount() {
  axios.get('/api/todoList').then((res) => {
    const { data } = res
    const action = getDataList(data)
    store.dispatch(action)
  })
}
```

# redux-thunk 的使用

使用 redux-thunk 之后在 actionCreators 中可以不再返回一个对象，而是可以返回一个函数，可以在这个函数中异步获取数据。
但是使用之前需要在 index.js 中配置，才能保证 redux-thunk 和 react-devTools 同时使用。

1. store 目录下的 index.js 文件。

```javascript
import { createStore, applyMiddleware, compose } from 'redux'
import thunk from 'redux-thunk'
// reducer 负责管理数据
import reducer from './reducer'

// 创建数据存储的公共仓库
const composeEnhancers = typeof window === 'object' && window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ ? window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({}) : compose
const enhancer = composeEnhancers(
  applyMiddleware(thunk)
)
const store = createStore(reducer, enhancer)

export default store
```

2. actionCreators.js 文件

```javascript
export const getInitDataList = (data) => ({
  type: INIT_DATA_LIST,
  data
})

export const getTodoList = () => {
  return (dispatch) => {
    axios.get('/api/todoList').then((res) => {
      const data = res.data
      const action = getInitDataList(data)
      dispatch(action)
    })
  }
}
```

3. todoList.js 文件

```javascript
import { getTodoList } from './store/actionCreators'

componentDidMount() {
  const action = getTodoList()
  store.dispatch(action)
}
```

流程：

生命周期 componentDidMount 执行，将 action 传给 store -> 传递给 store 的 action 是一个函数，在这个函数中异步获取数据 -> 获取数据之后，需要改变 store 中的数据，使用 getInitDataList 创建一个 action，下一步则需要将数据传递给 store。

使用 redux-thunk 之前 dispatch 只能接收一个对象，使用 redux-thunk 之后，可以接收一个函数或者一个对象。但是实际上也是在这个函数中处理对象。



# 设计原则

1. store 是唯一的。
2. 只有 store 能改变自己的数据，并不是 reducer 对数据进行的更新。Reducer 对 store 中的数据进行更改之后，返回一个全新的数据给 store，store 拿到 reducer 返回的数据之后对原来的数据进行更新。所以 reducer 不能直接修改 store 的数据。
3. reducer 必须是一个**纯函数**，不会有副作用。并且给了一个固定的输入之后，必须会有一个固定的输出。比如当 state 和 action 固定了之后，输出的值就是固定的。如果 newState 受到当前时间的影响(定时器、异步请求或者和日期相关内容的时候)，那就不是一个纯函数。副作用是指对传入的参数进行修改，比如 `state.inputValue = action.value`。

**核心概念**
createStore - store.dispatch - store.getState - store.subscribe

