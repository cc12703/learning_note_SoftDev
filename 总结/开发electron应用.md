

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
vue add @cc12703m/electron-builder
```



## 库
* [axios](https://github.com/axios/axios) 支持Promise的HTTP客户端
* [cheerio](https://github.com/cheeriojs/cheerio) 轻量级的JQuery核心实现
   * 用于解析HTML内容
* [electron-log](https://github.com/megahertz/electron-log) 输出日志
* [form-data](https://github.com/form-data/form-data) 创建muliipart/form-data流
   * 用于上传文件
* [moment](https://github.com/moment/moment/) 时间处理库
   * [中文文档](http://momentjs.cn/)
* [fs-extra](https://github.com/jprichardson/node-fs-extra) Node.js文件库的扩展
* [bluebird](https://github.com/petkaantonov/bluebird) Promise实现库
* [iconv-lite](https://github.com/ashtuchkin/iconv-lite) 编码转换库
* [watch](https://github.com/mikeal/watch) 文件夹监控
* [rimraf](https://github.com/isaacs/rimraf)  删除目录
