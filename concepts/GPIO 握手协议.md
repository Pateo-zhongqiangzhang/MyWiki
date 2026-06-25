---
title: GPIO 握手协议
category: concepts
summary: SPI 传输前的双方就绪协商机制，通过输入/输出 GPIO 电平实现
tags: [通信, 握手, GPIO]
updated: 2026-06-03T15:30:00Z
created: 2026-06-03T15:30:00Z
sources: ["file:Documents/README-architecture.md"]
tier: supporting
base_confidence: 0.67
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

# GPIO 握手协议

SPI 传输前的双方就绪协商机制，确保双方准备好后才开始传输，通过输入/输出 GPIO 电平实现。^[extracted]

## IO 抽象层接口

RpcIo_Type 通过函数指针表实现硬件抽象：^[extracted]

```c
typedef struct {
    SpiXferFn()      // SPI 数据收发
    XferRequestFn()  // 传输请求触发
    GetInReqLvlFn()  // 读取输入 GPIO（MCU 请求信号）
    SetOutReqLvlFn() // 设置输出 GPIO（SoC 请求信号）
} RpcIo_Type;
```

## 握手流程

```
1. SoC 想发送 → SetOutReqLvl(HIGH)
2. MCU 就绪 → InputGPIO 变高
3. 双方同时开始 SPI 全双工传输
4. 传输完毕 → GPIO 复位
```

## GPIO 信号线

| 信号 | 方向 | 作用 |
|------|------|------|
| gpio-output | SoC → MCU | SoC 通知 MCU 有数据要发 |
| gpio-input | MCU → SoC | MCU 就绪指示 |

## 配置示例

```xml
<rpc-device type="spi" device="/dev/spidev2.0"
    gpio-input="288" gpio-output="294"
    spi-mode="SPI_MODE_1" spi-speed="4000000" spi-bits="8" />
```

## 可靠性机制

- **重试次数**：最大 200 次^[extracted]
- **单次超时**：2000ms^[extracted]
- **中断触发**：MCU 拉低 gpio-input 触发 SoC 中断^[inferred]

## MCU 主动发起

当 MCU 要向 SoC 发送消息时：
1. MCU 拉低 gpio-input
2. SoC 检测到中断 → 发起 SPI 读取
3. 一次 768 字节全双工传输^[inferred]

## 相关概念

- [[SPI-RPC 架构]] — 握手协议在其中的上下文
- [[RpcIo 层]] — 硬件抽象层，通过函数指针支持多种后端