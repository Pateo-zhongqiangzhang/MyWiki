---
title: GID 消息寻址
category: concepts
summary: SoC 与 MCU 之间消息的地址标识系统，消息映射表定义了超过 100 种消息类型
tags: [通信, 消息, 寻址]
updated: 2026-06-03T16:10:00Z
created: 2026-06-03T15:30:00Z
sources: ["file:Documents/README-architecture.md", "file:Documents/architecture.md"]
tier: supporting
base_confidence: 0.75  # 2 sources, quality bucket: code-arch
lifecycle: draft
lifecycle_changed: 2026-06-03
provenance:
  extracted: 0.90
  inferred: 0.05
  ambiguous: 0.05
relationships:
  - target: "[[SPI-RPC 架构]]"
    type: used_by
---

# GID 消息寻址

GID（Group ID）是 SoC 与 MCU 之间消息路由的核心地址标识。^[extracted]

## 设计目的

不同车载功能模块通过不同的 GID 区分，上层可以订阅感兴趣的 GID 来接收特定类型的数据。^[inferred]

## 消息映射表

`rpc_message_type_infos[]` 定义了每种消息的：^[extracted]

- 消息类型枚举 → 2字节协议 ID
- 对应的 GrpId（与 RpcDataLib 数据层对接）
- 发送/接收方向标志 (`SEND_NOTIFY`/`RECV_NOTIFY`/`SEND_REQUEST`)
- 自定义的 `set_parser` / `recv_parser` 回调

### 主要消息类型

| 功能类别 | 典型 GID | 用途 |
|----------|----------|------|
| 电源管理 | `MESSAGE_MPU_POWER_STATE` | SoC 电源状态 |
| 电池状态 | `MESSAGE_BATT_STATE` | 电池信息 |
| 档位信息 | `MESSAGE_GEAR_POSITION` | 当前档位 |
| 软件版本 | `MESSAGE_SOFTWAREVER_REQ` | MCU/SOC 版本信息 |
| CAN 数据 | `MESSAGE_CAN_*` | CAN 信号透传 |
| 诊断 | `MESSAGE_DIAG_*` | UDS 诊断数据 |
| BLE | BLE 相关消息组 | 数字钥匙功能 |
| OTA | FRAME_FOTA 类型 | 升级数据传输 |
| 日志 | FRAME_LOG 类型 | MCU 日志采集 |
| BAP | 车载应用协议 | 车载应用通信 |

## 使用模式

### 订阅模式
```c
listener = RpcLib_AddNotifyListener(lib, callback, NULL);
RpcLib_AddGid(listener, MESSAGE_BATT_STATE);
RpcLib_AddGid(listener, MESSAGE_GEAR_POSITION);
```

### 发布与请求
```c
// 发布
RpcLib_Notify(lib, GID, data, len);

// 请求
RpcLib_SyncRequest(lib, GID, req, req_len, resp, &resp_len, timeout);
```

## 特点

- **唯一性**：每个 GID 对应特定的功能^[inferred]
- **可订阅**：同一监听器可订阅多个 GID^[extracted]
- **双向**：既可响应 MCU 发送，也可主动请求^[extracted]

## 相关概念

- [[SPI-RPC 架构]] — GID 在其中的地位
- [[MCU 车身控制器]] — GID 的发送者/接收者