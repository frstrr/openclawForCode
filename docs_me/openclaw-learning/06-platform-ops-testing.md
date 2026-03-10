# 06. 平台、运维与测试

本章目标：具备真实开发所需的“跑、测、看日志、定位问题”能力。

## 1) 平台文档入口

- macOS：[`../../docs/platforms/macos.md`](../../docs/platforms/macos.md)
- iOS：[`../../docs/platforms/ios.md`](../../docs/platforms/ios.md)
- Android：[`../../docs/platforms/android.md`](../../docs/platforms/android.md)
- 平台总览：[`../../docs/platforms/index.md`](../../docs/platforms/index.md)

## 2) 运维/排障文档

- 日志：[`../../docs/logging.md`](../../docs/logging.md)
- Gateway Doctor：[`../../docs/gateway/doctor.md`](../../docs/gateway/doctor.md)
- Gateway 故障排查：[`../../docs/gateway/troubleshooting.md`](../../docs/gateway/troubleshooting.md)
- Channels 排障：[`../../docs/channels/troubleshooting.md`](../../docs/channels/troubleshooting.md)

## 3) 测试与质量门禁

- 测试说明：[`../../docs/reference/test.md`](../../docs/reference/test.md)
- 代码目录中大量 `*.test.ts` 可直接作为行为文档

常用命令：

- `pnpm build`
- `pnpm test`
- `pnpm check`

## 4) 代码对应目录

- 平台 App：`apps/macos/`, `apps/ios/`, `apps/android/`
- 节点能力：`src/node-host/`
- 后台/守护：`src/daemon/`
- 终端交互：`src/terminal/`, `src/tui/`

## 5) 推荐排障顺序

1. 先确认配置和路由是否正确
2. 再确认模型/工具权限与可用性
3. 最后看平台端（设备权限、客户端版本、节点连接）

## 6) 本章完成标准

- 你能独立跑一次 build/test/check
- 你能用日志和 doctor 快速缩小问题范围
