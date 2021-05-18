


# 开发android应用

[TOC]



## 原则
* 使用google推荐的应用架构
* 使用Jetpack中的功能库


## 技术
* 使用Kotlin进行开发
* 使用Kotlin协程进行异步开发
* 使用ORM库进行数据库操作
* 使用依赖注入进行依赖管理
* 使用界面绑定库简化重复代码
* 使用约束布局进行界面布局


## 框架

### 架构图
![](https://gitee.com/cc12703/figurebed/raw/master/img/20201111113003.png)

### 要点
* 精简界面控制器的代码（Activity, Fragment）
* 界面组织为一个Activity，多个Fragment的结构
* 数据、数据逻辑存放在viewModel中
* 数据从Repository中获取
* Repository屏蔽数据来源细节（本地的、网络的）
* 数据使用LiveData包装
* 数据状态使用Resource包装


## 代码树
```
/data  存放数据相关代码
    /repo 数据仓库
        XXXRespository
    /api  网络接口
        XXXSerice
    /db   数据库接口
        AppDatabase  数据库文件类
        XXXDao       数据库表类
    /dl   下载器接口
/info  数据定义
    XXXInfo   自定义数据类
    Resource  包装数据（将数据和状态整合在一起）
    Status    定义数据状态
/ui    存放界面相关代码，按功能分目录
    /common 共用代码
    /XXX
        XXXFragment  界面控制器
        XXXViewModel 数据模型 
/util   存放辅助代码
    XXXUtil
    /di   依赖注入相关
        XXXModule 
    /binding  界面绑定相关
/worker  后台作业
    XXXWorker  
LaunchActivity  启动界面，用于权限申请
MainActivity    主界面容器
MainApplication 
```


## 库
* Room 数据库ORM库
* [Retrofit2](https://github.com/square/retrofit) HTTP包装库
* hilt、dagger 依赖注入库
* Paging  分页库
* work-manager 后台作业库
* Navigation 应用界面导航库
* [okdownload](https://github.com/lingochamp/okdownload) 下载库
* recyclerview，[BRVAH](https://github.com/CymChad/BaseRecyclerViewAdapterHelper)  列表显示库
* [apache-compress](https://commons.apache.org/proper/commons-compress/) 压缩、解压缩库
* [permissions-dispatcher](https://github.com/permissions-dispatcher/PermissionsDispatcher)权限动态申请




