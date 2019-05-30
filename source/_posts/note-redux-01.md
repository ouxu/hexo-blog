---
title: Redux 入门（一）
date: 2016-12-14 14:39:05
tags:
  - JavaScript
  - React
  - Redux
categories: 学习笔记
comments: false
---

## 基本用法

> & 读 [Redux 入门教程（一）：基本用法!](http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_one_basic_usages.html)

<!-- more -->

---
### 一、设计思想
+ Web 应用是一个状态机，视图与状态是一一对应的。
+ 所有的状态，保存在一个对象里面。

### 二、基本概念
#### 2.1 Store

Store 容器，保存数据的地方，整个应用都只能有一个 Store 。

Redux 提供 createStore 函数，用来生成 Store 。

```javascript
import { createStore } from 'redux';
const store = createStore(fn);
```

函数接受另一个 ***函数*** 作为参数，返回新的 Store 对象。

#### 2.2 State

State 数据集合，在 Store 对象中包含所有数据，其某个时间点的数据集合，就叫做 State 。

某个时刻的 State ，可以通过 store.getState() 得到。

```javascript
import { createStore } from 'redux';
const store = createStore(fn);

const state = store.getState();
```

Redux 规定，一个 State 对应一个 View ，相同的 State 对应相同的 View ，两者之间知道一个便能知道另外一个是什么样的。

#### 2.3 Action

Action 行为通知，用户无法接触到 State ，只能接触到 View ， State 的变化必须是 View 的变化导致的， Action 是 View 出发的行为通知，表示 State 应该要发生变化了。 Ation 描述当前发生的事情。而改变 State 的唯一方法，就是使用 Action ，它会把数据传输到 Store 。

Action 是一个对象。其必须具有一个 type 属性，表示 Action 的名称。其他属性可以自由设置，但是大家有一个通俗的规范可以用作参考。

```javascript
const action = {
  type: 'ADD_TODO',
  payload: 'Learn Redux'
};
```

上面代码中，Action 的名称是 ADD_TODO ，而它携带的信息是字符串 Learn Redux 。

#### 2.4 Action Creator

Action Creator ，顾名思义，Action 生成函数。因为 View 要发送多少消息，就要对应有多少种 Action 。如果都手写，会很麻烦，所以有了 Action Creator 。

```javascript
const ADD_TODO = ‘添加TODO’;

addTodo=(text)=>{
  type: ADD_TODO,
  payload: text
}

const action = addTodo('Learn Redux');
```

上面代码中， addTodo 函数就是一个 Action Creator 。


#### 2.5 store.dispatch()

store.dispatch()  方法，是 View  发出 Action
的唯一方法。

```javascript
import { createStore } from 'redux';
const store = createStore(fn);

store.dispatch({
  type: 'ADD_TODO',
  payload: 'Learn Redux'
});
```

上面代码中， store.dispatch 接收一个 Action 对象作为参数，将它发送出去。

结合 Action Creator ，这段代码可以改写如下。

```javascript
store.dispatch(addTodo('Learn Redux'));
```

#### 2.6 Reducer

Reducer 计算过程，Store 收到 Action 以后，必须给出一个新的 State ，这样 View  才会发生变化，其中的计算过程便称为 Reducer 。

Reducer 是一个函数，它接受 Action 和当前 State 作为参数，返回一个新的 State。

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

上面代码中，reducer 函数收到名为 ADD的 Action 以后，就返回一个新的 State，作为加法的计算结果。其他运算的逻辑（比如减法），也可以根据 Action 的不同来实现。

实际应用中，Reducer 函数不用像上面这样手动调用，store.dispatch方法会触发 Reducer 的自动执行。为此，Store 需要知道 Reducer 函数，做法就是在生成 Store 的时候，将 Reducer 传入 createStore 方法。
