---
title: React 组件的创建
date: 2017-3-25 14:39:05
tags:
  - JavaScript
  - React
  - ES6
categories: 学习笔记
comments: false
---


> 组件（Component）是 React 最强大的功能之一。React 中都是关于模块化、可组装的组件，组件可以封装可复用的代码，使开发更高效。这里总结 React 组件的三种写法，以及相应的使用场景。

<!-- more -->

### 无状态函数式组件（Stateless Functional Component）
```javascript
function Hello(props) {
    return (
        <div>Hello, {props.name}</div>
    );
}

// Arrow Function
const Hello = (props) => {
    const {name} = props; // 属性较多时可以先取出，不用每个属性都加上 props
    return (
       <div>Hello, {name}!</div>
    );
};

// use
<Hello
    name='World'
    // ...content
/>
```
正如其名字所表达的一样，这一类组件没有自身的状态，只根据传入的 `props` 进行相应的渲染。这种写法是官方极为推崇的，如果一个组件不涉及到 `state` 状态的操作，且用不到生命周期函数，建议把它写成无状态函数式组件。

这一类组件十分简洁，相同的输入一定对应相同的输出，且不会产生任何副作用。在大型项目中，无状态组件会搭配高阶组件使用，高阶组件托管 `state`，无状态函数组件则负责具体的页面渲染。

但是无状态组件不支持 `ref` ，而 `React` 自身就提醒要避免使用 `ref` ，所以无状态组件在大多是情况下都是很好的选择。


### ES5 写法（React.createClass）

```javascript
var Hello = React.createClass({
    getDefaultProp:function() {
    return {
        name: 'World'
    }
},
    render: function () {
        return (
            <div>Hello, {this.state.name}!</div>
        );
    }
});
```
这是 `React` 最开始推荐的组件写法，是用 `ES5` 的原生 `JavaScript` 来实现的 `React` 组件。这类组件接受一个对象为参数，对象中的 `render` 方法返回一个组件实例。但是随着 `React` 的发展，这种写法已经渐渐不适用了。

### ES6 写法（React.Component）
```javascript
import React, {Component} from 'react';

class Hello extends Component {
    constructor(props) {
        super(props);
        this.state = {
            name: 'World'
        };
        this.onClick=this.onClick.bind(this)
    }
    onClick() {
        // do something
    }
    render() {
        return (
            <div onClick={this.onClick}>Hello, {name}!</div>
        );
    }
}

export default Hello;
```
这是目前官方推荐的有状态组件的写法，使用 `ES6` 的语法来创建组件。使用这种方式创建组件时，`React`并没有对内部的函数进行 `this` 绑定，如果想要在函数回调时保持正确的 `this`，就要手动对函数进行 `this` 绑定。如例子中的 `onClick`，在构造函数中绑定了 `this`。

`React` 团队已经声明 `React.createClass` 最终会被 `React.Component` 的 类形式所取代，所以在实际的开发中，能用 `React.Component` 创建的组件的就尽量不用 `React.createClass` 形式创建组件。

---
推广:

+ [没那么复杂的 react-redux 脚手架](https://github.com/ouxu/react-redux-simple-boilerplate)
+ [基于 React 开发的 Online Judge 系统](https://github.com/ouxu/NEUQ-OJ)
