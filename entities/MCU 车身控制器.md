---
title: MCU 车身控制器
category: entities
summary: 车身控制器，作为 SPI Slave 与 SoC 通信，负责车身硬件控制
tags: [硬件, MCU, 车身控制]
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
  - target: "[[SPI-RPC 架构]]"
    type: communicate_with
  - target: "[[GID 消息寻址]]"
    type: uses
---

# MCU 车身控制器

Microcontroller Unit（微控制器），作为 SPI Slave 节点与车载 SoC 进行 RPC 通信。^[extracted]

## 职责

负责车身范围内的硬件控制，包括但不限于：
- 车门控制（上锁/解锁）^[inferred]
- 座椅调节^[inferred]
- 空调出风口控制^[inferred]
- 灯光控制^[inferred]
- 窗户/天窗控制^[inferred]
- CAN 总线数据收集^[inferred]

## 通信角色

### SPI Slave 模式
- 响应 SoC 的 Master 控制^[inferred]
- 通过 GPIO 信号线主动发起传输^[inferred]
- 全双工 768 字节帧格式^[extracted]

### 消息模型支持
- **接收**：SoC 的控制指令（电源状态、配置等）^[inferred]
- **推送**：车身状态变化（电池、档位、开关状态等）^[extracted]
- **响应**：同步请求（软件版本查询等）^[extracted]

## 技术规格

- 总线：SPI全双工^[extracted]
- 帧长：固定 768 字节^[extracted]
- 握手：GPIO 输入/输出信号^[extracted]
- CRC：每帧包含校验^[extracted]

## 典型交互

```
SoC: 请求软件版本
MCU: 响应版本号（通过 MESSAGE_SOFTWAREVER_REQ）

SoC: 设置电源状态
MCU: 接收并执行 MESSAGE_MPU_POWER_STATE

MCU: 电池状态变化（主动推送）
SoC: 接收 MESSAGE_BATT_STATE
```

## 相关概念

- [[SPI-RPC 架构]] — 深层架构
- [[GID 消息寻址]] — 消息类型定义
- [[GPIO 握手协议]] — 通信启动机制