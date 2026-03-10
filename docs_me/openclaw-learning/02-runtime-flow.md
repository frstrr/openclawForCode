# 02. 启动链路与运行时

本章目标：看懂“命令执行后，系统是如何一步步跑起来的”。

## 1) CLI 启动主链路

- 入口脚本：[`../../src/entry.ts`](../../src/entry.ts)
  - 处理环境变量/参数归一化
  - 处理 `--help` / `--version` 快路径
  - 最终动态加载 CLI 主逻辑
- CLI 运行入口：[`../../src/cli/run-main.ts`](../../src/cli/run-main.ts)
- 命令注册：[`../../src/cli/program/build-program.ts`](../../src/cli/program/build-program.ts)

建议配合官方文档：

- 快速上手：[`../../docs/start/quickstart.md`](../../docs/start/quickstart.md)
- 安装与运行：[`../../docs/start/getting-started.md`](../../docs/start/getting-started.md)

## 2) Gateway 启动相关

- 启动辅助：[`../../src/gateway/boot.ts`](../../src/gateway/boot.ts)
- 网关文档入口：[`../../docs/gateway/index.md`](../../docs/gateway/index.md)
- 配置文档：[`../../docs/gateway/configuration.md`](../../docs/gateway/configuration.md)

你可以把 Gateway 理解为“系统总线 + 中央调度器”。

## 3) 配置加载与生效

- 配置结构：[`../../src/config/config.ts`](../../src/config/config.ts)
- 读写与校验：[`../../src/config/io.ts`](../../src/config/io.ts)
- 默认值：[`../../src/config/defaults.ts`](../../src/config/defaults.ts)

对应文档：

- 配置参考：[`../../docs/gateway/configuration-reference.md`](../../docs/gateway/configuration-reference.md)

## 4) 你要重点理解的运行时特征（Node）

- 绝大多数操作非阻塞，靠事件循环推进
- 同时处理多个消息主要靠异步任务调度而非多线程共享对象
- 所有“慢操作”（网络、文件、子进程）都应异步化

## 5) 实操步骤（建议 60-90 分钟）

1. 执行 `pnpm openclaw --help`
2. 执行 `pnpm openclaw gateway --help`
3. 在 IDE 内断点/跟踪 `src/entry.ts` 到 `build-program.ts`
4. 写下你看到的“参数 -> 命令 -> 执行函数”映射

## 6) 本章完成标准

- 你能解释一条命令从入口到具体 handler 的调用路径
- 你能找到配置是在哪里加载、验证、合并默认值的
