---
title: 热门缓存
updated: 2026-06-04T14:58:00Z
---

# 热门缓存

*约 500 字的语义快照，记录最近的活动。每次重大写入操作后更新。*

## 最近活动

- [2026-06-04T14:58] INGEST_RAW — 提取 Hermes Agent + LLM Wiki + Obsidian 微信文章，创建 5 个新页面（2 个概念，3 个实体）
- [2026-06-03T16:10] INGEST — 导入 spi-rpc 架构分析文档，新增 4 个概念页面 + 更新 4 个现有页面
- [2026-06-03T15:30] INGEST — 导入 spi-rpc 架构文档，创建 6 个页面（3 个概念，3 个实体）

## 活跃主题

**知识管理领域（新增）：**

- **LLM Wiki 架构** — 三层级（原始资料层、维基层、指令层）
- **Hermes Agent** — 自动化 LLM Wiki 构建工具
- **Obsidian 集成** — Graph View、双向链接、Dataview
- **RAG vs LLM Wiki** — 检索原文 vs 预编译笔记

**车载 SPI-RPC 领域：**

- **分层设计** — API 分发 协议 数据 传输 IO 层
- **可靠传输** — 滑动窗口、重传、超时机制
- **多后端支持** — SPI / TCC IPC 硬件抽象

## 关键要点

### 知识管理（新增）

- LLM Wiki 让 AI **把知识编译成笔记**（而非仅检索原文）^[extracted]
- 三层架构：Raw Sources（只读）→ Wiki（AI 管理）→ Schema（指令配置）^[extracted]
- 与 RAG 对比：Wiki 越用越密、可见性强、部署简单^[extracted]

### SPI-RPC

- 分层架构：遵循 AUTOSAR 思想，完全的硬件与业务分离^[extracted]
- 可靠传输：窗口大小 2，最大重试 200 次，超时 2000ms^[extracted]
- IO 抽象：通过函数指针抽象设备驱动，支持 SPI / TCC IPC 后端^[extracted]

## 待解决的矛盾

*暂无。*