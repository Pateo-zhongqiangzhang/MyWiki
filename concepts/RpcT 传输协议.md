---
title: RpcT 传输协议
category: concepts
summary: SPI-RPC 传输层类似 AUTOSAR TP 层的可靠传输协议，实现滑动窗口、重传、超时管理
tags: [传输协议, 可靠传输, 滑动窗口]
updated: 2026-06-03T16:10:00Z
created: 2026-06-03T16:10:00Z
sources: ["file:Documents/architecture.md"]
tier: supporting
base_confidence: 0.58  # 1 source, quality bucket: code-arch
lifecycle: draft
lifecycle_changed: 2026-06-03
provenance:
  extracted: 0.85
  inferred: 0.10
  ambiguous: 0.05
relationships:
  - target: "[[SPI-RPC 架构]]"
    type: implements
  - target: "[[协议帧格式]]"
    type: uses
---

# RpcT 传输协议

传输控制层，实现可靠传输协议，类似 AUTOSAR TP 层的设计。^[extracted]

## 核心机制

### 滑动窗口

- **窗口大小**：2^[extracted]
- **CNT 序号校验** — 检测丢包、重复、乱序^[extracted]
- **状态机**：`IDLE → XFER_PROCESS → RX_DATA_PROCESS`^[extracted]

### 可靠性保障

| 机制 | 参数 | 说明 |
|------|------|------|
| 重传 | 最大 200 次 | 每次超时 2000ms^[extracted] |
| 超时管理 | 最大 2000ms | 单次传输最大等待时间^[extracted] |
| ACK/重传 | 序号匹配确认 | CNT 校验丢包和乱序^[extracted] |

### 故障模式

检测处理多种异常情况：^[extracted]

- 未初始化状态
- 校验失败
- ACK 不匹配
- 序号跳跃（异常跳跃值）
- 超时未响应

## 工作流程

```
1. IDLE: 空闲状态，等待传输请求
2. XFER_PROCESS: 
   - 发送数据包，启动定时器
   - 等待 ACK 响应
   - 窗口内最多 2 个未确认包
3. RX_DATA_PROCESS:
   - 接收端重组分片数据
   - 按 SN 序号校验和排序
   - 统一轮询到上层 Dispatcher
4. 收到 ACK:
   - 移动滑动窗口
   - 继续发送后续数据
5. 超时或失败:
   - 重传未确认包
   - 达到最大重试次数则上报错误
```

## 设计特点

遵循 AUTOSAR TP 层设计思想，适应车载 EMC 环境：^[extracted]

- 分片传输支持大数据块
- 确认重传保证数据完整性
- 序号机制防重复、防乱序
- 窗口流控避免对方缓冲区溢出

## 相关概念

- [[SPI-RPC 架构]] — 传输层在其中位置
- [[协议帧格式]] — CNT/SN 字段定义
- [[xbase]] — 定时器、信号量等基础工具