---
title: RAG
category: concepts
summary: 检索增强生成技术，每次检索原文返回答案，与 LLM Wiki 的预编译笔记模式不同
tags: [AI, RAG, 检索, 知识管理]
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
  - target: "[[LLM Wiki]]"
    type: replaced_by
---

# RAG

检索增强生成（Retrieval-Augmented Generation），每次检索原文返回答案的技术。^[extracted]

## 与 LLM Wiki 的对比

| 方式 | RAG | LLM Wiki |
|------|-----|----------|
| 工作原理 | 每次检索原文 | 读已整理的笔记^[extracted] |
| 知识积累 | ❌ 无积累 | ✅ 越用越密^[extracted] |
| 可见性 | 黑盒 | 全量 markdown^[extracted] |
| 部署 | 向量库+GPU | 本地文件夹^[extracted] |

## 局限性

RAG 技术的主要问题在于：**知识没有被消化**^[extracted]

- 只是"检索原文"不进行结构化的编译^[extracted]
- 没有积累，每次都是重新检索^[extracted]
- 是"查资料"而非"翻笔记"^[extracted]

## 相关概念

- [[LLM Wiki]] — 预编译笔记的替代方案
- [[OpenClaw]] — 使用 Active Memory（类似 RAG）的 Agent 工具

## 实体

- Andrej Karpathy — 提出 LLM Wiki 概念的研究者^[extracted]