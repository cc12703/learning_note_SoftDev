


# UIAutomator2用法


## 控件定位

### 定位条件
* `<uobj> = d(text=<str>)`  文本中等于指定字符串
* `<uobj> = d(textContains=<str>)`  文本中包含指定字符串
* `<uobj> = d(textStartsWith=<str>)`  文本开头包含指定字符串
* `<uobj> = d(textMatches=<reg>)`   文本匹配指定正则表达式

* `<uobj> = d(className=<str>)`  类名等于指定字符串
* `<uobj> = d{classNameMatches=<reg>}`  类名匹配指定正则表达式

* `<uobj> = d(description=<str>)`  描述信息等于指定字符串
* `<uobj> = d(descriptionContains=<str>)`  描述信息包含指定字符串
* `<uobj> = d(descriptionStartsWith=<str>)`  描述信息开头包含指定字符串
* `<uobj> = d(descriptionMatches=<reg>)`   描述信息匹配指定正则表达式

* `<uobj> = d(packageName=<str>)`   包名等于指定字符串
* `d(packageNameMatches=<reg>)`   包名匹配指定正则表达式

* `<uobj> = d(resourceId=<str>)`   资源ID等于指定字符串
* `<uobj> = d(resourceIdMatches=<reg>)`   资源ID匹配指定正则表达式

* `<uobj> = d(<cond>).child(<cond>)`  获取所有的子节点和孙子节点
* `<uobj> = d(<cond>).siblings(<cond>)`  获取所有的兄弟节点
* `<uobj> = d(<cond>).child_by_text(<str>, allow_scroll_search=<bool>, className=<str>)` 带滚动的搜索特定文本的子节点和孙子节点

* `<uodj> = d(<cond>, <cond>)` 使用多个条件


### 界面上的相对位置
* `<uobj> = d(<A>).left(<B>)` 选择A节点左边的B节点
* `<uobj> = d(<A>).right(<B>)` 选择A节点右边的B节点
* `<uobj> = d(<A>).up(<B>)` 选择A节点上面的B节点
* `<uobj> = d(<A>).down(<B>)` 选择A节点下面的B节点



## 控件操作

### 遍历
* `<uobj>.count`  节点个数
* `<uobj>[<index>]`  按索引值获取节点
* `for <item> in <uobj>`  遍历节点

* `<uobj>.exists`  判断是否存在
* `<uobj>.info`  获取节点信息


### 带超时等待
* `<uobj>.exists(timeout=<sec>)` 判断是否存在
* `<bool> = <uobj>.wait()`  等待出现
* `<bool> = <uobj>.wait_gone()`  等待消失

* `<uobj>.click(timeout=<sec>)`  点击
* `<uobj>.click(offset=(x,y))`   带偏移的点击
    * `(0.5, 0.5)` 中点
    * `(0, 0)`  左上角
    * `(1, 1)`  右下角

* `<bool> = <uobj>.click_exists()` 节点出现才点击
* `<bool> =  <uobj>.click_gone()`  节点消失才点击  
* `<uobj>.long_click()`   长按

* `<str> = <uobj>.get_text()` 获取文本值
* `<uobj>.set_text(<str>)` 设置文本值
* `<uobj>.clear_text()`  清除文本值


## 界面操作
* `d.swipe_ext("right")` 手指右滑
* `d.swipe_ext("left")` 手指左滑
* `d.swipe_ext("up")` 手指上滑
* `d.swipe_ext("down")` 手指下滑
* `d.swipe_ext(<direction>, scale=<percent>)` 指定滑动范围（屏幕宽度的百分比）
* `d.swipe_ext(<direction>, duration=<ms>)`   在指定时间内滑动
* `d.swipe_ext(<direction>, steps=<int>)`     在指定步数内滑动（1步为5毫秒） 




## 其他

* `<output>, <exitCode> = d.shell(<cmd>)`  执行shell命令