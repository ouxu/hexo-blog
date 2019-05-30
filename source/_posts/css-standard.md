---
title: CSS 编码规范
date: 2017-3-7 20:56:05
tags:
  - HTML
  - HTML5
  - CSS
  - 前端
categories: 学习笔记
comments: false
---

## 1 前言
不洗碗工作室 `CSS` 编码规范。

<!-- more -->

## 2 代码风格
* 缩进：使用 `4` 个空格作为缩进。
* 空格：
	* 选择器与 `{` 之间必须有空格。

		```css
		.selector {
			...
		}
		```
	* `属性名` 与 `:` 之间不能有空格，`:` 与 `属性值` 之间必须包含空格。

		```css
		/* good */
		color: #999;
		/* bad */
		color :#999;
		```
	* `列表型属性值` 书写在单行时，`,` 后必须跟一个空格。

		```css
		font-family: Arial, sans-serif;
		```
* 行长度：每行不超过 `120` 个字符，除非单行无法分割（URL）。
* 选择器：
	* 当一个 `rule` 包含多个 `selector` 时，每个选择器声明独占一行。


		```css
		/* good */
		.post,
		.page,
		.comment {
		    line-height: 1.5;
		}
		/* bad */
		.post, .page, .comment {
		    line-height: 1.5;
		}
		```
	* `>`、`+`、`~` 选择器的两边各保留一个空格。

		```css
		/* good */
		main > nav {
		    padding: 10px;
		}

		label + input {
		    margin-left: 5px;
		}

		input:checked ~ button {
		    background-color: #69C;
		}

		/* bad */
		main>nav {
		    padding: 10px;
		}

		label+input {
		    margin-left: 5px;
		}

		input:checked~button {
		    background-color: #69C;
		}
		```
	* 属性选择器中的值必须用双引号包围。
* 属性：
	* 属性定义必须另起一行。
	* 属性定义后必须以分号结尾。

## 3 其他
* 如无必要，不得为 `id`、`class` 选择器添加类型选择器进行限定。

	```css
	/* good */
	#error,
	.danger-message {
	    font-color: #c00;
	}

	/* bad */
	dialog#error,
	p.danger-message {
	    font-color: #c00;
	}
	```

*  选择器的嵌套层级应不大于 `3` 级，限定条件应尽可能精确。
*  在可以使用缩写的情况下，尽量使用属性缩写。

	```css
	/* good */
	.selector {
		margin: 10px 0 15px 5px;
	}

	/* bad */
	.selector {
		margin-top: 10px;
		margin-bottom: 15px;
		margin-right: 0;
		margin-left: 5px;
	}
	```

* 使用 `border` / `margin` / `padding` 等缩写时，应注意隐含值对实际数值的影响，确实需要设置多个方向的值时才使用缩写。

	```css
	/* centering <article class="selector"> horizontally and highlight featured ones */
	article {
	    margin: 5px;
	    border: 1px solid #999;
	}

	/* good */
	.selector {
	    margin-right: auto;
	    margin-left: auto;
	    border-color: #69c;
	}

	/* bad */
	.selector {
	    margin: 5px auto;
	    border: 1px solid #69c;
	}
	```
* 属性书写顺序：同一 `rule set `下的属性在书写时，应按功能进行分组，并以 `Formatting Model`（布局方式、位置） > `Box Model`（尺寸） > `Typographic`（文本相关） > `Visual`（视觉效果） 的顺序书写，以提高代码的可读性。
	* Formatting Model 相关属性包括：position / top / right / bottom / left / float / display / overflow 等
	* Box Model 相关属性包括：border / margin / padding / width / height 等
	* Typographic 相关属性包括：font / line-height / text-align / word-wrap 等
	* Visual 相关属性包括：background / color / transition / list-style 等

* 清除浮动：当元素需要撑起高度以包含内部的浮动元素时，通过对伪类设置 `clear` 或触发 `BFC` 的方式进行 `clearfix`。尽量不使用增加空标签的方式。
* 尽量不使用 `!important` 声明。当需要强制指定样式且不允许任何场景覆盖时，通过标签内联和 `!important`定义样式。
* 将 `z-index` 进行分层，对文档流外绝对定位元素的视觉层级关系进行管理。
	* 在可控环境下，期望显示在最上层的元素，z-index 指定为 999999。
	* 在第三方环境下，期望显示在最上层的元素，通过标签内联和 !important，将 z-index 指定为 2147483647。

## 4 值与单位
* 文本内容必须用双引号包围。

	```css
	/* good */
	html[lang|="zh"] q:before {
	    font-family: "Microsoft YaHei", sans-serif;
	    content: "“";
	}

	html[lang|="zh"] q:after {
	    font-family: "Microsoft YaHei", sans-serif;
	    content: "”";
	}

	/* bad */
	html[lang|=zh] q:before {
	    font-family: 'Microsoft YaHei', sans-serif;
	    content: '“';
	}

	html[lang|=zh] q:after {
	    font-family: "Microsoft YaHei", sans-serif;
	    content: "”";
	}
	```
* 当数值为 `0` - `1` 之间的小数时，省略整数部分的 `0`。

	```css
	/* good */
	panel {
	    opacity: .8;
	}

	/* bad */
	panel {
	    opacity: 0.8;
	}
	```
* `url()` 函数中的路径不加引号。
* 长度为 `0` 时须省略单位。
* `RGB`颜色值必须使用十六进制记号形式 `#rrggbb`。不允许使用 `rgb()`，带有alpha的颜色信息可以使用 rgba()。使用 rgba() 时每个逗号后必须保留一个空格。
* 颜色值可以缩写时，必须使用缩写形式。
* 颜色值不允许使用命名色值。
* 颜色值中的英文字符采用小写。如不用小写也需要保证同一项目内保持大小写一致。
* `2D`位置必须同时给出水平和垂直方向的位置。2D 位置初始值为 `0% 0%`，但在只有一个方向的值时，另一个方向的值会被解析为 center。为避免理解上的困扰，应同时给出两个方向的值。[background-position属性值的定义](http://www.w3.org/TR/CSS21/colors.html#propdef-background-position)

## 5 文本编排
* font-family：
	* 按「西文字体在前、中文字体在后」、「效果佳 (质量高/更能满足需求) 的字体在前、效果一般的字体在后」的顺序编写，最后必须指定一个通用字体族( `serif` / `sans-serif` )。
	* `font-family` 不区分大小写，但在同一个项目中，同样的 `Family Name` 大小写必须统一。
* 需要在 `Windows` 平台显示的中文内容，其字号应不小于 `12px`。
* 需要在 `Windows` 平台显示的中文内容，不要使用除 `normal` 外的 `font-style`。其他平台也应慎用。
* `font-weight` 属性必须使用数值方式描述。
* `line-height` 在定义文本段落时，应使用数值。

## 6 变换与动画
* 使用 `transition` 时应指定 `transition-property`。

	```css
	/* good */
	.box {
	    transition: color 1s, border-color 1s;
	}

	/* bad */
	.box {
	    transition: all 1s;
	}
	```
*  尽可能在浏览器能高效实现的属性上添加过渡和动画。

## 7 响应式
* `Media Query` 不得单独编排，必须与相关的规则一起定义。

	```css
	/* Good */
	/* header styles */
	@media (...) {
	    /* header styles */
	}

	/* main styles */
	@media (...) {
	    /* main styles */
	}

	/* footer styles */
	@media (...) {
	    /* footer styles */
	}


	/* Bad */
	/* header styles */
	/* main styles */
	/* footer styles */

	@media (...) {
	    /* header styles */
	    /* main styles */
	    /* footer styles */
	}
	```
* `Media Query` 如果有多个逗号分隔的条件时，应将每个条件放在单独一行中。

## 8 兼容性
* 属性前缀：带私有前缀的属性由长到短排列，按冒号位置对齐。

	```css
	.box {
	    -webkit-box-sizing: border-box;
	       -moz-box-sizing: border-box;
	            box-sizing: border-box;
	}
	```
* Hack：
	* 需要添加 `hack` 时应尽可能考虑是否可以采用其他方式解决。
	* 尽量使用 `选择器 hack` 处理兼容性，而非 `属性 hack`。
	* 尽量使用简单的 属性 `hack`。

* Expression：禁止使用 `Expression`。
