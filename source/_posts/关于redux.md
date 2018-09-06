---
title: 关于redux
date: 2018-09-06 09:50:02
tags:
- redux
- redux-thunk
categories:
- react
---

>前言：如果你的应用没那么复杂，就没必要用它。另一方面，Redux 只是 Web 架构的一种解决方案，也可以选择其他方案。
### 适用场景
redux的适用场景：`多交互/多数据源`
* 某个组件的状态，需要共享
* 某个状态需要在任何地方都可以拿到
* 一个组件需要改变全局状态
* 一个组件需要改变另一个组件的状态
### 设计思想
* Web 应用是一个状态机，视图与状态是一一对应的。
* 所有的状态，保存在一个对象里面。
### 基本概念
1. store
`Store`就是保存数据的地方，你可以把它看成一个容器。整个应用只能有一个 `Store`。
Redux 提供`createStore`这个函数，用来生成 Store。
```javascript
import { createStore } from 'redux';

const store = createStore(fn);
```
上面代码中，`createStore`函数接受另一个函数作为参数，返回新生成的 Store 对象。
2. state
Store 对象包含所有数据。如果想得到某个时点的数据，就要对 Store 生成快照。这种时点的数据集合，就叫做 `State`。

当前时刻的 `State`，可以通过`store.getState()`拿到。
```javascript
import { createStore } from 'redux';
const store = createStore(fn);

const state = store.getState();
```
Redux 规定， 一个 `State` 对应一个 View。只要 `State` 相同，View 就相同。你知道 `State`，就知道 View 是什么样，反之亦然。
3. action
State 的变化，会导致 View 的变化。但是，用户接触不到 State，只能接触到 View。所以，State 的变化必须是 View 导致的。`Action` 就是 View 发出的通知，表示 State 应该要发生变化了。

`Action` 是一个对象。其中的`type`属性是必须的，表示 Action 的名称。其他属性可以自由设置，[社区](https://github.com/redux-utilities/flux-standard-action)有一个规范可以参考。
```javascript
const action = {
  type: 'ADD_TODO',
  payload: 'Learn Redux'
};
```
上面代码中，`Action` 的名称是`ADD_TODO`，它携带的信息是字符串`Learn Redux`。

可以这样理解，`Action` 描述当前发生的事情。改变 State 的唯一办法，就是使用 `Action`。它会运送数据到 Store。
4. Action Creator
View 要发送多少种消息，就会有多少种 Action。如果都手写，会很麻烦。可以定义一个函数来生成 Action，这个函数就叫 `Action Creator`。
```javascript
const ADD_TODO = '添加 TODO';

function addTodo(payload) {
  return {
    type: ADD_TODO,
    payload
  }
}

const action = addTodo('Learn Redux');
```
上面代码中，`addTodo`函数就是一个 `Action Creator`。
5. store.dispatch()
`store.dispatch()`是 View 发出 Action 的唯一方法。
```javascript
import { createStore } from 'redux';
const store = createStore(fn);

store.dispatch({
  type: 'ADD_TODO',
  payload: 'Learn Redux'
});
```
上面代码中，store.dispatch接受一个 Action 对象作为参数，将它发送出去。

结合 Action Creator，这段代码可以改写如下。
```javascript
store.dispatch(addTodo('Learn Redux'));
```
6. reducer
Store 收到 Action 以后，必须给出一个新的 State，这样 View 才会发生变化。这种 State 的计算过程就叫做 `Reducer`。

`Reducer` 是一个函数，它接受 Action 和当前 State 作为参数，返回一个新的 State。
整个应用的初始状态，可以作为 State 的默认值。下面是一个实际的例子。
```javascript
const defaultState = 0;
const reducer = (state = defaultState, action) => {
  switch (action.type) {
    case 'ADD':
      return state + action.payload;
    default: 
      return state;
  }
};

const state = reducer(1, {
  type: 'ADD',
  payload: 2
});
```
上面代码中，`reducer`函数收到名为ADD的 Action 以后，就返回一个新的 State，作为加法的计算结果。其他运算的逻辑（比如减法），也可以根据 Action 的不同来实现。

实际应用中，`Reducer` 函数不用像上面这样手动调用，store.dispatch方法会触发 `Reducer` 的自动执行。为此，Store 需要知道 `Reducer` 函数，做法就是在生成 Store 的时候，将 `Reducer` 传入createStore方法。
```javascript
import { createStore } from 'redux';
const store = createStore(reducer);
```
具体参见见阮一峰的网络日志http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_one_basic_usages.html

## 中间件react-thunk与异步操作
上述的所有是同步的操作，若需要异步操作的话需要使用中间件
http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_two_async_operations.html