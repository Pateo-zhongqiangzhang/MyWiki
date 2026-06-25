---
title: SPI-RPC 架构
category: concepts
summary: 通过 SPI 总线和 GPIO 握手，在 SoC 与 MCU 之间实现全双工 RPC 通信的分层架构设计，遵循 AUTOSAR 分层思想
tags: [通信-架构, SPI, RPC, 分层设计]
updated: 2026-06-03T16:10:00Z
created: 2026-06-03T15:30:00Z
sources: ["file:Documents/README-architecture.md", "file:Documents/architecture.md"]
tier: core
base_confidence: 0.75  # 2 sources, quality bucket: code-arch
lifecycle: draft
lifecycle_changed: 2026-06-03
provenance:
  extracted: 0.85
  inferred: 0.10
  ambiguous: 0.05
relationships:
  - target: "[[GID 消息寻址]]"
    type: uses
  - target: "[[GPIO 握手协议]]"
    type: uses
  - target: "[[MCU 车身控制器]]"
    type: communicate_with
---

# SPI-RPC 架构

SPI-RPC 是运行在车载 SoC 上的 **SPI 全双工 RPC 中间件**，用于保时捷项目中 Android SoC (高通 8295) 与 MCU (车身控制器) 之间的双向可靠消息通信。^[extracted]

## 整体分层结构

```
上层应用（Application Layer）
        ↓ VHAL / 诊断 / OTA / BLE 等上层服务
        ↓
librpc 公共接口层
        ↓ RpcLib API 入口
消息分发层
        ↓ 基于 GID 的消息路由分发
协议编解码层
        ↓ 帧组装与解析 (768B固定帧长)
数据映射层
        ↓ GrpId 映射 & CAN 信号定义
传输控制层
        ↓ 滑动窗口、ACK/重传
IO 抽象层
        ↓ SPI / TCC IPC 硬件驱动
        ↓
MCU（车身控制器，SPI Slave）
```

### 核心分层

| 层级 | 文件 | 职责 |
|------|------|------|
| 应用层 | - | VHAL、诊断、OTA、BLE等服务 |
| 接口层 | librpc.h | 对外 API 封装 |
| 分发层 | rpc_dispatcher.c | 维护链表，按 GID 路由 |
| 协议层 | rpc_proto.c | 帧结构定义，消息映射表 |
| 数据层 | librpc-data | GrpId 到 buffer 映射，CAN监控 |
| 传输层 | RpcTp_Lib.c | 拥塞控制、队列管理、状态机 |
| IO 层 | RpcIo_Lib.c | SPI、IPC 硬件驱动接口 |
| 硬件层 | rpc_hardware_spi.c | /dev/spidev 设备节点管理 |
| 基础库 | xbase | epoll、定时器、链表、信号量 |

## 消息模型

### 发布（Fire-and-Forget）
```c
RpcLib_Notify(lib, Gid, payload, len);
```
异步通知，不等待响应，发送即忘模式。支持 MCU 主动推送通知到上层注册的监听器。^[extracted]

### 请求响应模式
```c
// 同步请求（阻塞等待）
RpcLib_SyncRequest(lib, Gid, req, req_len, resp, &resp_len, timeout);

// 异步请求（回调获取响应）
RpcLib_AsyncRequest(lib, Gid, req, req_len, handler, user, timeout);
```

### MCU 主动推送订阅
```c
listener = RpcLib_AddNotifyListener(lib, callback, NULL);
RpcLib_AddGid(listener, MESSAGE_BATT_STATE);
RpcLib_AddGid(listener, MESSAGE_GEAR_POSITION);
```
监听器可订阅多个 GID，当 MCU 发送对应 GID 的通知时触发回调。^[extracted]

### 任务类型
分发层支持 4 种任务类型：`NOTIFY`（通知）、`RESPONSE`（响应）、`ASYNC_REQUEST`（异步请求）、`SYNC_REQUEST`（同步请求）。^[extracted]

## 技术特性

- **固定帧长**：每 SPI 帧固定 768 字节，简化 DMA 传输和缓冲区管理^[extracted]
- **全双工**：每次传输同时发送和接收^[extracted]
- **帧标识**：帧头 0xA5，帧尾 0xAD^[extracted]
- **多消息复用**：单帧可包含多个逻辑消息^[extracted]
- **CRC 校验**：每帧包含 2 字节 CRC 校验^[extracted]
- **拥塞控制**：发送队列窗口大小为 2，最大重试 200 次，超时 2000ms^[extracted]
- **线程模型**：主事件循环、接收线程、wakeup 线程、SPI 线程^[extracted]
- **分层解耦**：遵循 AUTOSAR 分层思想，硬件抽象与业务逻辑完全分离^[extracted]
- **多后端支持**：IO 层通过函数指针抽象，可切换 SPI / TCC IPC 等不同物理介质^[extracted]

## 应用场景

消息类别覆盖：[^1]

- **电源管理**：MESSAGE_MPU_POWER_STATE、MESSAGE_BATT_STATE 
- **档位信息**：MESSAGE_GEAR_POSITION
- **软件版本**：MESSAGE_SOFTWAREVER_REQ
- **BLE 钥匙**：BLE 相关消息组^[inferred]
- **诊断 UDS**：MESSAGE_DIAG_* 诊断消息组^[inferred]
- **OTA 升级**：FRAME_FOTA 类型帧，512+5 字节数据^[extracted]
- **CAN 信号透传**：MESSAGE_CAN_* 消息组^[inferred]
- **BAP 协议**：车载应用协议消息组^[extracted]
- **MCU 日志**：FRAME_LOG 类型帧^[extracted]

## 数据流

```
SoC 发送: App → RpcLib_Notify(GID, data) → Dispatcher 打包 → Proto 组帧(768B)
         → Tp 分片+CNT → IO GPIO 握手 → SPI 全双工传输 → MCU

MCU 接收: SPI 中断 → IO RxData → Tp 重组 → Proto 解帧+CRC校验
         → 按 GID 路由到 Dispatcher → 回调上层 NotifyListener
```

## 相关概念

- [[GID 消息寻址]] — 消息地址标识，超过 100 种消息类型
- [[GPIO 握手协议]] — 传输前的就绪协商机制
- [[MCU 车身控制器]] — 通信对端，SPI Slave 设备
- [[RpcT 传输协议]] — 滑动窗口、ACK/重传机制
- [[RpcIo 层]] — 硬件抽象层，支持 SPI/TCC IPC
- [[RpcDataLib 数据映射层]] — GrpId 映射与 CAN 监控