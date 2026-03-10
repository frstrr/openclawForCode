# OpenClaw 本地使用与排障指南（基于本次会话）

这份文档用于后续代码变更后的快速回归和日常运维。

## 1. 环境基线

- Node.js 必须 `>= 22.12.0`（建议 `22.x`）
- 包管理器使用 `pnpm`
- 仓库根目录：`D:\work\MyGitProject\openclawForCode`

常见错误：

- 在 `C:\Users\gosky` 执行 `pnpm openclaw ...` 会报：
  `ERR_PNPM_NO_IMPORTER_MANIFEST_FOUND`
- 原因：当前目录没有 `package.json`

## 2. 启动前最小配置

首次启动网关前，至少设置：

```bash
pnpm openclaw config set gateway.mode local
```

然后启动：

```bash
pnpm openclaw gateway run --force
```

若尚未完整配置，也可临时启动：

```bash
pnpm openclaw gateway run --force --allow-unconfigured
```

## 3. 常用运行命令

```bash
pnpm install
pnpm build
pnpm dev
pnpm openclaw health
pnpm openclaw status
pnpm openclaw dashboard --no-open
```

说明：

- `dashboard --no-open` 会输出带 token 的本地控制台 URL
- `config get gateway.auth.token` 可能显示脱敏值，这是正常的

## 4. `pnpm openclaw` 与 `openclaw` 的区别

`pnpm openclaw ...`：

- 使用当前仓库脚本执行
- 必须在仓库目录运行
- 适合开发调试

`openclaw ...`：

- 使用全局命令
- 可在任意目录运行
- 适合日常运维

如果安装的是发布版：

```bash
npm install -g openclaw
```

如果希望全局命令指向本地源码：

```bash
cd D:\work\MyGitProject\openclawForCode
npm link
```

## 5. 改源码后是否要重装

一般只需要：

```bash
pnpm build
```

何时需要重新安装/链接：

- 改了依赖（`package.json`） -> 先 `pnpm install`
- 改了 `bin`/入口映射 -> 需要重新 `npm link` 或重新全局安装
- 切换“本地源码版”和“npm发布版” -> 需要重新安装对应来源

## 5.1 install 之后的关键认知（避免误判）

`install` 本身通常不会“重置 main agent 身份”，但会触发下面这些看起来像“变新了”的情况：

- **路由还在指向其他 agent**（最常见）：例如 `vision <- feishu:main`
- **消息落到了另一个 workspace**：该 workspace 的 `AGENTS.md/IDENTITY.md` 与 main 不同
- **你混用了命令入口**：`pnpm openclaw` 与全局 `openclaw` 可能连接到不同实例/端口

必须先做的三步真值检查：

```bash
pnpm openclaw agents bindings
pnpm openclaw agents list --json
pnpm openclaw dashboard --no-open
```

如果你在用全局命令，也做同样检查：

```bash
openclaw agents bindings
openclaw agents list --json
openclaw dashboard --no-open
```

目标是确认：

- 当前消息到底命中了哪个 agent
- 当前控制台到底连的是哪个 gateway（token/端口）
- `openclaw` 与 `pnpm openclaw` 是否在看同一个配置实例

## 6. 配置文件与实际生效链路

主配置文件：

- `C:\Users\gosky\.openclaw\openclaw.json`

关键字段：

- 默认模型：`agents.defaults.model.primary`
- agent 路由绑定：`bindings`
- 网关：`gateway.*`

注意：

- `~/.openclaw/agents/<id>/agent/models.json` 是模型目录/能力快照，不是“当前选中模型”开关
- 默认 `main` agent 的身份文件来自 workspace：
  - `~/.openclaw/workspace/AGENTS.md`
  - `~/.openclaw/workspace/IDENTITY.md`

## 7. Agent 模型与图片输入

本次配置中：

- `bailian/glm-5` 仅支持 `text`
- `bailian/qwen3.5-plus` 支持 `text + image`

因此报错：

`Cannot read "image.png" (this model does not support image input)`

通常不是程序异常，而是当前 agent 模型不支持图片。

## 8. 路由绑定（Feishu -> Agent）

查看绑定：

```bash
pnpm openclaw agents bindings
```

将飞书主账号绑定到 vision：

```bash
pnpm openclaw agents bind --agent vision --bind feishu:main
```

移除该绑定：

```bash
pnpm openclaw agents unbind --agent vision --bind feishu:main
```

改回 main：

```bash
pnpm openclaw agents bind --agent main --bind feishu:main
```

是否需要重启网关：

- 通常不需要（配置热加载）
- 但建议改完后发一条新消息验证路由

## 9. “看起来像被重置” 的典型原因

现象：

- 你觉得 main 被初始化了

常见根因：

- 实际命中了另一个 agent（例如 `vision <- feishu:main` 还在）
- 新 agent 用的是新 workspace（人格/记忆文件不同），表现像“新角色”
- install 后首次消息若进入新 session，也会给人“刚初始化”的体感，但不等于 main 文件被覆盖

排查顺序：

```bash
pnpm openclaw agents bindings
pnpm openclaw agents list --json
pnpm openclaw config get agents.defaults.model --json
pnpm openclaw dashboard --no-open
```

## 10. 控制台显示模型与实际模型不一致时

已知前端状态问题：

- UI 可能显示旧的 dirty form 值
- 实际网关配置已更新

建议：

1. `Ctrl+F5` 强刷控制台
2. 点击 `Reload Config`
3. 必要时清理站点 localStorage 后重开
4. 用 CLI 作为真值：`agents list --json`

## 11. 推荐长期策略

- `main` 用稳定文本模型（如 `glm-5`）
- 单独建 `vision` agent 处理图片
- 通过 `bindings` 按渠道/账号切分
- 每次改路由后用新消息回归，不只看 UI

## 12. 安全注意事项

- `~/.openclaw/openclaw.json` 里包含敏感信息（API Key、token）
- 不要把真实配置提交到 git、截图外发、贴到公开渠道
- 如需分享配置，请先脱敏

## 13. install 后快速回归清单（推荐每次执行）

```bash
openclaw --version
pnpm openclaw --version
openclaw config file
pnpm openclaw config file
pnpm openclaw health
pnpm openclaw agents bindings
```

判定标准：

- 两个 `--version` 一致（或你明确知道为何不一致）
- 两个 `config file` 指向同一个配置文件
- `health` 正常
- 绑定关系符合预期（例如不再是 `vision <- feishu:main`）
