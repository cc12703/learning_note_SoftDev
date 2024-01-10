



# Pnpm用法


## 命令

* `pnpm install`  安装项目依赖
* `pnpm add <pkg>`  安装依赖包，保存到dependencies
* `pnpm add -D <pkg>` 安装依赖包，保存到devDependencies

* `pnpm remove <pkg>` 移除依赖包

* `pnpm up` 更新项目依赖
* `pnpm up <pkg>` 更新指定依赖包

* `pnpm link --global` 将本地包链接到全局node_modules中，本地发布
* `pnpm unlink --global` 断开本地包到全局node_modules中的链接

* `pnpm link --global <pkg>` 从全局的node_modules中链接指定包，本地引用
* `pnpm uninstall --global <pkg>` 移除指定包的链接