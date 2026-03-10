# 04. 渠道与插件扩展

本章目标：理解 OpenClaw 如何支持大量渠道，并且还能继续扩展。

## 1) 渠道层设计思路

- 把不同 IM/聊天平台差异封装到 adapter/plugin
- 上层统一走标准消息结构和回发接口
- 这样 Agent 层基本不关心渠道细节

## 2) 先读哪些文档

- 渠道总览：[`../../docs/channels/index.md`](../../docs/channels/index.md)
- 例 1 Telegram：[`../../docs/channels/telegram.md`](../../docs/channels/telegram.md)
- 例 2 Discord：[`../../docs/channels/discord.md`](../../docs/channels/discord.md)
- 插件机制：[`../../docs/tools/plugin.md`](../../docs/tools/plugin.md)
- 插件清单：[`../../docs/plugins/community.md`](../../docs/plugins/community.md)

## 3) 代码入口与实现层次

- 渠道注册/抽象：[`../../src/channels/registry.ts`](../../src/channels/registry.ts)
- 渠道插件基类与类型：
  - [`../../src/channels/plugins/types.ts`](../../src/channels/plugins/types.ts)
  - [`../../src/channels/plugins/types.core.ts`](../../src/channels/plugins/types.core.ts)
- 发送规范化示例（outbound）：
  - [`../../src/channels/plugins/outbound/telegram.ts`](../../src/channels/plugins/outbound/telegram.ts)
  - [`../../src/channels/plugins/outbound/discord.ts`](../../src/channels/plugins/outbound/discord.ts)

## 4) extensions 目录怎么读

- 每个子目录通常对应一个插件包
- 重点看：初始化入口、配置读取、事件订阅、能力声明
- 先从你熟悉的渠道开始（例如 `extensions/telegram/` 或 `extensions/discord/`）

## 5) 设计优缺点（架构角度）

### 优点

- 渠道隔离清晰，主链路稳定
- 易于并行开发新渠道
- 核心能力可以复用

### 代价

- 适配层数量多，维护成本高
- 同一能力在不同渠道语义不一致（例如 thread/reply/mention）

## 6) 本章完成标准

- 你能解释“同一条 reply 为什么在不同渠道实现不同”
- 你能列出新增渠道至少需要接入的 3 个点
