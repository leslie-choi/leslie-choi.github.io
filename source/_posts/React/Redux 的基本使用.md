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
