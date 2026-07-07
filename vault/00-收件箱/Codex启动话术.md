---
title: Codex启动话术
type: note
permalink: joe-memory/quick-prompts/codex-start-prompts
tags:
- memory,prompt,codex
---

# Codex启动话术

## 通用启动
复制这一句发给 Codex：

```text
先按 joe-memory/index 做 Memory Check，再继续任务。
```

## SKU Image Workbench
```text
先按 joe-memory/index 做 Memory Check。这是 SKU Image Workbench 任务，再继续。
```

## 微信小程序上线
```text
先按 joe-memory/index 做 Memory Check。这是微信小程序上线任务，再继续。
```

## 工作室整理总控
```text
先按 joe-memory/index 做 Memory Check。这是工作室整理总控任务，再继续。
```

## 元提示词agent
```text
先按 joe-memory/index 做 Memory Check。这次项目是元提示词agent。主项目路径是 E:\元提示词agent。
```

## 跨平台文件版
适合 Claude Code、VS Code 智能体或不支持 Basic Memory MCP 的平台：

```text
先读取 E:\AI-Memory\vault\INDEX.md 做 Memory Check。这次项目是【项目名】。请根据 INDEX.md 只读取对应项目的 项目地图.md、当前进度.md、踩坑记录.md。不要扫描无关项目，不要读取客户素材，不要把聊天过程写进记忆。
```

## 任务结束时
```text
本轮任务结束。请判断哪些内容长期有用、以后会复用、能被验证；只有三个都是“是”的内容，才更新 Basic Memory。
```

## 检查标准
如果 Codex 没有先说明以下内容，就让它停下：

- 已读取哪些记忆
- 当前任务类型
- 本次不用读哪些范围
- 本次操作边界
