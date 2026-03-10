# 01. 项目架构总览

本章目标：先把 OpenClaw 当成一个“控制平面系统”看懂，而不是把它当成单一机器人脚本。

## 1) 系统角色

- Gateway：控制平面，负责连接、协议、路由、认证、配置、状态
- Agent：执行智能体循环，调用模型和工具
- Channels：渠道适配层（Telegram/Discord/Slack/Signal/...）
- Apps/Nodes：macOS/iOS/Android 等设备侧能力接入
- CLI：运维与开发入口

建议先看官方概念文档：

- 架构：[`../../docs/concepts/architecture.md`](../../docs/concepts/architecture.md)
- Gateway：[`../../docs/gateway/index.md`](../../docs/gateway/index.md)
- Channels：[`../../docs/channels/index.md`](../../docs/channels/index.md)
- Agent：[`../../docs/concepts/agent.md`](../../docs/concepts/agent.md)

## 2) 仓库结构（你最常走的目录）

- `src/`：核心 TypeScript 代码
- `extensions/`：插件/扩展包（渠道与能力扩展）
- `apps/`：macOS/iOS/Android 客户端
- `docs/`：官方文档
- `scripts/`：构建与工程脚本

## 3) src 目录功能地图

- CLI：`src/cli/`, `src/commands/`
- Gateway：`src/gateway/`
- Routing/Session：`src/routing/`, `src/sessions/`
- Agent：`src/agents/`
- Channels：`src/channels/` + `src/telegram/` `src/discord/` `src/slack/` ...
- Config：`src/config/`
- Provider/Memory：`src/providers/`, `src/memory/`

## 4) 最小主链路（先记住这个）

1. 渠道收到消息
2. 规范化消息结构
3. 路由层决定 agent + session
4. agent 执行（模型 + 工具）
5. 结果通过渠道适配器回发

## 5) 建议先读的关键文件

- 程序入口：[`../../src/entry.ts`](../../src/entry.ts)
- CLI 主程序构建：[`../../src/cli/program/build-program.ts`](../../src/cli/program/build-program.ts)
- 路由决策：[`../../src/routing/resolve-route.ts`](../../src/routing/resolve-route.ts)
- agent 命令入口：[`../../src/commands/agent.ts`](../../src/commands/agent.ts)
- 配置定义：[`../../src/config/config.ts`](../../src/config/config.ts)

## 6) 这章完成标准

- 你能说清“控制平面”和“业务执行平面”分别在哪
- 你能在目录层面定位一个问题大概属于哪一层
