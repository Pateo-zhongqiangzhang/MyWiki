---
title: Wiki 索引
---

# Wiki 索引

*本索引自动维护。最后更新: 2026-06-04*

## 概念

- [[SPI-RPC 架构]] — 通过 SPI 总线和 GPIO 握手，在 SoC 与 MCU 之间实现全双工 RPC 通信的分层架构设计
- [[GID 消息寻址]] — SoC 与 MCU 之间消息的地址标识系统，消息映射表定义了超过 100 种消息类型
- [[GPIO 握手协议]] — SPI 传输前的双方就绪协商机制，通过输入/输出 GPIO 电平实现
- [[RpcT 传输协议]] — SPI-RPC 传输层类似 AUTOSAR TP 层的可靠传输协议，实现滑动窗口、重传、超时管理
- [[RpcIo 层]] — 通过函数指针表现硬件抽象，支持 SPI 和 TCC IPC 两种物理传输后端
- [[RpcDataLib 数据映射层]] — 提供 GrpId 到 data buffer 的映射管理、CAN 消息监控、IC/IVI 通道双向数据版本管理
- [[协议帧格式]] — SPI-RPC 固定 768 字节帧结构，包含帧头 SN 消息块 CRC 和帧尾，支持多种帧类型
- [[LLM Wiki]] — 由 AI 代理全自动构建、维护和更新的结构化知识库，让 AI 将知识编译成笔记而非仅检索原文
- [[RAG]] — 检索增强生成技术，每次检索原文返回答案，与 LLM Wiki 的预编译笔记模式不同
- [[Claude Code]] — 住在终端里的 AI 编程同事，基于 Agentic Coding 智能体式编程，能自主读取项目、执行命令、修改代码

## 日志

- [[Claude Code 实战-微信文章摘要]] — 15 个核心模块的 Claude Code 实战指南摘要

## 实体

- [[MCU 车身控制器]] — MCU 车身控制器
- [[librpc]] — 车载 SPI-RPC 通信的主体库，分层架构包括 API 分发/协议/数据/传输/IO 层，支持 SPI/TCC IPC 后端
- [[xbase]] — xbase 基础库
- [[Hermes Agent]] — 自带 LLM Wiki 的 AI Agent，开箱即用的个人知识库自动化构建工具
- [[Obsidian]] — 基于 Markdown 的双向链接笔记工具，与 LLM Wiki 天然兼容，让知识图谱可视化
- [[OpenClaw]] — 24 万星标的 AI Agent 工具，使用 Active Memory 记录对话，记忆会随任务增多而变得零碎

## 技能

## 参考资料

## 综合

## 日志