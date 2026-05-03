

# ClaudeCode用法







## 权限

### 工具规则
* Allow: 可使用，无需手动批准
* Ask: 会提示确认
* Deny: 禁止使用

#### 优先级
* Deny -> Ask -> Allow

### 匹配工具
* Bash() : bash命令
* WebFetch() : 网络请求
* Read() : 文件读取
* Edit() : 文件编辑


## 配置

### 作用域
* User : 个人配置，应用到所有项目
* Project: 项目配置
* Local : 项目本地配置

#### 优先级
* 命令行参数 -> Local -> Project -> User

### 位置

#### Settings
* User : ~/.claude/settings.json
* Project : .claude/settings.json
* Local : .claude/settings.local.json

#### Subagents
* User : ~/.claude/agents/
* Project : .claude/agents/

#### MCP
* User : ~/.claude.json
* Project : .mcp.json

#### Plugins
* User : ~/.claude/settings.json
* Project : .claude/settings.json
* Local : .claude/settings.local.json

#### CLAUDE.md
* User : ~/.claude/CLAUDE.md
* Project : CLAUDE.md 或 .claude/CLAUDE.md
* Local : CLAUDE.local.md


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

### 渐进式披露
1. 发现：启动时只加载 `名称` 和 `描述` 信息
1. 激活：当任务匹配时，读取 `SKILL.md` 内容
1. 执行：根据需要加载其他文本或执行脚本

### 结构
```
skill-name/
    SKILL.md  # 必要
    scripts/  # 可执行代码
    references/ # 附加文档
    assets/     # 其他资源
```

### 格式
SKILL.md有固定的格式

#### 元数据
```
---
<key>: <value>
...
---
```
* name : 唯一标识符
* description ： 功能描述，使用时机
* allowed-tools ： 允许使用的工具


## Plugins

### 功能
* 打包完整的工作流
* 包括：Commands, Skills, Hooks, MCP