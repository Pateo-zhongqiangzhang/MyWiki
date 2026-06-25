---
title: Hermes Agent
category: entities
summary: 自带 LLM Wiki 的 AI Agent，开箱即用的个人知识库自动化构建工具
tags: [工具, AI Agent, Knowledge Base]
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
    type: implements
  - target: "[[Obsidian]]"
    type: compatible_with
---

# Hermes Agent

自带 LLM Wiki 功能的 AI Agent，两个月突破 10 万用户，开箱即用的个人知识库自动化构建工具。^[extracted]

## 快速开始

```bash
# 1. 安装 Hermes（需要 Python 3.10+）
pip install hermes-agent

# 2. 初始化
hermes init

# 3. 配置 LLM Wiki 技能
hermes skill enable llm-wiki

# 4. 开始使用
hermes chat
> /llm-wiki ingest https://xxx.org/abs/xxxx
```

## 自动化功能

Hermes 会自动：^[extracted]

- 读取文章
- 创建实体页、概念页
- 建立交叉链接
- 更新索引

## 查询知识库

```bash
> /llm-wiki query "XXXX"
```

## 工作流程

AI 读完内容，拆成多个页面：^[extracted]

- 实体页记录核心事实
- 概念页提炼技术内涵
- 对比分析做结构化比较
- 查询归档按主题归档调研结果

每个页面都包含 wikilinks，指向关联页面。知识网络像乐高一样拼起来。^[extracted]

## 对比

| 特性 | OpenClaw | Hermes Agent |
|------|----------|--------------|
| 星标 | 24 万 | 两个月破 10 万用户^[extracted] |
| 记忆系统 | Active Memory | LLM Wiki^[extracted] |
| 记忆状态 | 零碎[^1] | 结构化^[extracted] |

[^1]: 随着会话和任务越来越多，记忆就会变得零碎

## 相关概念

- [[LLM Wiki]] — 核心知识库架构
- [[Obsidian]] — 可视化工具