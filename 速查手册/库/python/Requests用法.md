

# Requests用法



## 概述

### 链接
* [Requests](https://github.com/psf/requests) 
* [中文文档](https://requests.readthedocs.io/en/latest/)

## 发送请求

* `requests.get(<url>)`  发送GET请求
* `requests.get(<url>, params=<obj>)` 带查询参数
* `requests.get(<url>, headers=<obj>)` 带自定义头
* `requests.post(<url>, data=<obj>)`  发送POST请求


### 示例
```py
payload = {'key1': 'value1', 'key2': 'value2'}
r = requests.get('https://httpbin.org/get', params=payload)
# 发送的URL: https://httpbin.org/get?key2=value2&key1=value1

# 多参数
payload = {'key1': 'value1', 'key2': ['value2', 'value3']}
r = requests.get('https://httpbin.org/get', params=payload)
# 发送的URL: https://httpbin.org/get?key1=value1&key2=value2&key2=value3

headers = {'user-agent': 'my-app/0.0.1'}
r = requests.get('https://httpbin.org/get', headers=headers)
```


## 获取响应

* `r.text`  获取文本数据
* `r.content` 获取二进制数据
* `r.json()` 获取JSON数据
* `r.status_code<int>` 获取HTTP状态码
* `r.header<dict>`  获取响应头，