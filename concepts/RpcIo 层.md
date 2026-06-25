---
title: RpcIo 层
category: concepts
summary: 通过函数指针表实现硬件抽象，支持 SPI 和 TCC IPC 两种物理传输后端
tags: [硬件抽象, IO 层, SPI, IPC]
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
  - target: "[[GPIO 握手协议]]"
    type: uses
  - target: "[[xbase]]"
    type: depends_on
---

# RpcT 层

IO 抽象层，通过函数指针表实现硬件抽象，支持多种物理传输后端。^[extracted]

## 函数指针接口

```c
typedef struct {
    SpiXferFn()      // SPI 数据收发
    XferRequestFn()  // 传输请求触发
    GetInReqLvlFn()  // 读取输入 GPIO（MCU 请求信号）
    SetOutReqLvlFn() // 设置输出 GPIO（SoC 请求信号）
} RpcIo_Type;
```

上层通过 RpcIo_Lib 访问底层，具体实现由后端提供。^[extracted]

## 支持的后端

| 后端类型 | 配置文件 | 设备节点 | 说明 |
|----------|----------|----------|------|
| SPI 模式 | `configs/spi-default.xml` | `/dev/spidev2.0` | 4MHz, MODE_1，标准 SPI |
| TCC IPC | `configs/tccipc-default.xml` | `/dev/tcc_ipc_micom` | Telechips IPC 设备 |

通过 XML 配置文件选择使用的后端。^[extracted]

## SPI 后端实现

### 配置示例

```xml
<rpc-device type="spi" device="/dev/spidev2.0"
    gpio-input="288" gpio-output="294"
    spi-mode="SPI_MODE_1" spi-speed="4000000" spi-bits="8" />
```

- **device**: SPI 设备节点路径
- **gpio-input**: MCU 请求信号的 GPIO 引脚号
- **gpio-output**: SoC 请求信号的 GPIO 引脚号^[extracted]
- **spi-mode**: SPI 模式（MODE_1 为 CPOL=0, CPHA=1）
- **spi-speed**: SPI 时钟速率 4MHz
- **spi-bits**: 每帧数据位宽 8bit

### 驱动操作

通过 Linux spi ioctl (`SPI_IOC_MESSAGE`) 进行全双工传输：^[extracted]

- 管理 `/dev/spidev` 设备节点
- 通过 GPIO 中断实现 Master/Slave 握手信号
- 全双工 SPI Transfer（同时收发）
- 异步完成回调 `XferDoneHandler`

## 设计优势

硬件抽象带来的好处：^[inferred]

- **可移植性**：上层代码不依赖具体硬件实现
- **可扩展性**：新增后端无需修改上层代码
- **可测试性**：可提供 mock 后端用于单元测试
- **多介质支持**：同一套代码跑在 SPI / IPC 等不同总线

## 相关概念

- [[SPI-RPC 架构]] — IO 层在架构中的位置
- [[GPIO 握手协议]] — GPIO 信号线协商机制
- [[xbase]] — 提供事件循环等基础工具