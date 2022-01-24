

# SASS用法

[TOC]


## 语法

### 嵌套
* 将一套样式嵌套进另一套样式中
* 内层样式将外层的选择器作为父选择器

```sass
#content {
  article {
    h1 { color: #333 }
    p { margin-bottom: 1.4em }
  }
  aside { background-color: #EEE }
}
```
编译为
```css
#content article h1 { color: #333 }
#content article p { margin-bottom: 1.4em }
#content aside { background-color: #EEE }
```

#### 群组选择器
```sass
.container {
  h1, h2, h3 {margin-bottom: .8em}
}

nav, aside {
  a {color: blue}
}
```
编译为
```css
.container h1, .container h2, .container h3 { margin-bottom: .8em }

nav a, aside a {color: blue}
```

#### 父选择器
* 使用`&`代表外层的父选择器

```sass
a {
  font-weight: bold;
  &:hover { text-decoration: underline; }
  body.firefox & { font-weight: normal; }
}
```
编译为
```css
a {
  font-weight: bold;
}
a:hover {
  text-decoration: underline; 
}
body.firefox a {
   font-weight: normal; 
}
```

#### 属性
* 指定一个命名空间

```sass
.funky {
  font: 20px/24px {
    family: fantasy;
    size: 30em;
    weight: bold;
  }
}
```
编译为
```css
.funky {
  font: 20px/24px;
  font-family: fantasy;
  font-size: 30em;
  font-weight: bold; 
}
```

### 注释
* `/* ... */` 多行注释，会被输出到编译后的css中
* `//` 单行注释，不会被输出到编译后的css中

```sass
/* This comment is
 * several lines long.
 * since it uses the CSS comment syntax,
 * it will appear in the CSS output. */
body { color: black; }

// These comments are only one line long each.
// They won't appear in the CSS output,
// since they use the single-line comment syntax.
a { color: green; }
```
编译后
```css
/* This comment is
 * several lines long.
 * since it uses the CSS comment syntax,
 * it will appear in the CSS output. */
body {
  color: black; 
}

a {
  color: green; 
}
```

### 脚本

#### 变量
* `$xxx: <value>` 定义变量
* `<atrr>: $xxx` 使用变量

```sass
$width: 5em;

#main {
  width: $width;
}

#sidebar {
  width: $width;
}
```
编译为
```css
#main {
  width: 5em;
}

#sidebar {
  width: 5em;
}
```

#### 数据类型
* 数字
* 字符串：`"xxx"`, `'xxx'`
* 颜色：`blue`, `#04a3f9`
* 布尔值：`true`, `false`
* 空值：`null`
* 数组（空格、逗号）：`1.5em 1em 0`, `Helvetica, Arial, sans-serif`
* 映射：`(key1: value1, key2: value2)`