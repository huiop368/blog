title: Redux 源码解析
date: 2016-05-23 23:13:59
tags: redux
---

我们先从一个简单的单纯的Redux开始吧 （这里认为你对redux已经有部分了解）

```javascript
import { createStore } from 'redux'

// 定义reducer函数，处理数据变化
const counter = (state = 0, action) => {
  switch (action.type) {
      case 'INCREMENT':
        return state + 1
      case 'DECREMENT':
        return state - 1
      default:
        return state
  }
}

// 创建一个store对象
let store = createStore(counter)

// 监听触发action时需要响应的事件
store.subscribe(() =>
  console.log(store.getState())
)

// 触发action, 即click一个button时你想触发什么action
// 页面初始化时，你想触发什么action
store.dispatch({ type: 'INCREMENT' })
// 1
store.dispatch({ type: 'INCREMENT' })
// 2
store.dispatch({ type: 'DECREMENT' })
// 1

```
<!--more-->

所以一个redux的过程应该是 createStore -> subscribe -> dispatch

那我们一步一步来看，这些操作到底背后做了什么?
