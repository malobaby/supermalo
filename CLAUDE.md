# CLAUDE.md

本文件为 Claude Code（claude.ai/code）在此仓库中工作时提供指导。

## 项目概述

Supermalo 是一个 CLI 工具兼资产集合，通过 agents、commands、rules、skills、hooks 和 MCP 配置来增强 Claude Code。通过 `npm install -g supermalo && supermalo install` 将这些资产安装到 `~/.claude/` 目录。

基于 [ai-max](https://github.com/zhukunpenglinyutong/ai-max)（MIT）分叉，后者源自 [everything-claude-code](https://github.com/affaan-m/everything-claude-code)（MIT）。所有 Supermalo 资产使用 `malo-` 前缀和 `/malo:` 斜杠命令命名空间，避免与共享 `~/.claude/` 目录中的其他 Claude Code 插件冲突。

## 常用命令

### CLI 命令

```bash
supermalo             # 交互模式（主菜单）
supermalo install     # 安装全部组件到 ~/.claude/
supermalo install -y  # 跳过确认，直接安装
supermalo install -f  # 强制覆盖（跳过备份）
supermalo update      # 更新已安装的组件
supermalo uninstall   # 卸载（精准删除记录过的文件）
supermalo list        # 列出已安装的资产和状态
supermalo docs        # 打开 GitHub 文档页面
```

### NPM Scripts

```bash
npm test              # 运行全部测试（vitest）
npm run test:watch    # 监视模式
npm run test:coverage # 含覆盖率报告
```

无需构建步骤——项目为纯 ESM JavaScript（Node >= 18，`"type": "module"`）。

## 架构

### CLI 层

- [bin/cli.js](bin/cli.js) — 入口文件，基于 `commander`。子命令：`install`、`update`、`uninstall`、`list`、`docs`。无子命令时进入交互模式。
- [src/index.js](src/index.js) — 编排层：`interactiveMode()`、`runInstall()`、`runUpdate()`、`runUninstall()`。安装始终安装全部组件（当前版本不支持部分选择组件）。
- [src/installer.js](src/installer.js) — 文件复制引擎。将资产从包的源目录复制到 `~/.claude/`。覆盖时自动创建 `.backup` 备份文件（除非使用 `--force`）。将每个已安装文件的路径记录到 `~/.claude/.supermalo-version`，以便精确卸载。
- [src/utils.js](src/utils.js) — 路径工具函数（`getClaudeDir()`、`getSourceDir()`）、`COMPONENTS` 组件定义映射、版本文件的读写（`getInstalledVersion()`、`saveInstalledVersion()`）。
- [src/prompts.js](src/prompts.js) — 基于 `inquirer` 的交互式提示（主菜单、确认、组件选择）。

### 资产分类（安装到 `~/.claude/`）

| 组件 | 源目录 | `~/.claude/` 中的目标 | 文件格式 |
|------|--------|----------------------|---------|
| Agents | `assets/agents/` | `agents/` | `.md` 文件，含 YAML frontmatter（`name`、`description`、`tools`、`model`） |
| Rules | `assets/rules/` | `rules/` | `.md` 文件（无 frontmatter） |
| Commands | `assets/commands/` | `commands/malo/` | `.md` 文件，含 YAML frontmatter（`description`）。隔离在 `malo/` 子目录以实现 `/malo:malo-*` 命名空间 |
| Skills | `assets/skills/` | `skills/` | 混合格式：部分为独立 `.md` 文件，部分为子目录中的 `SKILL.md` |

### 关键设计决策

1. **Commands 隔离在 `commands/malo/` 中**：与 agents/rules/skills 直接放入共享的 `~/.claude/` 目录不同，commands 拥有自己的 `malo/` 子目录。这使得 `/malo:malo-*` 斜杠命令命名空间成为可能，并防止与其他插件冲突。仓库中所有资产统一存放在 `assets/` 目录下。

2. **精准卸载**：安装器在 `~/.claude/.supermalo-version` 中记录复制的每一个文件。卸载时仅删除这些精确记录的文件——绝不会删除共享同一目录（agents/、rules/、skills/）的其他插件文件。对于没有文件列表记录的旧版本安装，回退到基于源文件匹配的删除方式。

3. **`malo-auto` 智能路由**：`/malo:malo-auto` 命令分析用户输入中的关键词，按优先级系统路由到最合适的命令（P0=构建错误 → P4=规划）。

### 测试

- [tests/utils.test.js](tests/utils.test.js) — 测试路径工具函数、COMPONENTS 结构、版本文件读写（mock fs）
- [tests/installer.test.js](tests/installer.test.js) — 测试安装/卸载的边界情况（空组件列表、非法组件名、不存在的目录、共享目录安全性）
- 测试框架：vitest + v8 覆盖率，配置见 [vitest.config.js](vitest.config.js)

编写新测试时的注意事项：mock 掉 `ora` 和 `chalk`（避免 spinner/彩色输出干扰测试），使用 `fs-extra` 创建临时目录，在 `afterEach` 中清理。

### Hooks（面向安装目标，非本仓库）

[assets/hooks/hooks.json](assets/hooks/hooks.json) 定义了 Claude Code 的钩子：
- **PreToolUse**：禁止在 tmux 外启动 dev server、提醒长时间命令使用 tmux、`git push` 前暂停确认、阻止随意创建 `.md` 文件
- **PostToolUse**：打印 PR URL、自动 prettier 格式化、TypeScript 检查、警告 `console.log`
- **Stop**：会话结束时最终审计修改文件中的 `console.log`
