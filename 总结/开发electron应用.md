

# 开发electron应用


[TOC]


## 技术
* 使用typescript进行开发
* 应用界面使用vue.js框架进行开发



## 代码树
```
    tsconfig.json   ts配置文件
    vue.config.js   vue配置文件
    package.json    webpack配置文件
    public/        资源目录
    src/
        background.ts  系统启动文件
        main.ts       界面启动文件
        common/    公共代码
           types/   ts数据类型定义
        main/      后台功能代码（运行在主进程）
        render/    界面代码（运行在渲染进程）
           App.vue  主界面
           components/  通用组件
           router/     路径定义
           store/      数据管理
           pages/      页面
```


## 工程创建
1. 使用 vue cli 创建vue工程
```shell
vue create xxx
```
1. 使用 Vue CLI Plugin Electron Builder 转换工程
```shell
vue add electron-builder
```
1. 手动调整代码结构
