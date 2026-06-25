---
title: librpc
category: entities
summary: 车载 SPI-RPC 通信的主体库，分层架构包括 API 分发/协议/数据/传输/IO 层，支持 SPI/TCC IPC 后端
tags: [库, librpc, RPC]
updated: 2026-06-03T16:10:00Z
created: 2026-06-03T15:30:00Z
sources: ["file:Documents/README-architecture.md", "file:Documents/architecture.md"]
tier: supporting
base_confidence: 0.83  # 2 sources, quality bucket: code-arch
lifecycle: draft
lifecycle_changed: 2026-06-03
provenance:
  extracted: 0.85
  inferred: 0.10
  ambiguous: 0.05
relationships:
  - target: "[[SPI-RPC 架构]]"
    type: implements
  - target: "[[xbase]]"
    type: depends_on
---

# librpc

车载 SPI-RPC 通信的主体库，目录位于 `vehicle/cluster-rpc/`。^[extracted]

## 核心组件

| 组件 | 文件 | 职责 |
|------|------|------|
| 公共接口 | include/librpc.h | 对外 API 封装[^1] |
| 消息分发 | src/rpc_dispatcher.c | 维护链表，按 GID 路由收到的消息到已注册的 Handler |
| 协议处理 | src/rpc_proto.c | 帧组装与解析，消息映射表定义 |
| 传输控制 | src/RpcTp_Lib.c | 滑动窗口、ACK/重传、超时管理、状态机 |
| 硬件接口 | src/RpcIo_Lib.c | 函数指针表抽象，支持 SPI / TCC IPC 后端 |
| SPI 驱动 | src/rpc_hardware_spi.c | 管理 /dev/spidev 设备节点，GPIO 中断处理 |

## 初始化方式

### 1. 代码配置
```c
RpcLibConfigureType config = { ... };
RpcLibType *lib = RpcLib_Init(&config);
```

### 2. XML 配置（推荐）
```c
RpcLibType *lib = RpcLib_Init2("/vendor/etc/spi-default.xml", 256);
```

XML 配置文件包含 SPI 参数、GPIO 引脚号等硬件配置。^[extracted]

## 启动流程

```c
// 初始化
RpcLibType *lib = RpcLib_Init2(config_path, 256);

// 启动后台线程
RpcLib_Start(lib);

// ... 业务逻辑 ...

// 释放资源
RpcLib_Finalize(lib);
```

启动后台线程包括：主事件循环、接收线程、wakeup 线程。^[extracted]

## API 汇总

| 功能 | API |
|------|-----|
| 消息发布 | `RpcLib_Notify()` |
| 同步请求 | `RpcLib_SyncRequest()` |
| 异步请求 | `RpcLib_AsyncRequest()` |
| 订阅通知 | `RpcLib_AddNotifyListener()` |
| 添加请求监听 | `RpcLib_AddRequestListener()` |
| 发送响应 | `RpcLib_Response()` |
| 握手状态监听 | `RpcLib_SetHandShakeStatusListener()` |

## 队列管理

- 发送队列窗口大小为 2^[extracted]
- 超时重试：最大 200 次，每次 2000ms^[extracted]

## 相关概念

- [[SPI-RPC 架构]] — 架构设计，分层详解
- [[xbase]] — 基础设施
- [[RpcT 传输协议]] — 传输控制层实现
- [[RpcIo 层]] — 硬件抽象层
- [[RpcDataLib 数据映射层]] — 数据映射层
- [[协议帧格式]] — 帧结构和消息类型

[^1]: 上层业务只需包含 librpc.h 即可。