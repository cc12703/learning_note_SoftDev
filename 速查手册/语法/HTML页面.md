


# HTML页面



## 点击坐标

* `clientX/Y` 相对于浏览器窗口，不随页面滚动而改变
* `pageX/Y`   相对于文档（body元素）, 随页面滚动而改变
    * `pageX/Y = clientX/Y + scrollX/Y`
* `offsetX/Y` 相对于被触发dom的左上角
* `layerX/Y`  相对于当前坐标系
* `screenX/Y` 相对于屏幕左上角

### 图示
![](http://picbed.cc12703.com/20221130121134.png)



## 尺寸

* `clientW/H` 元素的像素尺寸，包含内边距
* `offsetW/H` 元素的可见尺寸，包括内边距、边框

### 图示
![](http://picbed.cc12703.com/20221130121514.png)