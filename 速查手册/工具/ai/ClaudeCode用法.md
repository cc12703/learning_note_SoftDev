

# ClaudeCode用法



## 总体

### 配置文件
* `~/.claude/settings.json` 用户全局
* `.claude/settings.json`  项目相关


## claude.md

### 功能
* 项目上下文文件
* CC会自动加载

### 内容
* 核心文件说明
* 代码分格指南
* 测试说明
* 常见命令

## Hooks

### 功能
* 在CC的特定事件发生时，自动执行shell命令


### 用途
* 处理一些重复性的手工操作
    * 每次文件修改后自动运行lint
    * 每次ClaudeCode编辑文件后，自动修复代码风格



## SubAgents

### 功能
* 有独立的上下文
* 可以并行工作，最后合并结果

### 用途
* 将复杂任务分解为子任务，每个SubAgents执行一个子任务




## Commands

### 功能
* 可以快速被调起

### 用途
* 快速调用一些重复的工作流程

### 内嵌
* `/init`  使用CLAUDE.md初始化项目
* `/clear` 清除历史
* `/config` 打开配置界面
* `/memory` 编辑CLAUDE.md
* `/review` 代码审查
* `/todos`  列出待办事项


### 自定义
* 在`.claude/commands`目录中存储一个`markdown`文件




## Skills

### 特点
* 封装一个完整的工作流
* 被CC自动调用


## MCP

### 特点
* 访问外部资源



## Plugins

### 功能
* 打包完整的工作流
* 包括：Commands, Skills, Hooks, MCP