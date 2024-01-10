



# JsonPath语法


## 基本规则
* `$` 文档根元素
* `@` 当前元素
* `.` 匹配下级元素
* `..` 匹配所有子元素
* `[<name>]` 匹配下级元素
* `*` 通配符，匹配下级元素
* `[<index>]` 匹配数组中的元素，索引从0开始


### 例子
* `$.store.book[*].author`  获取所有book的author节点
* `$..author`  获取所有author节点
* `$.store.*`  获取store下的所有节点
* `$..book[2]` 获取第3个book节点
* `$..book[0,1]` 获取第1，2个book节点


```json
"store": {
    "book": [
        {
            "author": "...",
            "price": xxx
        },
        {
            "author": "...",
            "price": xxx
        }
    ]
}
```