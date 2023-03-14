


# Compose用法

## 要点

* 所有函数都要加`@Composable`标注



## 装饰符

* 一个对象 `Modifier`
* 调用函数的顺序非常重要，会影响最终效果


### 函数
* `clickable { ... }`             用户点击
* `background(<color>)` 背景颜色
* `border(<size>, <color>, <corner-shape)` 边框
* `padding(<all>)`      内边距
* `padding(<start>, <top>, <end>, <bottom>)` 内边距
* `padding(<horizontal>, <vertical>)`  内边距
* `size(<width>, <height>)`  大小，受约束条件影响
* `requireSize(<width>, <height>)`  固定大小，不受约束条件影响
* `fillMaxHeight()` 填充所有可用高度
* `fillMaxWidth()`  填充所有可用宽度

## 布局

### 函数
* `Box { ... }`   放置在元素的上面
* `Column { ... }` 在垂直方向上线性布局（一个列中）

* `Row { ... }`  在水平方向上线性布局（一个行中）
* `Row (horizontalArrangement=xxx)`  子控件在水平方向上的排列方式
* `Row (verticalAlignment=xxx)`  子控件在垂直方向上的对齐方式
    * `Top` 顶部对齐
    * `CenterVertically` 居中对齐
    * `Bottom` 底部对齐


### 示例
```koltin
@Composable
fun ArtistCard(artist: Artist) {
    Row(verticalAlignment = Alignment.CenterVertically) {
        Image(/*...*/)
        Column {
            Text(artist.name)
            Text(artist.lastSeenOnline)
        }
    }
}
```

![](http://picbed.cc12703.com/20230224141223.png)


**Arrangment值的效果**

![](http://picbed.cc12703.com/20230224143039.png)




## 控件

### 文本
* `Text(<str>)` 显示字符串
* `Text(stringResource(R.string.xxx))` 显示字符串资源
* `Text(..., color=xxx)` 设置颜色
* `Text(..., fontSize=xxx)`  设置字号
* `Text(..., textAlign=xxx)` 设置文字对齐方式
* `Text(..., overflow=xxx)`  设置文字溢出方式
    * `TextOverflow.Clip`  回折
    * `TextOverflow.Ellipsis` 显示省略号