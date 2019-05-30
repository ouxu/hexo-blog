---
title: HTML 编码规范
date: 2017-3-7 19:56:05
tags:
  - HTML
  - HTML5
  - 前端
categories: 学习笔记
comments: false
---


## 介绍

不洗碗工作室 HTML 编码规范

<!-- more -->

## 代码风格

### 缩进与换行

1. 使用 `4` 个空格作为一个缩进层级，不使用两个空格或 `tab` 字符（可在代码编辑器中设置）。
2. 每行不超过 `120` 个字符。

### 命名

1. `class`、`id` 命名字母全小写，单词以 `-` 分隔。
2. `class`、`id` 命名有意义，需代表相应模块部件的内容或功能，不以样式信息进行命名。

	```html
	<!-- good -->
	<div class="sidebar"></div>
	<!-- bad -->
	<div class="left"></div>
	```

3. 元素 `id` 保证页面唯一，在不确定情况下，换成 `class`。
4. 不允许 `class` 只用于让 `JavaScript` 选择某些元素，此时可使用 `id`。`class` 具有明确的语意和样式，否则易导致 `CSS class` 泛滥。
5. 同一页面，避免使用相同的 `name` 和 `id`。
6. 属性名小写

	```html
	<!-- good -->
	<table cellspacing="0">...</table>
	<!-- bad -->
	<table cellSpacing="0">...</table>
	```
7. 无需自闭和标签，不允许自闭和。常见的有 `input`、`br`、`img`、`hr` 等。

	```html
	<!-- good -->
	<input type="text" name="title">
	<!-- bad -->
	<input type="text" name="title" />
	```
8. `HTML5` 中规定允许省略的闭合标签，不允许省略，防止严苛环境中出现错误

	```html
	<!-- good -->
	<ul>
	    <li>first</li>
	    <li>second</li>
	</ul>

	<!-- bad -->
	<ul>
	    <li>first
	    <li>second
	</ul>
	```
9. 标签使用符合标签嵌套规则，如 `div` 不得置于 `p` 中，`tbody` 必须置于 `table` 中。详情上网查询。


### 书写

1. HTML 标签使用遵循标签的语义。下面是常见的标签语义：

 + p - 段落
 + h1,h2,h3,h4,h5,h6 - 层级标题
 + strong,em - 强调
 + ins - 插入
 + del - 删除
 + abbr - 缩写
 + code - 代码标识
 + cite - 引述来源作品的标题
 + q - 引用
 + blockquote - 一段或长篇引用
 + ul - 无序列表
 + ol - 有序列表
 + dl,dt,dd - 定义列表

2. 标签的使用尽量简洁，减少不必要的标签。

	```html
	<!-- good -->
	<img class="avatar" src="image.png">

	<!-- bad -->
	<span class="avatar">
	    <img src="image.png">
	</span>
	```
3. 属性值用双引号包围，不使用单引号。
4. 布尔类型的属性，建议不添加属性值

	```html
	<input type="text" disabled>
	<input type="checkbox" value="1" checked>
	```
5. 引入 `CSS ` 时必须指明 rel="stylesheet"
6. 建议引入 `CSS` 和 `JavaScript` 时不指明 `type` 属性。
7. 页面必须包含 `title` 标签声明标题。
8. `title` 必须作为 `head` 的直接子元素，并紧随 `charset` 声明之后。`title` 中如果包含 `ASCII` 之外的字符，浏览器需要知道字符编码类型才能进行解码，否则可能导致乱码。
9. 保证 `favicon` 可访问。
10. 禁止 `img` 的 `src` 取值为空。延迟加载的图片也要增加默认的 `src`。`src` 取值为空，会导致部分浏览器重新加载一次当前页面
11. 使用 button 元素时必须指明 `type` 属性值。

### 其他
1. `DOCTYPE` 使用 `HTML5` 的 `doctype` 来启用标准模式，建议使用大写的 `DOCTYPE`。
2. 在 `html`标签上设置正确的 `lang` 属性，有助于提高页面的可访问性。
3. 页面必须使用精简形式，明确指定字符编码。指定字符编码的 `meta` 必须是 `head` 的第一个直接子元素。

	```html
	<html>
	    <head>
	        <meta charset="UTF-8">
	        ......
	    </head>
	    <body>
	        ......
	    </body>
	</html>
	```
4. 在 `head` 中引入页面需要的所有 `CSS` 资源。
5. `JavaScript` 应当放在页面末尾，或采用异步加载。
6. 避免为 `img`添加不必要的 `title` 属性。
7. 建议有下载需求的图片采用 `img` 标签实现，无下载需求的图片采用 `CSS` 背景图实现。
