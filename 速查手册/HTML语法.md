

# HTML语法

[TOC]

## 概述

* HTML用于控制网页的结构
* 标签分为：根据元素的表现形式
	* 块元素：显示时占据一整行，排斥其他元素
	* 行内元素：显示时可以和其他元素共享一行
* 语义化：在需要的地方使用正确的标签


## 基本标签

* `<head> ... </head>`  网页头

* `<title> ... </title>` 网页标题
* `<meta> ... </meta>`   页面的特殊信息
	* `<meta name="keywords" content="xxxx" />`  页面关键字
	* `<meta name="description" content="xxxx" />` 页面描述
	* `<meta charset="utf-8" />`   说明页面使用的编码是utf-8
* `<style> ... </style>`  定义CSS样式
* `<script> ... </script>`  定义JS代码
* `<link type="text/css" rel="stylesheet" href="xxxx" />`  引入外部样式文件

* `<body> ... </body>`  定义网页内容
* `<!-- xxxx -->`       定义注释
* `<hN> ... </hN>`      [块元素]定义标签(N：1--6)
* `<p> ... </p>`        [块元素]定义段落
	* 段落内会自动换行
	* 段落前后会自动加空行
* `<br/>`               定义换行
* `<hr/>`               定义水平线
* `<div> ... </div>`    [行内元素]定义分区，结合CSS进行样式控制


## 文本标签

* `<b> ... </b>`  [行内元素]粗体文本
* `<i> ... </i>`  [行内元素]斜体文本
* `<em> ... </em>`  [行内元素]斜体文本
* `<sup> ... </sup>`  上标文本
* `<sub> ... </sub>`  下标文本
* `<s> ... </s>`      中划线
* `<u> ... </u>`      下划线

### 示例
```html
(a +)b <sup>2</sup> = a <sup>2</sup> + b <sup>2</sup> + 2ab
```

## 高级标签

### 有序列表
```html
<ol> 
	<li> ... </li> 
	<li> ... </li> 
</ol>
```

### 有序列表
```html
<ul> 
	<li> ... </li> 
	<li> ... </li> 
</ul>
```

### 定义列表
```html
<dl> 
	<dt>名词</dt>
	<dd>描述</dd> 
	
	<dt>名词</dt>
	<dd>描述</dd> 
</dl>
```

### 表格
```html
<table> 
	<caption> 标题 </caption>
	<thead>   <!-- 表头 -->
		<tr>
			<th> ... </th>  <!-- 表头单元格 -->
			<th> ... </th> 
		</tr>
	</thead>
	<tbody> <!-- 表身 -->
		<tr> <!-- 表格行 -->
			<td> ... </td>  <!-- 单元格 -->
			<td> ... </td> 
		</tr> 
		<tr> <!-- 表格行 -->
			<td> ... </td> 
			<td> ... </td> 
		</tr> 
	</tbody>
	<tfoot>  <!-- 表脚，用于统计数据 -->
		<tr>
			<td> ... </td> 
			<td> ... </td> 
		</tr> 
	</tfoot>
</table>
```

### 图片标签

* `<img src="xxxx" />`  设置图片路径：绝对路径，相对路径
* `<img alt="xxxx" />`  设置图片描述信息，用于展示给搜索引擎
* `<img title="xxxx" />` 设置图片描述信息，用于展示给用户

### 超链接标签

* `<a href="xxxx"> ... </a>`  设置要跳转的页面路径
* `<a target="xxxxx"> ... </a>` 设置打开窗口的方式
	* `_self`: 在原窗口打开链接（默认方式）
	* `_blank`: 在新窗口打开链接

#### 类型
* 外部链接：指向”外部网站页面“
* 内部链接：指向"自身网站页面"
* 锚点链接：内部链接的一种，指向”当前页面的某个部分“



## 表单

### 表单标签
* `<form name="xxxx" />`   设置表单名称
* `<form method="xxx" />`  设置提交方式
	* 方式：get, post
* `<form action="xxxx" />` 设置提交地址
* `<form target="xxxx" />` 设置打开方式
	* `_blank`: 在新窗口打开链接
* `<form enctype="xxxx" />` 设置提交数据的编码方式

### 输入标签
* `<input type="text" value="xxx" size="xxx" maxlength="xxx"/>`  单行文本框
	* value: 设置默认值
	* size:  设置文本框长度
	* maxlength: 设置输入字符的最大个数

