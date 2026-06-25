---
title: LLM Wiki
category: concepts
summary: 由 AI 代理全自动构建、维护和更新的结构化知识库，让 AI 将知识编译成笔记而非仅检索原文
tags: [知识管理, AI Agent, 知识库]
updated: 2026-06-04T14:58:00Z
created: 2026-06-04T14:58:00Z
sources: ["web:https://mp.weixin.qq.com/s/j7Rdxlf9roBGqFmhff0EFQ"]
tier: supporting
base_confidence: 0.58  # 1 source, quality bucket: blog
lifecycle: draft
lifecycle_changed: 2026-06-04
provenance:
  extracted: 0.80
  inferred: 0.15
  ambiguous: 0.05
relationships:
  - target: "[[Hermes Agent]]"
    type: implements
  - target: "[[Obsidian]]"
    type: uses
  - target: "[[RAG]]"
    type: replaces
---

# LLM Wiki

由 AI 代理（AI Agent）全自动构建、维护和更新的结构化知识库，构建你的第二大脑。^[extracted]

## 三层核心架构

### 1. Raw Sources（原始资料层）

丢进去的 PDF、网页、论文、代码文件。这是"真理的源头"，只读，永远不会被修改。^[extracted]

### 2. The Wiki（维基层）

包含无数 Markdown 文件的文件夹，完全由 AI 编写和掌管。AI 在这里生成：^[extracted]

- **实体页（Entity Pages）**：记录人物、项目、产品的核心事实
- **概念页（Concept Pages）**：提炼技术、方法论、思想的内涵
- **主题总结**：跨文档的综合分析

这些页面通过 `双向链接` 串联成知识图谱。^[extracted]

### 3. The Schema（指令层）

配置文件（如 `CLAUDE.md` 或 `SCHEMA.md`），告诉 AI：^[extracted]

- 核心目标是什么
- 应该遵循什么格式和规则来编写维基
- 如何组织和链接知识

## 目录结构

```
my-wiki/
├── raw/                    # 原始资料层（只读）
│   ├── papers/
│   ├── articles/
│   └── notes/
├── wiki/                   # 维基层（AI 管理）
│   ├── entities/
│   │   ├── andrej-karpathy.md
│   │   └── hermes-agent.md
│   ├── concepts/
│   │   ├── llm-wiki.md
│   │   └── rag-vs-wiki.md
│   └── INDEX.md
└── SCHEMA.md               # 指令层
```

## 与 RAG 的对比

| 方式 | RAG | LLM Wiki |
|------|-----|----------|
| 工作原理 | 每次检索原文 | 读已整理的笔记^[extracted] |
| 知识积累 | ❌ 无积累 | ✅ 越用越密^[extracted] |
| 可见性 | 黑盒 | 全量 markdown^[extracted] |
| 部署 | 向量库+GPU | 本地文件夹^[extracted] |

**核心区别：RAG 是"查资料"，LLM Wiki 是"翻笔记"**^[extracted]

## 设计理念

主要问题在于：**知识没有被消化**^[extracted]

- OpenClaw 的 Active Memory 只是"记录对话"
- RAG 只是"检索原文"
- 你需要的是让 AI **把知识编译成笔记**^[inferred]

## 相关概念

- [[Hermes Agent]] — 实现 LLM Wiki 的工具
- [[Obsidian]] — 可视化知识图谱工具
- [[RAG]] — 检索增强生成