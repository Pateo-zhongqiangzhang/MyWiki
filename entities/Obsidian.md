---
title: Obsidian
category: entities
summary: 基于 Markdown 的双向链接笔记工具，与 LLM Wiki 天然兼容，让知识图谱可视化
tags: [工具, 笔记软件, 双向链接]
updated: 2026-06-04T14:58:00Z
created: 2026-06-04T14:58:00Z
sources: ["web:https://mp.weixin.qq.com/s/j7Rdxlf9roBGqFmhff0EFQ"]
tier: supporting
base_confidence: 0.58  # 1 source, quality bucket: blog
lifecycle: draft
lifecycle_changed: 2026-06-04
provenance:
  extracted: 0.85
  inferred: 0.10
  ambiguous: 0.05
relationships:
  - target: "[[LLM Wiki]]"
    type: visualizes
  - target: "[[Hermes Agent]]"
    type: compatible_with
---

# Obsidian

基于 Markdown 的双向链接笔记工具，让 LLM Wiki 生成的知识"看得见"。^[extracted]

## 与 LLM Wiki 的完美结合

LLM Wiki 本身就是 markdown 文件目录，天然兼容 Obsidian^[extracted]，所以结合 Obsidian 非常完美^[inferred]

不需要学新工具，不需要部署复杂的知识库平台。打开 Obsidian，指向 wiki 目录。^[extracted]

## 核心功能

- **Graph View**：一眼看清哪些概念是孤岛、哪些知识互联了^[extracted]
- **双向链接**：`wikilink` 语法让 AI 写页面时自动建立交叉引用^[extracted]
- **Dataview 插件**：按标签、类型做动态查询^[extracted]
- **标签系统**：一目了然的分类体系^[extracted]

## 人机协作

AI 负责录入、整理、链接、更新。你负责浏览、阅读、补充、纠错。人机协作的完美分工。^[extracted]

## 集成工作流

```
Hermes Agent → 生成 LLM Wiki → Obsidian 可视化
     ↓              ↓              ↓
  读取资料      编写笔记      浏览图谱
```

**LLM Wiki 提供方法论，Hermes Agent 负责自动执行，Obsidian 让一切可见可控** — 完整的个人知识库解决方案。^[extracted]

## 相关概念

- [[LLM Wiki]] — Wiki 内容来源
- [[Hermes Agent]] — Wiki 自动生成工具