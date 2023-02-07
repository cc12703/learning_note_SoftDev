

# Three.js用法

[TOC]


## 总体

### 概念
* `OpenGL`：开放的绘图标准
* `WebGL`：js的3D图形接口，等于Javascript + OpenGL ES
* `Canvas`：HTML5的画布元素，使用WebGL进行3D绘制
* `Three.js`：js的3D库，基于WebGL进行封装

### 坐标系统
* 基于右手的笛卡尔坐标系

![](http://picbed.cc12703.com/20221226202328.png)


### 核心要素
* `相机(camera)`获取到`场景(scene)`内显示的内容，通过`渲染器(renderer)`渲染到画布上


![](http://picbed.cc12703.com/20221226202918.png)


### 基本要素
* 所有物体都是有一个个`小三角形`构成的，是`几何体`的最小单位
* `材质`就是物体的纹理，决定了模型的外表
* `网格(mesh)`用于将`几何体`和`材质`结合起来

![](http://picbed.cc12703.com/20221226203941.png)


### 要素关系

![](http://picbed.cc12703.com/20221226214439.png)



## 几何体

### 顶点
* 几何体的本质就是一系列的顶点构成
* 顶点包含`位置数据`和`颜色数据`
* 顶点法向量：每个顶点对应的一个方向向量
    * 用于方向性光源对几何体产生影响


### 变换
* 操作：移动、平移、旋转
* 本质改变几何体的顶点位置

### 其他

#### 材质数组
* 立方体：右，左，上，下，前，后


## 材质

### 概述
* 材质是对WebGL着色器代码的封装

### 分类
* `PointsMaterial` 点材质
* `LineBasicMaterial` 线基础材质
* `MeshBasicMaterial` 网络基础材质，不受带有方向光源影响
* `MeshLambertMaterial` 网络漫反射材质
* `MeshPhongMaterial`  网络高光材质

### 通用属性
* `side`  面的渲染方式（前面、后面、双面）
* `opacity` 透明程度，值 0 -- 1(不透明)
* `transparent` 是否开启透明


## 模型对象
* `Points` 点模型，将几何体的每个顶点渲染成一个方形区域
* `Line` 线模型，使用线将每个顶点连接起来
* `Mesh` 网格模型，通过三角形面来渲染所有顶点


## 光源



## 相机

### 投影
![](http://picbed.cc12703.com/20221227121303.png)


### 正投影相机
* `OrthographicCamera` 使用正投影法来计算几何体的投影

![](http://picbed.cc12703.com/20221227121504.png)


### 透视投影相机
* `PerspectiveCamera` 使用透视投影法来计算几何体的投影

![](http://picbed.cc12703.com/20221227121554.png)

