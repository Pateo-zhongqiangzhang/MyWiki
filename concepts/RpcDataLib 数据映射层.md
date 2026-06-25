---
title: RpcDataLib 数据映射层
category: concepts
summary: 提供 GrpId 到数据 buffer 的映射管理、CAN 消息监控、IC/IVI 通道双向数据版本管理
tags: [数据映射, CAN, GrpId]
updated: 2026-06-03T16:10:00Z
created: 2026-06-03T16:10:00Z
sources: ["file:Documents/architecture.md"]
tier: supporting
base_confidence: 0.58  # 1 source, quality bucket: code-arch
lifecycle: draft
lifecycle_changed: 2026-06-03
provenance:
  extracted: 0.90
  inferred: 0.05
  ambiguous: 0.05
relationships:
  - target: "[[SPI-RPC 架构]]"
    type: implements
  - target: "[[GID 消息寻址]]"
    type: maps
---

# RpcDataLib 数据映射层

数据映射层，上层为 `librpc`，目录在 `librpc-data/`，提供 GrpId 到数据 buffer 的映射管理。^[extracted]

## 核心功能

### GrpId 映射管理

将每个 GID 关联到一个数据 buffer，上层通过 GID 读写数据，无需关心底层协议细节。^[extracted]

### CAN 消息监控状态

监控 CAN 线上消息的健康状态：^[extracted]

- 丢失检测
- 超时检测
- 校验错误检测

### 双向数据版本管理

维护 MCU 和 SoC 两侧的数据版本信息，便于检测脏数据和协商同步。^[extracted]

### 通道区分

区分两种业务通道：^[extracted]

- **IC 通道**：仪表盘（Instrument Cluster）相关数据
- **IVI 通道**：车机（In-Vehicle Infotainment）相关数据

## 与协议层对接

协议层 `rpc_message_type_infos[]` 消息映射表中的 GrpId 字段直接对应 RpcDataLib 的数据映射。^[extracted]

```c
// 协议层定义消息类型
{
    .message = MESSAGE_BATT_STATE,
    .gid = RPC_DATA_BATT_STATE,      // 对应数据层的 GrpId
    .send_parser = batt_state_parser,
    .recv_parser = batt_state_parser,
}
```

## 设计价值

数据映射层存在的意义：^[inferred]

- **关注点分离**：底层协议只管传输，数据结构由数据层管理
- **版本控制**：检测两侧数据是否同步，避免读写过期数据
- **CAN 监控**：提供统一的消息健康状态查询接口
- **多通道**：IC/IVI 区离管理，避免数据混乱

## 相关概念

- [[SPI-RPC 架构]] — 数据层在架构中的位置
- [[GID 消息寻址]] — GrpId 定义与使用
- [[MCU 车身控制器]] —— CAN 消息来源