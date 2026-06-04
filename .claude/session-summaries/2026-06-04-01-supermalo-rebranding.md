# 会话摘要：Supermalo Rebranding 完整执行

> **日期**: 2026-06-04  
> **会话编号**: 01  

---

## 📌 问题背景

在 `supermalo/` 已从 ai-max 物理复制、命名规范（[命名规范.md](命名规范.md)）已定稿的前提下，需要：
1. 审计规范文档本身是否有遗漏/不一致
2. 重写 README.md，追加「致谢」与「为何自建 Supermalo」章节
3. 按规范执行全套 rebranding

## ✅ 解决方案

### 1. 命名规范审计
深度审查了所有 34 个资产文件、9 个源码/测试文件，发现 5 处规范问题并全部修正：`AI MAX` 遗漏、`getAimaxDir` 处置模糊、LICENSE 缺项、`project-guidelines-example.md` 无 frontmatter、§11 清单表述歧义。

### 2. README 完整重写
纯中文，包含：快速开始、斜杠命令速查（10 条）、核心概念（Agents/Rules 对照表）、为何自建 Supermalo（上游停滞 4 个月 / 生态区分 / 个人工作流）、致谢（追溯 everything-claude-code → ai-max 脉络）。作者确认为 **malobaby**。

### 3. Rebranding 全量执行
采用 Subagent-Driven Development，分 6 个 Phase 完成：
- **Phase 1**: 34 个资产文件重命名为 `malo-*` 前缀
- **Phase 2**: 7 个源码/配置文件替换品牌（package.json、bin/cli.js、src/*.js、LICENSE）
- **Phase 3**: 全部 agent/skill frontmatter `name:` → `malo-*`，命令文件交叉引用 `/aimax:*` → `/malo:malo-*`
- **Phase 4**: 测试文件修正（函数名、路径、品牌引用）
- **Phase 5**: 新建 NOTICE，更新 examples/
- **Phase 6**: 全局验证 — 零残留搜索 + `npm test` 25/25 通过

## 🔑 关键决策

| 决策点 | 选择 | 原因 |
|--------|------|------|
| 版本号 | 重置到 `0.1.0` | 新起点，supermalo 首个版本 |
| Author | `malobaby` | 用户 GitHub 用户名 |
| `getAimaxDir` 处置 | 重命名为 `getSupermaloDir` | 用户明确要求保留并改名 |
| README 语言 | 仅中文 | 用户偏好 |
| README 策略 | 完整重写 | 旧版全文都是 aimax 引用，增量追加不现实 |
| 执行方式 | Subagent-Driven | 任务多且独立，分批派发子代理效率高 |
| Git commit | 不执行 | 用户手动 commit，仅提供 commit message |
| 预存 bug `checkStatus` | 顺手修复 | 测试引用但函数未导出，导致 3 个测试失败 |

## 📂 代码状态

### 已修改文件（53+ 个）

**重命名**（34 个）：
- `commands/`（10）：`auto.md` → `malo-auto.md`，`plan.md` → `malo-plan.md`，等
- `agents/`（9）：`planner.md` → `malo-planner.md`，等
- `rules/`（8）：`security.md` → `malo-security.md`，等
- `skills/`（7）：`tdd-workflow/` → `malo-tdd-workflow/`，等

**源码修改**（8 个）：
- `package.json` - name、version(0.1.0)、bin、description、author、repository
- `bin/cli.js` - program name、描述、docs URL
- `src/utils.js` - `getAimaxDir`→`getSupermaloDir`、`.aimax-version`→`.supermalo-version`、COMPONENTS.commands
- `src/installer.js` - 版本文件路径、中文注释、新增 `checkStatus` 导出
- `src/index.js` - AI MAX→Supermalo、斜杠命令示例、GitHub URL
- `src/prompts.js` - 横幅、菜单文字
- `LICENSE` - 追加 `Copyright (c) 2026 malobaby`

**Frontmatter + 内容**（18+ 个）：
- 全部 9 个 agent `name:` → `malo-*`
- 全部 7 个 skill `name:` → `malo-*`（含 `project-guidelines-example` 新建 frontmatter）
- `commands/malo-auto.md` - 路由表 + 全部 `/aimax:*` → `/malo:malo-*`
- `commands/malo-e2e.md`、`malo-plan.md`、`malo-tdd.md` - 使用示例中的命令引用
- `rules/malo-agents.md` - 全部 agent 名称
- 其他 command 文件交叉引用

**测试**（2 个）：
- `tests/utils.test.js` - 函数名、路径、COMPONENTS 断言、版本号
- `tests/installer.test.js` - 目录名、注释、版本文件路径

**文档**（4 个）：
- `README.md` - 完整重写
- `NOTICE` - 新建
- `examples/CLAUDE.md` - 命令引用
- `examples/user-CLAUDE.md` - 规则名 + agent 名

**规范修正**（1 个）：
- `命名规范.md` - 审计后修正 5 处

### 新增文件
- `README.md` - Supermalo 完整中文 README
- `NOTICE` - 致谢上游项目
- `docs/superpowers/plans/2026-06-04-supermalo-rebranding.md` - 实施计划

### 未修改
- `hooks/hooks.json` - 无品牌引用
- `mcp-configs/mcp-servers.json` - 无品牌引用
- `plugins/README.md` - 无品牌引用
- `examples/statusline.json` - 无品牌引用
- `vitest.config.js` - 无品牌引用
- `.gitignore` - 无需修改

## ⏭️ 待完成事项

- [ ] 手动 git commit（commit message 已在会话中提供）
- [ ] `npm link` → `supermalo install -y` 端到端验证
- [ ] 本机若曾装 aimax：清理 `~/.claude/commands/aimax/`、`.aimax-version`、无前缀残留 agents/rules/skills
- [ ] 推送到 `https://github.com/malobaby/supermalo`
- [ ] `npm publish`（发布到 npm）
- [ ] （可选）后续增补 `malo-brainstorm` 等自有 skill

## 📝 补充说明

- **验证结果**：`npm test` → 25/25 通过；全文搜索（排除 NOTICE/命名规范/计划文档）→ 零 aimax 残留
- **与之前会话关系**：会话 01（仓库初始化 + 复制）→ 会话 02（命名规范定稿）→ 本会话（审计规范 + 重写 README + 执行全套 rebranding）
- **`package-lock.json`** 在 `npm install` 后已自动更新
- **`node_modules/`** 已在 `.gitignore` 中
- **计划文档** 中保留有旧名称引用（`bin.aimax` 等），属于历史记录正常现象
