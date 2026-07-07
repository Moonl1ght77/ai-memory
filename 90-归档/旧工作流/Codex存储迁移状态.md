---
title: Codex存储迁移状态
type: note
permalink: joe-memory/30-workflows/codex-storage-migration-state
tags:
- workflow,codex,storage
---

# Codex存储迁移状态

## 当前状态
- Basic Memory 已在 Codex MCP 中配置为 `basic-memory`。
- Basic Memory 项目名：`joe-memory`。
- 记忆库路径：`E:\AI-Memory\vault`。
- 配置目录：`E:\AI-Memory\basic-memory-config`。

## Codex 历史迁移
- 旧 Codex 日期目录已逐步迁移或兼容到 `E:\Codex-Workspace\history`。
- C 盘旧路径通过 junction 保持兼容。
- 当前活跃的 `2026-06-05` 目录暂未整体迁移。

## 后续收尾
关闭 Codex 后，可以使用已生成的迁移脚本把 Codex 根目录迁移到 E 盘，并在 C 盘保留 junction。
