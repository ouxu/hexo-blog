---
title: React Router 2 开发经验与工具分享
date: 2017-06-23 20:32:51
tags:
  - JavaScript
  - ES6
  - Utils
  - React
categories: 工具封装
comments: false
---

> 本工具包括以下几个功能实现，`React Router` 版本为 2.0

1. 根据开发环境决定使用 `hashHistory` 还是 `browserHistory`
2. js 代码中实现路由跳转
3. 实现改变 queryString 的方法函数

<!-- more -->

以上功能实现起来很简单，主要是应用了` process.env.NODE_ENV` 这个环境变量，用以判断现在代码的运行环境，有兴趣的可以深入去了解一下 `process.env` 这个环境变量。


### 根据开发环境决定 `history` 类型

```javascript
import { browserHistory, hashHistory, IndexRoute, Route, Router } from 'react-router'

const history = process.env.NODE_ENV === 'development' ? hashHistory : browserHistory

const RouterApp = store => (
  <Router history={history}>
  		···
  </Router>
 )
```

其实实现起来非常简单，无非是根据当前的运行环境，将 history 改变。

### js 代码中实现路由跳转

```javascript
import { browserHistory, hashHistory } from 'react-router'

export default (path) => {
  if (path) {
    process.env.NODE_ENV === 'development'
      ? hashHistory.push(path)
      : browserHistory.push(path)
  } else {
    throw new Error('Path cannot be null!')
  }
}

```
同样很简单，在这就不分析代码了。需要注意的一点是，这个 `path` 参数需要写全，举个例子吧：

`hashHistory.push('homepage')`这样的写法需要写成`hashHistory.push('/homepage')`。

请不要为了省事而不写这个`/`，不然会有意想不到的 `bug`，同理在 `Link` 标签中的 `to` 属性也应该写全。

### 实现改变 queryString 的方法函数

> 场景：作为常见的表格页，分页是少不了的，而保存当前页码，搜索字段的最好方法就是将这些字段存入 `queryString` 中了。

```javascript
import { urlEncode } from 'utils'
export default (router, params, clear = false) => {
  let {location: {pathname, query}} = router
  params = clear ? params : {
    ...query,
    ...params
  }
  let queryString = urlEncode(params)
  if (process.env.NODE_ENV === 'development') {
    pathname = '/#' + pathname
  }
  window.location.replace(`${pathname}?${queryString}`)
}
```

看看这个函数吧，默认参数有三个，第一个 `router` 为 `react-router` 的 `router` 属性，里面包括了我们要用到的 `pathname`、`query`参数。至于为何不直接取 `window.location`，这是考虑到开发环境的不同，所引起的 `bug`，当前 `history` 为 `hashHistory`时，`window.location` 中的`pathname`、`query`参数与我们所期待的不一样。

第二个参数为传入的 `query` 对象，这个就是我们需要修改的 `queryString`；第三个参数决定是否清除之前的 `queryString`。

函数中调用的 [urlEncode](http://www.outxu.cn/js-util-parseparams/) 作用是将 `params` 参数拼接，点击可以跳转查看 [urlEncode](http://www.outxu.cn/js-util-parseparams/) 实现。

最后根据当前的环境进行路由替换。
