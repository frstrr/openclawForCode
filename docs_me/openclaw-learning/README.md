# OpenClaw 学习计划（C#/C++ 背景，JS/TS 零基础）

这套文档按“先语言，再框架，再子系统，再实战”的顺序组织，目标是让你可以在 2-4 周内从能读代码到能改核心路径。

## 使用方式

- 第 1 周优先读 `00`、`01`、`02`
- 第 2 周读 `03`、`04`、`05`
- 第 3 周读 `06` 并执行 `07` 的实操清单
- 每天都要“读文档 + 跟代码 + 跑命令”三件事

## 文档导航

1. [00 - JS/TS 速览与语言对比](./00-language-primer.md)
2. [01 - 项目架构总览](./01-architecture-map.md)
3. [02 - 启动链路与运行时](./02-runtime-flow.md)
4. [03 - 路由与会话系统](./03-routing-sessions.md)
5. [04 - 渠道与插件扩展](./04-channels-plugins.md)
6. [05 - Agent/模型/工具系统](./05-agent-tools-models.md)
7. [06 - 平台、运维与测试](./06-platform-ops-testing.md)
8. [07 - 分周执行计划与里程碑](./07-weekly-plan-checklists.md)
9. [08 - 30 个关键源码阅读顺序](./08-key-files-reading-order.md)

## 你要达到的能力

- 能解释：消息从 channel 进入后如何被路由到 agent/session，再回发
- 能定位：配置问题、路由问题、模型/工具问题分别在哪一层排查
- 能改动：在不破坏主链路前提下，完成一个小功能或修复一个小 bug

## 先读的官方资料（全局）

- 项目入口：[`../../README.md`](../../README.md)
- 快速开始：[`../../docs/start/getting-started.md`](../../docs/start/getting-started.md)
- 架构概念：[`../../docs/concepts/architecture.md`](../../docs/concepts/architecture.md)
- Gateway 说明：[`../../docs/gateway/index.md`](../../docs/gateway/index.md)
- Channels 总览：[`../../docs/channels/index.md`](../../docs/channels/index.md)
- Tools 总览：[`../../docs/tools/index.md`](../../docs/tools/index.md)
