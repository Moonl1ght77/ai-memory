---
title: AI多平台记忆使用手册
type: note
permalink: joe-memory/quick-prompts/multi-platform-memory-manual
tags:
- memory,manual,multi-platform,codex,claude,vscode
---

# AI多平台记忆使用手册

这份手册只解决一件事：Joe 在 Codex、Claude Code、VS Code 智能体和其它 AI 平台里切换项目时，怎么保证所有 AI 都围绕同一个记忆中心工作，不再各写各的、各记各的。

可视化页面：

```text
E:\AI-Memory\roadbook\ai-memory-multi-platform-manual.html
```

双击打开：

```text
E:\AI-Memory\tools\open-memory-manual.cmd
```

## 0. 总原则

只同步项目状态入口，不同步聊天记录。

长期记忆中心只有一个：

```text
E:\AI-Memory\vault
```

项目本体各放各的：

```text
E:\元提示词agent
E:\Projects\sku-image-workbench
E:\乐顺科纺程序
```

客户素材、参考图、交付图、项目内部知识库不要搬进总记忆。总记忆只负责告诉 AI：

```text
这个项目在哪里
现在做到哪
下次先读什么
哪些坑不要再踩
```

## 1. 目录分工

### Basic Memory 总记忆

位置：

```text
E:\AI-Memory\vault
```

用途：

```text
AI 路由
项目状态
长期规则
踩坑记录
跨平台协作入口
```

不放：

```text
项目源码
客户图片
完整聊天记录
项目内部知识库全文
一次性临时想法
```

### 项目本体

例子：

```text
E:\元提示词agent
E:\Projects\sku-image-workbench
E:\乐顺科纺程序
```

用途：

```text
源码
手册
模板
项目内部知识库
示例
可视化页面
Git 提交
```

### 项目内部知识库

例子：

```text
E:\元提示词agent\知识库
```

用途：

```text
提示词规则
追问规则
类目知识
客户知识
案例沉淀
模板细节
```

它属于项目本体，不属于 Basic Memory。Basic Memory 只记录它的入口：

```text
元提示词agent 的知识库位于 E:\元提示词agent\知识库。
改提示词规则先读 知识库\规则\提示词格式规则.md。
改追问逻辑先读 知识库\规则\追问路由规则.md。
改类目经验先读 知识库\类目知识\<类目>.md。
```

## 2. 开工流程

### 场景 A：在当前 Codex 总控对话里聊项目

复制：

```text
先按 joe-memory/index 做 Memory Check。这次聊的是【项目名】。
```

例子：

```text
先按 joe-memory/index 做 Memory Check。这次聊的是元提示词agent。
```

AI 应该先说明：

```text
已读取哪些记忆
当前任务属于哪个项目
本次不读取哪些范围
本次操作边界是什么
```

### 场景 B：在 Codex 其它项目对话里继续

复制：

```text
先按 joe-memory/index 做 Memory Check。这次项目是【项目名】。主项目路径是【项目路径】。
```

例子：

```text
先按 joe-memory/index 做 Memory Check。这次项目是元提示词agent。主项目路径是 E:\元提示词agent。
```

### 场景 C：在 Claude Code / VS Code 智能体 / 其它平台继续

如果平台不能直接用 Basic Memory MCP，复制文件路径版：

```text
先读取 E:\AI-Memory\vault\INDEX.md 做 Memory Check。这次项目是【项目名】。请根据 INDEX.md 只读取对应项目的 项目地图.md、当前进度.md、踩坑记录.md。不要扫描无关项目，不要读取客户素材，不要把聊天过程写进记忆。
```

如果平台支持 Basic Memory MCP，复制短句：

```text
先按 joe-memory/index 做 Memory Check。这次项目是【项目名】。
```

## 3. 开工验收

AI 开始干活前，必须先给出类似下面的确认：

```text
我已读取 joe-memory/index 和 元提示词agent 的 项目地图/当前进度/踩坑记录。
本次任务属于元提示词agent。
本次不读取 SKU、小程序、客户图片资产。
本次只检查提示词规则和使用手册，不改无关文件。
```

如果 AI 没说清楚，打断它：

```text
先停下。请先说明你读取了哪些记忆、当前任务属于哪个项目、本次不读哪些范围、本次操作边界是什么。
```

## 4. 执行边界

允许默认读取：

```text
当前项目三份记忆
当前项目必要文件
当前任务相关配置、日志、模板
```

不允许默认读取：

```text
其它项目源码
客户图片资产
整个 Obsidian 库
整个聊天记录
无关旧目录
```

如果 AI 开始大范围扫描，复制：

```text
先停下。请只按 INDEX.md 的当前项目路线读取，不要扫描无关项目和客户素材。
```

## 5. 收尾流程

每轮任务结束后，先让 AI 做筛选，不要直接写。

复制：

```text
本轮任务结束。请判断哪些内容长期有用、以后会复用、能被验证。只有三个都是“是”的内容，才更新 E:\AI-Memory\vault 里对应项目的 项目地图.md、当前进度.md、踩坑记录.md。不要写入一次性聊天过程、客户图片素材、临时想法。
```

写入前要求它列计划：

```text
写入前先列出：准备写入哪一个文件、为什么长期有用、以后怎么复用、如何验证。确认后再写。
```

## 6. 三份记忆写什么

### 项目地图.md

写稳定事实：

```text
项目定位
正式路径
核心资产
技术栈
固定入口
长期原则
重要外部链接
```

不要写：

```text
当天过程
临时聊天
没有验证的猜测
一次性素材
```

### 当前进度.md

写当前状态：

```text
现在做到哪
最近提交
下一步
当前阻塞
当前不做什么
```

不要写：

```text
稳定架构全文
大量旧记录
客户图片细节
完整任务过程
```

### 踩坑记录.md

写已确认问题：

```text
失败方案
已知风险
复现条件
规避策略
验证过的修复方法
```

不要写：

```text
情绪化评价
未验证的猜测
泛泛而谈的建议
无关平台体验
```

## 7. 写入质量判断

写入前问三句话：

```text
长期有用吗？
以后会复用吗？
能被验证吗？
```

三个都是“是”才写入长期记忆。否则留在当前任务，不进入 Basic Memory。

## 8. 同步完成检查

写完记忆后，让 AI 做最后一步：

```text
请重新索引 Basic Memory，并验证对应 permalink 能读到最新内容。
```

合格闭环是：

```text
项目修改
Git 提交
筛选长期有效内容
写入对应三份记忆
重新索引 Basic Memory
验证 permalink 可读
```

## 9. 回到总控要不要再说一遍

如果你在其它项目对话里已经完成了完整闭环：

```text
项目修改
Git 提交
筛选长期有效内容
写入对应三份记忆
重新索引 Basic Memory
验证 permalink 可读
```

那么回到总控对话时，原则上不用再重复汇报。因为最新状态已经写进：

```text
E:\AI-Memory\vault
```

总控对话下次只要按 `joe-memory/index` 做 Memory Check，就应该能读到最新状态。

需要回到总控说的情况只有两种。

### 需要核对写得对不对

复制：

```text
我刚在【项目名】里做了收尾记忆同步，请你只读检查对应三份记忆，看写得对不对，不要改文件。
```

### 需要跨项目总览和优先级

复制：

```text
我今天几个项目都做了记忆同步。请按 joe-memory/index 做 Memory Check，只读各项目当前进度，帮我总结现在每个项目状态和下一步优先级。
```

### 不需要回总控重复汇报时

如果项目对话已经完成“写入三份记忆、重新索引、验证 permalink 可读”，不用再把过程复制回总控。下次在总控只需要说：

```text
先按 joe-memory/index 做 Memory Check。
```

如果想指定某个项目：

```text
先按 joe-memory/index 做 Memory Check。这次看【项目名】的最新状态。
```

### 不确定有没有同步成功时

复制：

```text
我不确定【项目名】刚才有没有同步成功。请只读检查对应三份记忆和 Basic Memory permalink，告诉我是否已经是最新，不要改文件。
```

不要在总控里重复粘贴完整项目过程。否则又会变成聊天记录同步。

正确分工：

```text
项目对话：负责干活和写回记忆
总控对话：负责盘点、核对、跨项目决策
Basic Memory：负责承接所有项目状态
```

## 10. 常用项目话术

### 元提示词agent

Codex / 支持 Basic Memory MCP 时：

```text
先按 joe-memory/index 做 Memory Check。这次项目是元提示词agent。主项目路径是 E:\元提示词agent。
```

Claude Code / VS Code 智能体 / 不确定是否支持 MCP 时：

```text
先读取 E:\AI-Memory\vault\INDEX.md 做 Memory Check。这次项目是元提示词agent。主项目路径是 E:\元提示词agent。请根据 INDEX.md 只读对应项目三份记忆，不要扫描无关项目和客户素材。
```

对应 Basic Memory permalink：

```text
joe-memory/20-projects/meta-prompt-agent/context
joe-memory/20-projects/meta-prompt-agent/progress
joe-memory/20-projects/meta-prompt-agent/bugs
```

对应本地记忆文件：

```text
E:\AI-Memory\vault\20-项目记忆\元提示词agent\项目地图.md
E:\AI-Memory\vault\20-项目记忆\元提示词agent\当前进度.md
E:\AI-Memory\vault\20-项目记忆\元提示词agent\踩坑记录.md
```

常用命令用途：

- `read-note .../context`：读取项目地图，确认 Agent 项目定位、主目录、知识库入口和长期规则。
- `read-note .../progress`：读取当前进度，确认最近优化、提交状态、下一步维护重点。
- `read-note .../bugs`：读取踩坑记录，确认多图台账、防幻觉、图片引用自检等风险规则。
- `search-notes '元提示词agent'`：搜索该项目相关记忆，确认索引和项目名是否能被找到。

```powershell
$env:BASIC_MEMORY_CONFIG_DIR='E:\AI-Memory\basic-memory-config'
uvx basic-memory tool read-note 'joe-memory/20-projects/meta-prompt-agent/context' --project joe-memory --local
uvx basic-memory tool read-note 'joe-memory/20-projects/meta-prompt-agent/progress' --project joe-memory --local
uvx basic-memory tool read-note 'joe-memory/20-projects/meta-prompt-agent/bugs' --project joe-memory --local
uvx basic-memory tool search-notes '元提示词agent' --project joe-memory --local
uvx basic-memory reindex --full --project joe-memory
```

### SKU Image Workbench

Codex / 支持 Basic Memory MCP 时：

```text
先按 joe-memory/index 做 Memory Check。这次项目是 SKU Image Workbench。主项目路径是 E:\Projects\sku-image-workbench。
```

Claude Code / VS Code 智能体 / 不确定是否支持 MCP 时：

```text
先读取 E:\AI-Memory\vault\INDEX.md 做 Memory Check。这次项目是 SKU Image Workbench。主项目路径是 E:\Projects\sku-image-workbench。请根据 INDEX.md 只读对应项目三份记忆，不要扫描无关项目和客户素材。
```

对应 Basic Memory permalink：

```text
joe-memory/20-projects/sku-image-workbench/context
joe-memory/20-projects/sku-image-workbench/progress
joe-memory/20-projects/sku-image-workbench/bugs
```

对应本地记忆文件：

```text
E:\AI-Memory\vault\20-项目记忆\无限智能画布-sku-image-workbench\项目地图.md
E:\AI-Memory\vault\20-项目记忆\无限智能画布-sku-image-workbench\当前进度.md
E:\AI-Memory\vault\20-项目记忆\无限智能画布-sku-image-workbench\踩坑记录.md
```

常用命令用途：

- `read-note .../context`：读取项目地图，确认无限智能画布/SKU 工具定位、主目录、技术栈和核心入口。
- `read-note .../progress`：读取当前进度，确认迁移状态、构建验证、Git 状态和下一步。
- `read-note .../bugs`：读取踩坑记录，确认迁移、依赖、构建和工作区风险。
- `search-notes 'SKU Image Workbench'`：搜索该项目相关记忆，确认索引是否能搜到。

```powershell
$env:BASIC_MEMORY_CONFIG_DIR='E:\AI-Memory\basic-memory-config'
uvx basic-memory tool read-note 'joe-memory/20-projects/sku-image-workbench/context' --project joe-memory --local
uvx basic-memory tool read-note 'joe-memory/20-projects/sku-image-workbench/progress' --project joe-memory --local
uvx basic-memory tool read-note 'joe-memory/20-projects/sku-image-workbench/bugs' --project joe-memory --local
uvx basic-memory tool search-notes 'SKU Image Workbench' --project joe-memory --local
uvx basic-memory reindex --full --project joe-memory
```

### 微信小程序上线

Codex / 支持 Basic Memory MCP 时：

```text
先按 joe-memory/index 做 Memory Check。这次项目是微信小程序上线。主项目路径是 E:\乐顺科纺程序。
```

Claude Code / VS Code 智能体 / 不确定是否支持 MCP 时：

```text
先读取 E:\AI-Memory\vault\INDEX.md 做 Memory Check。这次项目是微信小程序上线。主项目路径是 E:\乐顺科纺程序。请根据 INDEX.md 只读对应项目三份记忆，不要扫描无关项目和客户素材。
```

对应 Basic Memory permalink：

```text
joe-memory/20-projects/wechat-miniprogram/context
joe-memory/20-projects/wechat-miniprogram/progress
joe-memory/20-projects/wechat-miniprogram/bugs
```

对应本地记忆文件：

```text
E:\AI-Memory\vault\20-项目记忆\微信小程序上线\项目地图.md
E:\AI-Memory\vault\20-项目记忆\微信小程序上线\当前进度.md
E:\AI-Memory\vault\20-项目记忆\微信小程序上线\踩坑记录.md
```

常用命令用途：

- `read-note .../context`：读取项目地图，确认乐顺科纺小程序定位、主目录、上线备案范围和关键入口。
- `read-note .../progress`：读取当前进度，确认上线前检查状态、待办事项和当前不该改的范围。
- `read-note .../bugs`：读取踩坑记录，确认备案、环境、后台暴露、订阅消息等已知上线风险。
- `search-notes '微信小程序上线'`：搜索该项目相关记忆，确认索引是否能搜到。

```powershell
$env:BASIC_MEMORY_CONFIG_DIR='E:\AI-Memory\basic-memory-config'
uvx basic-memory tool read-note 'joe-memory/20-projects/wechat-miniprogram/context' --project joe-memory --local
uvx basic-memory tool read-note 'joe-memory/20-projects/wechat-miniprogram/progress' --project joe-memory --local
uvx basic-memory tool read-note 'joe-memory/20-projects/wechat-miniprogram/bugs' --project joe-memory --local
uvx basic-memory tool search-notes '微信小程序上线' --project joe-memory --local
uvx basic-memory reindex --full --project joe-memory
```

### 工作室整理总控

```text
先按 joe-memory/index 做 Memory Check。这次项目是工作室整理总控。
```

### 实时原料看盘

Codex / 支持 Basic Memory MCP 时：

```text
先按 joe-memory/index 做 Memory Check。这次项目是实时原料看盘。主项目路径是 E:\实时原料看盘。
```

Claude Code / VS Code 智能体 / 不确定是否支持 MCP 时：

```text
先读取 E:\AI-Memory\vault\INDEX.md 做 Memory Check。这次项目是实时原料看盘。主项目路径是 E:\实时原料看盘。请根据 INDEX.md 只读对应项目三份记忆，不要扫描无关项目和客户素材。
```

对应 Basic Memory permalink：

```text
joe-memory/20-projects/realtime-material-panel/context
joe-memory/20-projects/realtime-material-panel/progress
joe-memory/20-projects/realtime-material-panel/bugs
```

对应本地记忆文件：

```text
E:\AI-Memory\vault\20-项目记忆\实时原料看盘\context.md
E:\AI-Memory\vault\20-项目记忆\实时原料看盘\progress.md
E:\AI-Memory\vault\20-项目记忆\实时原料看盘\bugs.md
```

常用命令：

这些命令主要用于排查和验证 Basic Memory 是否能读到 `实时原料看盘` 的最新记忆。

- `$env:BASIC_MEMORY_CONFIG_DIR=...`：指定 Basic Memory 配置目录。当前命令行会话里先执行这一句，后面的 `uvx basic-memory` 才知道读取哪套配置。
- `read-note .../context`：读取项目地图，确认项目定位、主目录、技术栈、数据源和长期入口。
- `read-note .../progress`：读取当前进度，确认现在做到哪、下一步是什么、有没有阻塞。
- `read-note .../bugs`：读取踩坑记录，确认已知问题、风险和规避策略。
- `search-notes '实时原料看盘'`：搜索项目相关记忆，适合不确定 permalink 是否正确或想确认索引是否能搜到。
- `reindex --full`：重建 Basic Memory 索引。修改 `E:\AI-Memory\vault` 里的记忆文件后执行，确保 MCP 和搜索能读到最新内容。

```powershell
$env:BASIC_MEMORY_CONFIG_DIR='E:\AI-Memory\basic-memory-config'
uvx basic-memory tool read-note 'joe-memory/20-projects/realtime-material-panel/context' --project joe-memory --local
uvx basic-memory tool read-note 'joe-memory/20-projects/realtime-material-panel/progress' --project joe-memory --local
uvx basic-memory tool read-note 'joe-memory/20-projects/realtime-material-panel/bugs' --project joe-memory --local
uvx basic-memory tool search-notes '实时原料看盘' --project joe-memory --local
uvx basic-memory reindex --full --project joe-memory
```

## 11. Joe 只需要记三句

开工：

```text
先按 joe-memory/index 做 Memory Check。这次项目是【项目名】。
```

跨平台：

```text
先读取 E:\AI-Memory\vault\INDEX.md 做 Memory Check。这次项目是【项目名】。
```

收尾：

```text
本轮任务结束，只把长期有用、以后会复用、能验证的内容更新到对应项目三份记忆。
```
