

# CSS用法

[TOC]


## 总体
* CSS：层叠样式表（Cascading Style Sheets）
	* 样式定义了如何显示HTML元素
	* 多个样式可层叠合并为一个
* 层叠优先级
	1. 内联样式（用于单个元素）
	1. 内部样式表（用于单个文档）
	1. 外部样式表（用于多个页面应用共享样式）
	1. 浏览器缺省设置


### 示例
```css
/* 内联样式 */
<p style="color:sienna;margin-left:20px">这是一个段落。</p>

/* 内部样式表 */
<head>
    <style>
        hr {color:sienna;}
        p {margin-left:20px;}
        body {background-image:url("images/back40.gif");}
    </style>
</head>

/* 外部样式表 */
<head> 
    <link rel="stylesheet" type="text/css" href="mystyle.css"> 
</head>
```

## 语法

### 规则
* 格式：`选择器 { 属性:值; 属性:值; }`
	* 选择器用于定义HTML元素
* `/* */` 为注释


### 选择器
* `name { ... }` 根据元素名称来选择
* `#name { ... }` 根据元素的ID属性值来选
* `.name { ... }` 根据元素的class属性值来选
* `* { ... }`     选择所有元素
* `name,name { ... }` 分组选择，选择组内的所有元素

* `nameE nameF { ... }` 后代选择器，选择`nameE`元素内的所有`nameF`元素
* `nameE > nameF { ... }` 子元素选择器，选择`nameE`元素的所有`nameF`的直接(一级)子元素
* `nameE + nameF { ... }` 相邻兄弟选择器，选择紧连着`nameE`元素的第一个`nameF`元素
* `nameE ~ nameF { ... }` 后续兄弟选择器，选择`nameE`元素之后的所有`nameF`的相邻兄弟元素

* `selName[attr] { ... }` 选择所有带指定属性的元素
* `selName[attr="value"] { ... }` 选择所有带指定属性和值的元素


#### 示例
```css
p {text-align:center;}
#para1 {text-align:center;}
.center {text-align:center;}
* {text-align:center;}


a[target] { background-color: yellow; }
a[target="_blank"] { background-color: yellow; }
```

### 伪选择器
格式：`elmName:pName { ... }`
* `link`  未访问的元素
* `visited`  已访问的元素 
* `hover`  鼠标悬停其上的元素
* `first-child` 第一个子元素
* `empty` 没有子元素
* `root`  根元素
* `before` 元素的前面
* `after`  元素的后面


#### 示例
```css
a:link { background-color:yellow; }
```


### 值

#### 绝对单位
* `px` 像素单位
* `#RRGGBB`  颜色值
* `rgb(red,green,blue)` 颜色值
* `rgba(red,green,blue,alpha)` 颜色值

#### 相对单位
* `%` 百分比单位
* `em` 字体尺寸单位，相对于当前元素
* `rem` 字体尺寸单位，相对于根元素
* `vh` 视口单位，视口高度的1/100
* `vw` 视口单位，视口宽度的1/100
* `vmin` 视口单位，视口宽、高中较小一方的1/100
* `vmax` 视口单位，视口宽、高中较大一方的1/100

##### 提示
* `rem`设置字号，`px`设置边框，`em`设置大部分属性

##### 示例
```css
body {
  font-size: 16px;
}

.slogan {
  font-size: 1.2em;  /* 计算值为 19.2px (16px * 1.2) */
  padding: 1.2em;   /* 计算值为 23.04px (19.2px * 1.2) */
}
```

### 其他

#### `!important`
* 作用：用于增加样式的权重，使该声明覆盖其他任何声明
* **要尽量避免使用该规则**
* 可以用来给网站设定一个全站样式的CSS




## 基本属性

### 显示
使用display属性来确定一个元素如何显示，visibility属性来确定一个元素是否可见

* `display:block` 将元素作为块元素显示
* `display:inline` 将元素作为内联元素显示
* `display:none` 不显示元素，不会占用任何空间
* `visibility:hidden` 隐藏元素，会占用空间

### 定位
使用position属性来确定一个元素在网页上的位置

* `position:static` 默认行为，遵守**正常页面流**
	* **正常页面流**：浏览器按照源码顺序，决定每个元素的位置
* `position:relative` 相对于默认位置(static时的位置)进行偏移
	* 需要使用`top`, `bottom`, `left`, `right`来确定偏移方向和距离
* `position:absolute` 相对于父元素进行偏移
	* 需要使用`top`, `bottom`, `left`, `right`来确定偏移方向和距离
	* 该元素会被“正常页面流”忽略，会和其他元素重叠
* `position:fixed` 相对于浏览器窗口进行偏移
	* 需要使用`top`, `bottom`, `left`, `right`来确定偏移方向和距离
	* 导致元素位置不随页面滚动而变化
* `position:sticky` 根据用户的滚动情况，在relative模式和fixed模式之间进行切换
	* 需要使用`top`, `bottom`, `left`, `right`进行配合


### 溢出
* `overflow:visible` 溢出容器的内容也可见。默认值
* `overflow:hidden`  溢出容器的内容不可见，被裁剪
* `overflow:scroll`  显示滚动条，通过滚动条查看溢出内容
* `overflow:auto`    只有溢出时才会出现滚动条

## 元素属性


### 背景
* `background-color`  背景颜色   
* `background-image` 背景图片 
* `background-size`  背景大小
  * `contain`  不缩放图片来填充容器
  * `cover`    通过缩放图片来填充容器
  * `auto auto`  在指定方向上缩放图片
* `background-position` 背景的起始位置
* `background-repeat` 背景图片如何重复
  * `repeat`   重复放置
  * `repeat-x` 在X轴上重复放置
  * `no-repeat`  不重复放置
* `background-attachment` 背景是否随其余部分滚动  
* `background-filter`  背景滤镜
  * `grayscale(percent)` 灰度
  * `blur(value)`        模糊
  * `opacity(percent)`   透明度

#### 示例
```css
body 
{ 
    background-image:url('img_tree.png'); 
    background-repeat:no-repeat; 
    background-position:right top; 
}

body        
{        
   background-image:url('img_tree.png');        
   background-repeat:no-repeat;        
   background-position:66% 33%;        
}
```


### 文本
* `color` 文本颜色
* `line-height` 行高（行间的距离）
* `text-align` 对齐方式 （水平对齐方式）
* `text-decoration` 修饰
* `text-transform`  转换 （大写，小写）

* `font-family` 字体系列
* `font-style` 字体样式  （正常、斜体、倾斜）
* `font-size` 字体大小  （绝对大小、相对大小）


### 盒属性

* `box-sizing:border-box`   大小模式为border，常用该模式
  * `height/width = content + padding + border`
* `box-sizing:content-box`  大小模式为content
  * `height/width = content`

* `margin-<oriention>:auto` 浏览器自动计算
* `margin-<oriention>:xxxpx` 固定大小
* `margin-<oriention>:xxx%`  百分比
* `margin-<oriention>:inherit` 继承父元素
* `margin: <top> <right> <bottom> <left>` 同时设置四个方向

* `padding-<oriention>:xxxpx` 固定大小
* `padding-<oriention>:xxx%`  百分比
* `padding-<oriention>:inherit` 继承父元素
* `padding: <top> <right> <bottom> <left>` 同时设置四个方向 

* `border-style: <type>`  边框类型
* `border-style: <top-type> <right-type> <bottom-type> <left-type>`  边框类型
* `border-width: xxpx`    边框宽度，指定大小
* `border-width: thin|medium|thick` 边框宽度，预置大小
* `border-color: <color>` 边框颜色
* `border-color: <top-color> <right-color> <bottom-color> <left-color>`  边框颜色

\<oriention> 包含 top, bottom, left, right


#### 值类型

* `auto` 默认值，浏览器会自动计算
* `xxx`  固定大小
* `inherit` 继承父元素中对应属性的值
* `xxx%` 百分比，相对于`包含块`的值（不一定是父元素）


#### 盒模型

##### 图示
![](http://picbed.cc12703.com/20201109101036.png)

##### 说明
* Margin（外边距）   清除边框区域。没有背景颜色，它是完全透明
* Border（边框）     边框周围的填充和内容。会受到盒子的背景颜色影响
* Padding（内边距）  清除内容周围的区域。会受到框中填充的背景颜色影响
* Content（内容）    盒子的内容，显示文本和图像





## Flex布局


### 概述
* 一种弹性布局，使用`display`来启用
* 概念：容器、项目、主轴、交叉轴

#### 示例
```css
.box{
  display: flex;  /* 用于容器 */
}

.box{
  display: inline-flex;  /* 用于行内元素 */
}
```

#### 图示
![](http://picbed.cc12703.com/20201109101158.png)



### 容器属性

* `flex-direction:row` 主轴为水平，起点在左端
* `flex-direction:row-reverse` 主轴为水平，起点在右端
* `flex-direction:column` 主轴为垂直，起点在上端
* `flex-direction:column-reverse` 主轴为垂直，起点在下端

* `flex-wrap:nowrap` 内容排不下了不换行
* `flex-wrap:wrap` 内容排不下了进行换行，新行在下方
* `flex-wrap:wrap-reverse` 内容排不下了进行换行，新行在上方

* `justify-content:flex-start` 项目在主轴上左对齐
* `justify-content:flex-end` 项目在主轴上右对齐
* `justify-content:center` 项目在主轴上居中
* `justify-content:space-between` 项目在主轴上两端对齐，项目之间间隔相等
* `justify-content:space-around` 项目在主轴上两侧的间隔相等

* `align-items:flex-start` 项目在交叉轴上起点对齐
* `align-items:flex-end` 项目在交叉轴上终点对齐
* `align-items:center` 项目在交叉轴上中心对齐
* `align-items:stretch` 项目在交叉轴上占满整个容器的高度
* `align-items:baseline` 项目的第一行文字的基线对齐


#### 示例
```css
.box {
  flex-direction: row | row-reverse | column | column-reverse;
}
```
![](http://picbed.cc12703.com/20201109101300.png)


```css
.box {
  justify-content: flex-start | flex-end | center | space-between | space-around;
}
```
![](http://picbed.cc12703.com/20201109101420.png)


```css
.box {
  align-items: flex-start | flex-end | center | baseline | stretch;
}
```
![](http://picbed.cc12703.com/20201109101456.png)



### 项目属性

* `order: <int:0>` 排列顺序，值越小越靠前
* `flex-grow: <int:0>` 放大比例，0为不放大
* `flex-shrink: <int:1>` 缩小比例，1为缩小
* `flex-basis: <length>` 占据固定空间
* `flex-basis: auto` 占据项目本身的大小
* `align-self: <value>`  项目自己的对齐方式，值同`align-items` 



## Grid布局

### 概述
* 一种二维布局，使用`display`来启用
* 布局方法：将网页划分为一个个网格，通过任意组合不同的网格来布局
* 概念
	* 布局区域为**容器**，区域内的顶层子元素为**项目**
	* 水平区域为**行**，垂直区域为**列**，交叉区域为**单元格**

#### 示例
```css
.box{
  display: grid;  /* 用于容器 */
}

.box{
  display: inline-grid;  /* 用于行内元素 */
}
```

### 容器属性

* `grid-template-columns: <val> <val> <val>` 定义列宽
* `grid-template-rows: <val> <val> <val>` 定义行高


#### 示例
```css
.container {
  grid-template-columns: 33.33% 33.33% 33.33%;
  grid-template-rows: 33.33% 33.33% 33.33%;
}

.container {
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
}

/* 简化写法 */
.container {
  grid-template-columns: repeat(3, 33.33%);
  grid-template-rows: repeat(3, 33.33%);
}
```
![](http://picbed.cc12703.com/20210907151902.png)



## 媒体查询

### 语法
```
@media 媒体类型 and|not|only (媒体特征) {
    CSS代码;
}
```

#### 关键字
* and 用于包含特定的媒体类型
* not 用于排除特定的媒体类型

#### 媒体类型
* all  所有设备
* screen 电脑屏幕
* print 打印机、打印预览

#### 媒体特征
* width 浏览器页面的可视宽度
  * min-width 页面最小的可视宽度
  * max-width 页面最大的可视宽度
* height 浏览器页面的可视高度
  * min-height 页面最小的可视高度
  * max-height 页面最大的可视高度
* device-width 设备屏幕的可见宽度
* device-height 设备屏幕的可见高度
* aspect-ratio 页面可见区域的宽/高比率
* device-aspect-ratio 设备屏幕的宽/高比率
* orientation 设备屏幕的方向

### meta标签

#### 示例
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
```

#### 参数
* width=device-width 可视宽度等于设备宽度
* initial-scale 初始的缩放比例
* minimum-scale 允许用户缩放到的最小比例
* maximum-scale 允许用户缩放到的最大比例
* user-scalable 用户是否可以手动缩放

