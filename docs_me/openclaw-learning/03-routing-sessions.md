# 03. 路由与会话系统（核心）

本章目标：掌握 OpenClaw 最核心的“消息归属决策”。

## 1) 为什么这层最重要

- 绝大多数“看起来像模型问题”的故障，根因其实是路由/会话
- 多渠道 + 多 agent + 多会话的系统里，路由正确性决定可用性

## 2) 代码主入口

- 路由决策：[`../../src/routing/resolve-route.ts`](../../src/routing/resolve-route.ts)
- 绑定规则：[`../../src/routing/bindings.ts`](../../src/routing/bindings.ts)
- 会话键：[`../../src/routing/session-key.ts`](../../src/routing/session-key.ts)
- 会话相关：[`../../src/sessions/session-id.ts`](../../src/sessions/session-id.ts)

## 3) 必读文档

- 渠道路由：[`../../docs/channels/channel-routing.md`](../../docs/channels/channel-routing.md)
- 群组/会话：[`../../docs/channels/groups.md`](../../docs/channels/groups.md)
- Session 概念：[`../../docs/concepts/session.md`](../../docs/concepts/session.md)
- Queue 概念：[`../../docs/concepts/queue.md`](../../docs/concepts/queue.md)

## 4) 你要搞懂的决策维度

- channel（来自哪个渠道）
- accountId（哪个账号）
- peer（私聊对象/群对象）
- guild/team/roles（某些渠道额外上下文）
- 默认路由与绑定路由的优先级

## 5) 和 C# 路由系统的类比

- 可类比 ASP.NET Core 的 middleware + endpoint routing
- 不同点是这里路由目标是“agent/session”，不是 HTTP controller

## 6) 常见故障模式

- 以为命中 main，实际命中其他 agent
- 绑定规则覆盖了默认规则
- session key 变化导致上下文“像被重置”

## 7) 建议实操

1. 读 `resolve-route.ts` 的输入结构与输出结构
2. 找一次真实消息处理时 route 对象是怎么生成的
3. 用 CLI 检查绑定关系：`pnpm openclaw agents bindings`

## 8) 本章完成标准

- 你能根据一条消息推断最终 agentId + sessionKey
- 你能解释“为什么看起来像人格/记忆丢失”但其实是会话切换
