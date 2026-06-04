# Supermalo

> 基于 [everything-claude-code](https://github.com/affaan-m/everything-claude-code)（MIT）的 agent 体系，经由 [ai-max](https://github.com/zhukunpenglinyutong/ai-max)（MIT）的 CLI 安装器二次开发而来。Supermalo 在此基础上引入 `malo-` 统一前缀与 `/malo:` 斜杠命名空间，适配多插件并存的 Claude Code 环境。

**Claude Code 能力增强，开箱即用。**

本仓库包含生产级 agents（代理）、skills（技能）、hooks（钩子）、commands（命令）、rules（规则）和 MCP 配置，帮助你快速提升 Claude Code 的使用体验。

---

## 快速开始

```bash
# 全局安装
npm install -g supermalo

# 一键安装全部组件
supermalo install -y

# 在 Claude Code 中使用
/malo:malo-auto 你的需求    # 智能分发到最合适的命令
```

CLI 提供交互式界面，按需选择组件：
- **Agents** — 专用子代理（malo-planner、malo-architect、malo-tdd-guide 等）
- **Rules** — 全局准则（malo-security、malo-testing、malo-coding-style 等）
- **Commands** — 斜杠命令（/malo:malo-plan、/malo:malo-tdd、/malo:malo-code-review 等）
- **Skills** — 工作流定义与领域知识

### CLI 命令参考

| 命令 | 说明 | 常用选项 |
|------|------|----------|
| `supermalo` | 交互式主菜单 | — |
| `supermalo install` | 安装全部组件到 `~/.claude/` | `-y` 跳过确认，`-f` / `--force` 强制覆盖 |
| `supermalo update` | 更新已安装的组件（强制覆盖模式） | `-y` 跳过确认 |
| `supermalo uninstall` | 卸载（精准删除记录过的文件） | `-y` 跳过确认 |
| `supermalo list` | 列出各组件的安装状态 | — |
| `supermalo docs` | 打开 GitHub 文档页面 | — |

> **提示**：安装后在 `~/.claude/.supermalo-version` 中会记录每个已复制文件的路径，`uninstall` 仅删除这些记录过的文件，不会影响同一目录下其他插件的文件。

---

## 斜杠命令速查

| 命令 | 用途 | 示例 |
|------|------|------|
| `/malo:malo-auto` | 智能分发，自动匹配最优命令 | `/malo:malo-auto 帮我修这个 bug` |
| `/malo:malo-plan` | 功能规划与实现方案 | `/malo:malo-plan 添加用户登录` |
| `/malo:malo-tdd` | 测试驱动开发 | `/malo:malo-tdd 实现购物车` |
| `/malo:malo-code-review` | 代码质量与安全审查 | `/malo:malo-code-review` |
| `/malo:malo-build-fix` | 修复构建 / 类型错误 | `/malo:malo-build-fix` |
| `/malo:malo-e2e` | 端到端测试生成 | `/malo:malo-e2e 测试注册流程` |
| `/malo:malo-test-coverage` | 测试覆盖率分析 | `/malo:malo-test-coverage` |
| `/malo:malo-refactor-clean` | 代码重构与清理 | `/malo:malo-refactor-clean` |
| `/malo:malo-update-docs` | 更新项目文档 | `/malo:malo-update-docs` |
| `/malo:malo-update-codemaps` | 更新代码架构图 | `/malo:malo-update-codemaps` |

### 推荐工作流

```
1. /malo:malo-plan        → 规划功能
2. /malo:malo-tdd         → 测试驱动实现
3. /malo:malo-code-review → 审查代码
4. /malo:malo-build-fix   → 修复构建问题（如有）
5. git commit             → 提交
```

---

## 核心概念

### Agents（代理）

子代理以有限范围处理委派任务，位于 `~/.claude/agents/`，文件名均以 `malo-` 为前缀：

| Agent | 用途 |
|-------|------|
| malo-planner | 功能规划 |
| malo-architect | 系统架构设计 |
| malo-tdd-guide | 测试驱动开发 |
| malo-code-reviewer | 代码审查 |
| malo-security-reviewer | 安全漏洞分析 |
| malo-build-error-resolver | 构建错误修复 |
| malo-e2e-runner | E2E 测试 |
| malo-refactor-cleaner | 死代码清理 |
| malo-doc-updater | 文档同步 |

### Rules（规则）

强制性准则，位于 `~/.claude/rules/`：

| 规则 | 内容 |
|------|------|
| malo-security | 禁止硬编码密钥、输入验证 |
| malo-coding-style | 不可变性、文件组织 |
| malo-testing | TDD、80% 覆盖率 |
| malo-git-workflow | 提交格式、PR 流程 |
| malo-agents | Agent 编排策略 |
| malo-performance | 模型选择、上下文管理 |
| malo-patterns | API 响应、通用模式 |
| malo-hooks | Hook 配置文档 |

---

## 为何自建 Supermalo

### 1. 上游维护停滞

ai-max 的全部开发集中在 2026 年 1 月 22 日到 2 月 11 日的 20 天内（共 11 个 commit），此后至今已约四个月无新提交。Bug 修复、功能演进和生态适配均难以指望上游响应。

### 2. 与生态插件区分

Claude Code 环境中常同时存在多份开源 skills / agents（如 Superpowers 等）。若自有资产无统一前缀，在 `~/.claude` 目录中极易与第三方短名混淆，排查和卸载都非常困难。Supermalo 采用 `malo-` 前缀 + `/malo:malo-*` 斜杠命名空间，搜索 `malo-` 即可精准定位全部自有资产。

### 3. 个人工作流定制

参考社区优秀实践后，希望长期维护一套贴合自身习惯的 skills / agents / commands，由 `supermalo` CLI 一键安装与更新，不必依赖第三方仓库的更新节奏。

---

## 致谢

Supermalo 的诞生离不开以下开源项目：

- **[everything-claude-code](https://github.com/affaan-m/everything-claude-code)**（MIT）— 提供了 agents、skills、hooks 的核心方法论与初始模式。该项目拥有 2000+ commits、170+ 贡献者，持续每周更新，是 Claude Code 生态的重要基础设施。
- **[ai-max](https://github.com/zhukunpenglinyutong/ai-max)**（MIT）— 在 everything-claude-code 基础上封装了 npm CLI 安装器、中文命令体系和开箱即用的组件结构。Supermalo 的初始代码直接来自 ai-max。

Supermalo 在此致谢上游种子项目。本仓库为独立分支，与上游无官方关联。

---

## 仓库结构

```
supermalo/
├── assets/              # Claude Code 资产集合
│   ├── agents/          # 专用子代理（malo-planner、malo-code-reviewer 等）
│   ├── commands/        # 斜杠命令（malo-plan.md、malo-tdd.md 等）
│   ├── rules/           # 强制性准则（malo-security.md 等）
│   ├── skills/          # 工作流与领域知识（malo-tdd-workflow/ 等）
│   ├── hooks/           # 事件触发器（hooks.json）
│   ├── mcp-configs/     # MCP 服务器配置
│   ├── plugins/         # 社区插件注册表
│   └── examples/        # 项目级 / 用户级 CLAUDE.md 示例
├── src/                 # CLI 安装器源码
├── bin/cli.js           # CLI 入口
└── tests/               # 单元测试
```

所有可安装资产（commands / agents / rules / skills）均以 `malo-` 为前缀，安装后在 `~/.claude` 中可通过搜索 `malo-` 一键定位。

---

## 许可证

MIT — 自由使用，按需修改。
