---
title: Claude Code
category: concepts
summary: 住在终端里的 AI 编程同事，基于 Agentic Coding 智能体式编程，能自主读取项目、执行命令、修改代码
tags: [Claude Code, AI编程, 智能体]
updated: 2026-06-04T17:10:00Z
created: 2026-06-04T17:10:00Z
sources: ["web:https://mp.weixin.qq.com/s/2ZFJe8L9vXQHDR4kJGIeoA"]
tier: supporting
base_confidence: 0.58  # 1 source, quality bucket: blog
lifecycle: draft
lifecycle_changed: 2026-06-04
provenance:
  extracted: 0.85
  inferred: 0.10
  ambiguous: 0.05
relationships:
  - target: "[[Agentic Coding]]"
    type: implements
  - target: "[[MCP]]"
    type: uses
  - target: "[[Agent SDK]]"
    type: includes
---

# Claude Code

住在终端里的 AI 编程同事，基于 Agentic Coding 智能体式编程，能自主读取项目、执行命令、修改代码、查看测试结果并调整方案。^[extracted]

## 核心特性

不是传统工具的三种误解：^[extracted]

- ❌ 代码补全工具
- ❌ 聊天机器人加文件读取能力  
- ❌ 简单的问答工具

真正的本质：**开发环境里的编程智能体**^[extracted]

## 工作循环

任务执行的完整循环：^[extracted]

```
理解目标 → 读取上下文 → 调用工具 → 观察结果 → 修改方案 → 再次验证
```

这是一个持续迭代、自主展开行动的过程。

## 关系变化

从 "我问你答" 转变为：^[extracted]

**"我分配任务，你执行，我审查结果"**

这种关系变化需要调整使用方式：

- 不要只问："这个函数什么意思？"
- 应该说："请先理解这个模块，再告诉我风险点，最后给出修改计划。"

## 入门三件事

1. 安装 `claude` 命令^[extracted]
   ```bash
   # macOS / Linux / WSL
   curl -fsSL https://claude.ai/install.sh|bash
   # Windows PowerShell
   irm https://claude.install.ps1|iex
   ```

2. 登录账号或配置企业凭证^[extracted]

3. 在项目目录中启动^[extracted]

## 账号要求

需要以下账号之一：^[extracted]

- Claude Pro、Max、Team、Enterprise、Console 账号
- 或通过 Bedrock、Vertex、Foundry 等第三方云供应商接入

免费 Claude.ai 账号不包含 Claude Code 使用权限。

## 核心模块概览

- 上下文管理 — 让同事真正懂项目
- 模型和 effort — 不同难度用合适的脑力
- Subagent、Worktree、Agent Team — 分工协作
- MCP、Skills、Plugins — 接外部系统和专业能力
- Hooks、Routines、CI/CD — 进入自动化流水线
- 权限、安全、沙盒 — 跑得动、跑得稳、跑得安全
- SDK、部署、监控 — 从个人工具到企业能力
- 个性化与速查 — 成为日常生产工具

## 主线总结

**先把 Claude Code 用起来，再让它懂项目；先让它帮你做事，再让它安全、可控、可扩展地进入团队和企业流程。**^[extracted]

## 相关概念

- [[Agentic Coding]] — 核心编程方式
- [[MCP]] — 硬件抽象，支持多种协议
- [[Agent SDK]] — 用代码构建自己的 Claude Agent
- [[RAG]] — 检索增强生成（作为对比参考）