---
title: 简单的 url 参数拼接
date: 2017-04-16 21:43:19
tags:
  - JavaScript
  - ES6
  - Utils
categories: 工具封装
comments: false
---

> http://api.xxxxx.xx/problems/search?keyword=t&page=1&size=2
> 场景：在请求的 API 中附带参数，以`?`、`&`连接
> 推荐使用社区 npm 包：[ qs ](https://www.npmjs.com/package/qs) 或 [ query-string ](https://www.npmjs.com/package/query-string)实现该功能 【2017年11月19日更新】

<!-- more -->

### 参数拼接

```javascript
const parseParams = (uri, params) => {
  const paramsArray = []
  Object.keys(params).forEach(key => params[key] && paramsArray.push(`${key}=${params[key]}`))
  if (uri.search(/\?/) === -1) {
    uri += `?${paramsArray.join('&')}`
  } else {
    uri += `&${paramsArray.join('&')}`
  }
  return uri
}
```

```javascript
let params = {
  keyword: 't',
  page: 1,
  size: 2
}

let uri = 'http://api.xxxxx.xx/problems/search'

parseParams(uri, params)
//http://api.xxxxx.xx/problems/search?keyword=t&page=1&size=2

```


### FormData

```javascript
urlEncode (params) {
  let paramsArray = []
  Object.keys(params).forEach(key => params[key] && paramsArray.push(`${key}=${params[key]}`))
  return paramsArray.join('&')
}
```

```javascript
let params = {
  name: 'cs1'
  email: 'cs1@cs1.com'
  mobile: '1xxxxxxxxxx'
  password: '123456'
  password_confirmation: '123456'
  captcha: 'xxhej'
}

parseParams(params)
//name=cs1&email=cs1@cs1.com&mobile=15603315002&password=123456&school=neuq&password_confirmation=123456&captcha=xxhej
```

### 分析

+ 创建空数组

	```javascript
	  const paramsArray = []
	```
+  遍历对象的 key 属性，只要对应的 value 存在，就将其以 `key=value` 的形式存入数组

	```javascript
	  Object.keys(params).forEach(key => params[key] && paramsArray.push(`${key}=${params[key]}`))

	```

+  利用`Array.join()`方法拼接数组元素为字符串，如果之前无`?`，在`uri`中填加
  ```javascript
    if (uri.search(/\?/) === -1) {
      uri += `?${paramsArray.join('&')}`
    } else {
      uri += `&${paramsArray.join('&')}`
    }
    return uri
  ```
