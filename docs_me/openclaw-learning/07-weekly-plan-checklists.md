# 07. 分周执行计划与里程碑

本章目标：把前面章节变成可执行任务，避免只读不练。

## 第 1 周：建立认知框架

### 任务

- 读完：`00`, `01`, `02`
- 跑通：`pnpm install`, `pnpm build`, `pnpm openclaw --help`
- 跟踪：从 `src/entry.ts` 跟到 `src/cli/program/build-program.ts`

### 输出物

- 你自己的 1 页系统架构图
- 1 页“TS/Node 与 C#/C++ 对照笔记”

### 验收标准

- 你能口头说明命令执行路径与配置加载路径

## 第 2 周：吃透路由、会话、渠道

### 任务

- 读完：`03`, `04`
- 跟踪一条真实消息路径：渠道输入 -> 路由 -> 会话 -> agent
- 对照查看一个渠道文档和其实现

### 输出物

- 一张路由决策表（channel/account/peer -> agent/session）
- 一个“渠道适配差异清单”（至少 5 条）

### 验收标准

- 能解释会话错位为何会表现为“人格被重置”

## 第 3 周：Agent 与工具实战

### 任务

- 读完：`05`, `06`
- 跟一次 agent 执行闭环（含工具调用）
- 跑 `pnpm test` 和 `pnpm check`

### 输出物

- 一份“agent loop 时序图”
- 一份“故障定位流程”（路由/模型/工具/平台分层）

### 验收标准

- 能独立定位一个小问题，并提出修复点

## 第 4 周（可选）：贡献准备

### 任务

- 选择一个小改动：
  - 文案/日志改进
  - 路由规则边界修复
  - 一个测试补全
- 本地完成 build + test + check

### 输出物

- 一次可复现的最小变更说明（含修改文件与验证命令）

## 每日模板（建议）

- 30 分钟：读文档
- 60 分钟：跟代码
- 30 分钟：命令验证/测试
- 15 分钟：写学习笔记

## 附：高价值链接索引

- 学习总入口：[`./README.md`](./README.md)
- 项目 README：[`../../README.md`](../../README.md)
- 架构：[`../../docs/concepts/architecture.md`](../../docs/concepts/architecture.md)
- Gateway：[`../../docs/gateway/index.md`](../../docs/gateway/index.md)
- 路由：[`../../docs/channels/channel-routing.md`](../../docs/channels/channel-routing.md)
- Session：[`../../docs/concepts/session.md`](../../docs/concepts/session.md)
- Tools：[`../../docs/tools/index.md`](../../docs/tools/index.md)
- Testing：[`../../docs/reference/test.md`](../../docs/reference/test.md)
