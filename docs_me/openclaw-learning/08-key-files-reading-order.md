# 08. 30 个关键源码文件阅读顺序（含看点与时长）

本清单按“先主链路，再核心模块，再扩展模块”的顺序设计。你不需要一次读完，建议分 7-10 天执行。

## 使用方法

- 每天读 3-5 个文件
- 每个文件按同一套路看：输入 -> 关键逻辑 -> 输出 -> 副作用
- 每读完 5 个文件，跑一次命令验证

---

## A. 启动与 CLI 主链路（1-6）

### 1. `src/entry.ts`（30-45 分钟）

- 看点：进程入口、参数预处理、动态加载 CLI
- 目标：知道 `openclaw` 是怎么启动到主程序的

### 2. `src/cli/run-main.ts`（20-30 分钟）

- 看点：CLI 执行主流程
- 目标：知道 runCli 如何承接入口

### 3. `src/cli/program.ts`（5-10 分钟）

- 看点：program 导出与聚合
- 目标：建立文件跳转入口

### 4. `src/cli/program/build-program.ts`（45-60 分钟）

- 看点：子命令注册、命令树装配
- 目标：知道一个 CLI 命令在哪里被挂进去

### 5. `src/cli/program/routes.ts`（20-30 分钟）

- 看点：命令路由映射
- 目标：理解命令路径和 handler 的对应关系

### 6. `src/commands/agent.ts`（90-120 分钟，分两次看）

- 看点：agent 命令主入口、会话持久化、工具/模型协同
- 目标：建立后续所有 agent 逻辑的主锚点

---

## B. Gateway 与配置（7-12）

### 7. `src/gateway/boot.ts`（30-45 分钟）

- 看点：网关启动时 boot 行为、session mapping 快照恢复
- 目标：理解“启动期副作用”如何受控

### 8. `src/commands/gateway-status.ts`（20-30 分钟）

- 看点：gateway 状态命令与探测逻辑
- 目标：知道如何从 CLI 看网关健康

### 9. `src/config/config.ts`（60-90 分钟）

- 看点：配置类型与校验
- 目标：知道配置对象全貌

### 10. `src/config/io.ts`（45-60 分钟）

- 看点：配置读写、快照、安全处理
- 目标：知道配置如何落盘与生效

### 11. `src/config/defaults.ts`（20-30 分钟）

- 看点：默认值策略
- 目标：理解“没配时系统怎么跑”

### 12. `src/config/bindings.ts`（20-30 分钟）

- 看点：绑定配置表示与解析
- 目标：为路由章节打基础

---

## C. 路由与会话（13-18）

### 13. `src/routing/resolve-route.ts`（90-120 分钟，重点）

- 看点：匹配维度、缓存、优先级、最终 route 结构
- 目标：能手推一条消息命中的 route

### 14. `src/routing/session-key.ts`（45-60 分钟）

- 看点：session key 生成规则
- 目标：解释“为何会话会变化”

### 15. `src/routing/bindings.ts`（20-30 分钟）

- 看点：bindings 列表与过滤
- 目标：明确绑定数据源

### 16. `src/sessions/session-id.ts`（15-20 分钟）

- 看点：session id 约束
- 目标：区分 session id 与 session key

### 17. `src/sessions/send-policy.ts`（20-30 分钟）

- 看点：回发策略
- 目标：理解消息是否/如何回传

### 18. `src/sessions/transcript-events.ts`（20-30 分钟）

- 看点：会话事件与转录更新
- 目标：知道上下文记录如何更新

---

## D. 渠道与插件层（19-24）

### 19. `src/channels/registry.ts`（30-45 分钟）

- 看点：渠道注册与查找
- 目标：掌握渠道层总入口

### 20. `src/channels/plugins/types.ts`（20-30 分钟）

- 看点：插件对外类型边界
- 目标：读懂插件接口契约

### 21. `src/channels/plugins/types.core.ts`（20-30 分钟）

- 看点：核心类型拆分
- 目标：知道哪些结构是框架保留字段

### 22. `src/channels/plugins/outbound/telegram.ts`（30-45 分钟）

- 看点：发送适配、payload 规范化
- 目标：看懂一个具体渠道怎么回发

### 23. `src/channels/plugins/outbound/discord.ts`（30-45 分钟）

- 看点：另一种渠道语义差异
- 目标：比较 Telegram/Discord 差异

### 24. `src/channels/typing.ts`（20-30 分钟）

- 看点：typing 生命周期管理
- 目标：理解实时交互体验相关逻辑

---

## E. Agent、模型、Provider、Memory（25-30）

### 25. `src/agents/model-selection.ts`（45-60 分钟）

- 看点：模型选择、provider 归一化、默认策略
- 目标：知道“最终用哪个模型”如何决定

### 26. `src/agents/model-fallback.ts`（30-45 分钟）

- 看点：fallback 链路
- 目标：知道失败后如何自动降级

### 27. `src/providers/github-copilot-models.ts`（20-30 分钟）

- 看点：一个 provider 的模型能力处理
- 目标：形成 provider 适配的模板认知

### 28. `src/providers/github-copilot-auth.ts`（20-30 分钟）

- 看点：provider 认证路径
- 目标：知道认证逻辑通常放在哪

### 29. `src/memory/manager.ts`（60-90 分钟）

- 看点：记忆管理主流程
- 目标：理解长期记忆/检索如何接入 agent

### 30. `src/memory/search-manager.ts`（30-45 分钟）

- 看点：检索编排与策略
- 目标：知道 memory 查询结果如何影响回复

---

## 配套文档跳转（建议边读边对照）

- 架构：[`../../docs/concepts/architecture.md`](../../docs/concepts/architecture.md)
- Agent：[`../../docs/concepts/agent.md`](../../docs/concepts/agent.md)
- Agent loop：[`../../docs/concepts/agent-loop.md`](../../docs/concepts/agent-loop.md)
- Session：[`../../docs/concepts/session.md`](../../docs/concepts/session.md)
- Routing：[`../../docs/channels/channel-routing.md`](../../docs/channels/channel-routing.md)
- Gateway：[`../../docs/gateway/index.md`](../../docs/gateway/index.md)
- Tools：[`../../docs/tools/index.md`](../../docs/tools/index.md)

## 每日最小验证命令

- `pnpm openclaw --help`
- `pnpm openclaw status`
- `pnpm test -- --runInBand`（如果你当天改了代码）

## 读完 30 文件后的能力标志

- 你能独立追踪一条消息的端到端路径
- 你能快速判断问题属于配置/路由/agent/渠道哪一层
- 你能做小规模功能改动并保证回归可验证
