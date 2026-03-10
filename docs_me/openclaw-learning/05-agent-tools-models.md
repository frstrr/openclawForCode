# 05. Agent / 模型 / 工具系统

本章目标：看懂 OpenClaw 的“智能体执行闭环”。

## 1) 先读概念文档

- Agent：[`../../docs/concepts/agent.md`](../../docs/concepts/agent.md)
- Agent loop：[`../../docs/concepts/agent-loop.md`](../../docs/concepts/agent-loop.md)
- 上下文：[`../../docs/concepts/context.md`](../../docs/concepts/context.md)
- 模型：[`../../docs/concepts/models.md`](../../docs/concepts/models.md)
- 模型故障切换：[`../../docs/concepts/model-failover.md`](../../docs/concepts/model-failover.md)
- 工具系统：[`../../docs/tools/index.md`](../../docs/tools/index.md)

## 2) 代码主路径

- Agent 命令入口：[`../../src/commands/agent.ts`](../../src/commands/agent.ts)
- Agent 相关实现：[`../../src/agents/`](../../src/agents/)
- Provider 适配：[`../../src/providers/`](../../src/providers/)
- Memory 系统：[`../../src/memory/`](../../src/memory/)

## 3) 一次执行的大致流程

1. 收到用户输入
2. 选择/解析模型与策略
3. 构建 prompt + 上下文
4. 执行模型推理
5. 如需要调用工具，进入工具执行
6. 回写会话/状态并输出

## 4) 对 C# 开发者最有价值的理解点

- 这不是单次函数调用，而是“状态机 + 事件流”
- 工具调用类似“可插拔命令总线”，不是传统固定 RPC
- 失败处理更依赖策略层（fallback/retry）而不是单点 try-catch

## 5) 模型与工具的边界

- 模型：负责语言推理与决策
- 工具：负责真实世界动作（shell/web/消息/API）
- Agent loop：负责把二者编排为可恢复流程

## 6) 实操建议

1. 从 `agent.ts` 顶层参数开始看
2. 跟到模型选择/fallback 逻辑
3. 跟一次工具调用请求到执行与结果回流

## 7) 本章完成标准

- 你能解释“为什么同样输入在不同模型配置下表现不同”
- 你能定位一次工具失败属于“调度问题、权限问题、还是工具本身问题”
