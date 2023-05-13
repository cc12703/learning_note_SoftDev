
# Hurl用法


[TOC]



## 概述

### 功能
* 用于运行和测试HTTP请求
* 在文本文件中定义HTTP请求


### 文件结构
* 每个文件包含多个实体
* 每个实体包含一个强制的请求，一个非强制的响应

### 请求结构
* 方法、URL
* 头信息
* 其他参数：查询字符串、表单参数、cookie、鉴权
* 请求体


![](http://picbed.cc12703.com/20230511141755.png)


## 语法

### 请求
* `<key>:<value>`  设置头信息
* `[QueryStringParams]` 查询参数
    * `<key>:<value>`
* `[FormParams]` 表单参数
    * `<key>:<value>`
* `[Cookies]` 设置cookie值
    * `<key>:<value>`


#### 示例
```
GET https://example.org/news
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.14; rv:70.0) Gecko/20100101 Firefox/70.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive


PATCH https://example.org/file.txt
If-Match: "e0023aa4e"
```
