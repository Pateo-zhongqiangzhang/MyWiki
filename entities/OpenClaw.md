---
title: OpenClaw
category: entities
summary: 24 万星标的 AI Agent 工具，使用 Active Memory 记录对话，记忆会随任务增多而变得零碎
tags: [工具, AI Agent, GitHub]
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
    type: related_to
---

# OpenClaw

拥有 24 万星标的 AI Agent 工具，使用 Active Memory 系统记录对话历史^[extracted]

## 记忆系统

主要问题在于：**知识没有被消化**^[extracted]

- Active Memory 只是"记录对话"^[extracted]
- 随着会话和任务越来越多，记忆就会变得零碎^[extracted]

## 对比

| 特性 | OpenClaw | Hermes Agent |
|------|----------|--------------|
| 星标 | 24 万^[extracted] | 两个月破 10 万用户^[extracted] |
| 记忆系统 | Active Memory | LLM Wiki^[extracted] |
| 记忆状态 | 零碎[^1] | 结构化^[extracted] |

[^1]: 主要问题在于零碎的知识没有经过编译整合

## 相关概念

- [[Hermes Agent]] — LLM Wiki 实现工具
- [[LLM Wiki]] — 更结构化的知识管理方案