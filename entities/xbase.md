---
title: xbase 基础库
category: entities
summary: 提供事件循环、链表、定时器、信号量等基础设施的基础工具库
tags: [库, 基础设施]
updated: 2026-06-03T15:30:00Z
created: 2026-06-03T15:30:00Z
sources: ["file:Documents/README-architecture.md"]
tier: supporting
base_confidence: 0.67
lifecycle: draft
lifecycle_changed: 2026-06-03
provenance:
  extracted: 0.85
  inferred: 0.10
  ambiguous: 0.05
relationships:
  - target: "[[librpc]]"
    type: underlies
---

# xbase 基础库

基础设施工具库，目录位于 `core/xbase/`，支撑整个 SPI-RPC 系统的异步事件驱动。^[extracted]

## 核心模块

| 模块 | 文件 | 作用 |
|------|------|------|
| 事件循环 | polling.c | epoll 事件循环，驱动所有异步事件 |
| 定时器 | xtime.c | 基于 epoll 的定时器（非阻塞） |
| 链表 | xlist.c | 侵入式双向链表（消息队列骨架） |
| 阻塞原语 | xblock.c | 同步阻塞（SyncRequest 的阻塞等待） |
| 信号量 | xsemaphore.c | 线程启动同步 |
| CRC 校验 | xcrc.c | CRC 校验计算^[extracted] |
| 内存块管理 | xblock.c | 内存块管理^[extracted] |

## 设计特点

### 非阻塞异步模型
所有 I/O 操作基于 epoll 事件驱动，不使用阻塞式 sleep。^[extracted]

### 侵入式链表
xlist.c 提供的链表节点嵌入到数据结构中，避免额外的内存分配。^[inferred]

### 线程安全
提供信号量用于线程间同步。^[extracted]

## 在 SPI-RPC 中的使用

| 目标 | xbase 模块 |
|------|-----------|
| 主事件循环 | polling（驱动 xrpc-lib 线程） |
| 定时轮询 | xtime（心跳、超时检测） |
| 发送队列 | xlist（拥塞控制的队列） |
| 同步等待 | xblock（SyncRequest 阻塞等待） |
| 线程启动 | xsemaphore（启动时同步） |

## 依赖关系

```
librpc → 依赖 xbase
```

librpc 不直接使用标准库的阻塞 I/O，而通过 xbase 实现 async 模式。^[inferred]

## 相关实体

- [[librpc]] — 使用 xbase 的上层库