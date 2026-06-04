# Supermalo Rebranding 实施计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 将 supermalo 仓库中全部 ai-max 品牌引用替换为 supermalo/malo 品牌，按 `命名规范.md` §11 检查清单执行。

**Architecture:** 分 6 个阶段执行。Phase 1 重命名 34 个资产文件（必须最先完成）；Phase 2 修改 Node 源码配置；Phase 3 更新所有 frontmatter 与交叉引用；Phase 4 修正测试文件；Phase 5 补全文档与新建 NOTICE；Phase 6 验证。

**Tech Stack:** Node.js, PowerShell (文件重命名), Vitest

**仓库根路径:** `D:\su_workspace\cursor\zzz_vibe\supermalo`

---

## Phase 1: 资产文件重命名（必须最先完成）

### Task 1.1: 重命名 commands/ 目录（10 个文件）

- [ ] **Step 1: 批量重命名**

```powershell
cd D:\su_workspace\cursor\zzz_vibe\supermalo\commands
Rename-Item "auto.md" "malo-auto.md"
Rename-Item "plan.md" "malo-plan.md"
Rename-Item "tdd.md" "malo-tdd.md"
Rename-Item "e2e.md" "malo-e2e.md"
Rename-Item "code-review.md" "malo-code-review.md"
Rename-Item "build-fix.md" "malo-build-fix.md"
Rename-Item "test-coverage.md" "malo-test-coverage.md"
Rename-Item "refactor-clean.md" "malo-refactor-clean.md"
Rename-Item "update-docs.md" "malo-update-docs.md"
Rename-Item "update-codemaps.md" "malo-update-codemaps.md"
```

- [ ] **Step 2: 验证重命名**

```powershell
Get-ChildItem D:\su_workspace\cursor\zzz_vibe\supermalo\commands
```

Expected: 全部 10 个文件以 `malo-` 开头，无 `auto.md`、`plan.md` 等旧名残留。

### Task 1.2: 重命名 agents/ 目录（9 个文件）

- [ ] **Step 1: 批量重命名**

```powershell
cd D:\su_workspace\cursor\zzz_vibe\supermalo\agents
Rename-Item "planner.md" "malo-planner.md"
Rename-Item "architect.md" "malo-architect.md"
Rename-Item "tdd-guide.md" "malo-tdd-guide.md"
Rename-Item "code-reviewer.md" "malo-code-reviewer.md"
Rename-Item "security-reviewer.md" "malo-security-reviewer.md"
Rename-Item "build-error-resolver.md" "malo-build-error-resolver.md"
Rename-Item "e2e-runner.md" "malo-e2e-runner.md"
Rename-Item "refactor-cleaner.md" "malo-refactor-cleaner.md"
Rename-Item "doc-updater.md" "malo-doc-updater.md"
```

- [ ] **Step 2: 验证**

```powershell
Get-ChildItem D:\su_workspace\cursor\zzz_vibe\supermalo\agents
```

### Task 1.3: 重命名 rules/ 目录（8 个文件）

- [ ] **Step 1: 批量重命名**

```powershell
cd D:\su_workspace\cursor\zzz_vibe\supermalo\rules
Rename-Item "agents.md" "malo-agents.md"
Rename-Item "coding-style.md" "malo-coding-style.md"
Rename-Item "git-workflow.md" "malo-git-workflow.md"
Rename-Item "hooks.md" "malo-hooks.md"
Rename-Item "patterns.md" "malo-patterns.md"
Rename-Item "performance.md" "malo-performance.md"
Rename-Item "security.md" "malo-security.md"
Rename-Item "testing.md" "malo-testing.md"
```

- [ ] **Step 2: 验证**

```powershell
Get-ChildItem D:\su_workspace\cursor\zzz_vibe\supermalo\rules
```

### Task 1.4: 重命名 skills/ 目录（7 个条目：2 个目录型 + 5 个单文件型）

- [ ] **Step 1: 重命名目录型 skills**

```powershell
cd D:\su_workspace\cursor\zzz_vibe\supermalo\skills
Rename-Item "tdd-workflow" "malo-tdd-workflow"
Rename-Item "security-review" "malo-security-review"
```

- [ ] **Step 2: 重命名单文件型 skills**

```powershell
Rename-Item "coding-standards.md" "malo-coding-standards.md"
Rename-Item "backend-patterns.md" "malo-backend-patterns.md"
Rename-Item "frontend-patterns.md" "malo-frontend-patterns.md"
Rename-Item "clickhouse-io.md" "malo-clickhouse-io.md"
Rename-Item "project-guidelines-example.md" "malo-project-guidelines-example.md"
```

- [ ] **Step 3: 验证**

```powershell
Get-ChildItem D:\su_workspace\cursor\zzz_vibe\supermalo\skills -Recurse
```

Expected: 目录 `malo-tdd-workflow/`、`malo-security-review/`（内含 `SKILL.md`），5 个 `malo-*.md` 单文件。

---

## Phase 2: Node 源码与配置文件

### Task 2.1: 修改 `package.json`

**File:** `D:\su_workspace\cursor\zzz_vibe\supermalo\package.json`

- [ ] **Step 1: 替换 name、description、bin、author、repository**

```json
{
  "name": "supermalo",
  "version": "0.1.0",
  "description": "Supermalo - Claude Code 能力增强，开箱即用",
  "main": "src/index.js",
  "bin": {
    "supermalo": "./bin/cli.js"
  },
  ...
  "author": "malobaby",
  "license": "MIT",
  "repository": {
    "type": "git",
    "url": "https://github.com/malobaby/supermalo"
  },
```

共 5 处变更：`name`、`version`、`description`、`bin.aimax`→`bin.supermalo`、`author`、`repository.url`。

- [ ] **Step 2: 验证**

```powershell
cd D:\su_workspace\cursor\zzz_vibe\supermalo
node -e "const pkg = require('./package.json'); console.log(pkg.name, pkg.version, pkg.bin)"
```

Expected: `supermalo 0.1.0 { supermalo: './bin/cli.js' }`

### Task 2.2: 修改 `src/utils.js`

**File:** `D:\su_workspace\cursor\zzz_vibe\supermalo\src\utils.js`

- [ ] **Step 1: 替换 `getAimaxDir` → `getSupermaloDir`（函数名与内部路径）**

```javascript
// 修改前（约 L16-18）:
export function getAimaxDir() {
  return path.join(getClaudeDir(), 'aimax');
}

// 修改后:
export function getSupermaloDir() {
  return path.join(getClaudeDir(), 'supermalo');
}
```

- [ ] **Step 2: 替换 `getVersionFilePath` 中的 `.aimax-version` → `.supermalo-version`**

```javascript
// 修改前（约 L31）:
return path.join(getClaudeDir(), '.aimax-version');

// 修改后:
return path.join(getClaudeDir(), '.supermalo-version');
```

- [ ] **Step 3: 替换 `COMPONENTS.commands` 的 name、description、target**

```javascript
// 修改前（约 L93-98）:
commands: {
    name: 'aimax 斜杠指令',
    description: '斜杠命令（/aimax:plan, /aimax:tdd, /aimax:code-review 等）',
    source: 'commands',
    target: 'commands/aimax',
    pattern: '*.md'
},

// 修改后:
commands: {
    name: 'Supermalo 斜杠指令',
    description: '斜杠命令（/malo:malo-plan, /malo:malo-tdd, /malo:malo-code-review 等）',
    source: 'commands',
    target: 'commands/malo',
    pattern: '*.md'
},
```

共 3 处 `aimax` → `malo` + 1 处 `aimax` → `Supermalo`（组件显示名）。

- [ ] **Step 4: 全文验证**

```powershell
Select-String -Path "D:\su_workspace\cursor\zzz_vibe\supermalo\src\utils.js" -Pattern "aimax|AI MAX"
```

Expected: 无任何匹配。

### Task 2.3: 修改 `src/installer.js`

**File:** `D:\su_workspace\cursor\zzz_vibe\supermalo\src\installer.js`

- [ ] **Step 1: 替换所有注释和字符串中的 `AI MAX`**

找到以下位置并替换：
- `~/清理 AI MAX 相关的空目录/` → `清理 Supermalo 相关的空目录`（约 L265）
- `~/只有 commands/aimax 目录可以整体删除（这是 AI MAX 独占的）/` → `只有 commands/malo 目录可以整体删除（这是 Supermalo 独占的）`（约 L170-171）
- `~/对于 commands/aimax，如果目录为空则删除/` → `对于 commands/malo，如果目录为空则删除`（约 L274）

- [ ] **Step 2: 替换版本文件引用**

```javascript
// 修改前（约 L197）:
const versionFile = path.join(claudeDir, '.aimax-version');

// 修改后:
const versionFile = path.join(claudeDir, '.supermalo-version');
```

- [ ] **Step 3: 全文验证**

```powershell
Select-String -Path "D:\su_workspace\cursor\zzz_vibe\supermalo\src\installer.js" -Pattern "aimax|AI MAX"
```

Expected: 无任何匹配。

### Task 2.4: 修改 `src/index.js`

**File:** `D:\su_workspace\cursor\zzz_vibe\supermalo\src\index.js`

- [ ] **Step 1: 替换所有 `AI MAX` → `Supermalo`**

共需替换约 5 处（第 50、51、57、91、128 行附近）：
- `'将要安装 AI MAX 的所有组件：'` → `'将要安装 Supermalo 的所有组件：'`
- `'确认安装 AI MAX？'` → `'确认安装 Supermalo？'`
- `'确认更新 AI MAX？'` → `'确认更新 Supermalo？'`
- `https://github.com/zhukunpenglinyutong/ai-max` → `https://github.com/malobaby/supermalo`

- [ ] **Step 2: 替换 `/aimax:` → `/malo:malo-`**

```javascript
// 修改前（约 L70）:
console.log(chalk.gray('  2. 使用 /aimax:plan, /aimax:tdd, /aimax:code-review 等命令'));

// 修改后:
console.log(chalk.gray('  2. 使用 /malo:malo-plan, /malo:malo-tdd, /malo:malo-code-review 等命令'));
```

- [ ] **Step 3: 全文验证**

```powershell
Select-String -Path "D:\su_workspace\cursor\zzz_vibe\supermalo\src\index.js" -Pattern "aimax|AI MAX|/aimax:"
```

Expected: 无任何匹配。

### Task 2.5: 修改 `src/prompts.js`

**File:** `D:\su_workspace\cursor\zzz_vibe\supermalo\src\prompts.js`

- [ ] **Step 1: 替换所有 `AI MAX` → `Supermalo`**

共需替换 5 处：
- 横幅标题 `AI MAX（v${version}）` → `Supermalo（v${version}）`（约 L13）
- `'安装 AI MAX'` → `'安装 Supermalo'`（约 L89）
- `'更新 AI MAX'` → `'更新 Supermalo'`（约 L90）
- `'卸载 AI MAX'` → `'卸载 Supermalo'`（约 L91）
- `'确定要卸载 AI MAX 吗？'` → `'确定要卸载 Supermalo 吗？'`（约 L74）

- [ ] **Step 2: 全文验证**

```powershell
Select-String -Path "D:\su_workspace\cursor\zzz_vibe\supermalo\src\prompts.js" -Pattern "AI MAX"
```

Expected: 无任何匹配。

### Task 2.6: 修改 `bin/cli.js`

**File:** `D:\su_workspace\cursor\zzz_vibe\supermalo\bin\cli.js`

- [ ] **Step 1: 替换 program name、description 和命令描述**

```javascript
// 修改前（约 L11-12）:
program
  .name('aimax')
  .description('AI MAX - Claude Code 能力增强，开箱即用')

// 修改后:
program
  .name('supermalo')
  .description('Supermalo - Claude Code 能力增强，开箱即用')
```

- [ ] **Step 2: 替换各子命令描述中的 `AI MAX`**

- `'安装 AI MAX'` → `'安装 Supermalo'`（约 L29）
- `'更新 AI MAX'` → `'更新 Supermalo'`（约 L48）
- `'卸载 AI MAX'` → `'卸载 Supermalo'`（约 L62）

- [ ] **Step 3: 替换 docs 命令的 URL**

```javascript
// 修改前（约 L92）:
const url = 'https://github.com/zhukunpenglinyutong/ai-max';

// 修改后:
const url = 'https://github.com/malobaby/supermalo';
```

- [ ] **Step 4: 全文验证**

```powershell
Select-String -Path "D:\su_workspace\cursor\zzz_vibe\supermalo\bin\cli.js" -Pattern "aimax|AI MAX"
```

Expected: 无任何匹配。

### Task 2.7: 修改 `LICENSE`

**File:** `D:\su_workspace\cursor\zzz_vibe\supermalo\LICENSE`

- [ ] **Step 1: 追加版权行**

```text
// 修改前:
Copyright (c) 2026 everything-claude-code
Copyright (c) 2026 朱昆鹏

// 修改后:
Copyright (c) 2026 everything-claude-code
Copyright (c) 2026 朱昆鹏
Copyright (c) 2026 malobaby
```

在原有两行 copyright 后追加一行 `Copyright (c) 2026 malobaby`，保留上游致谢。

---

## Phase 3: Frontmatter 与内容交叉引用更新

### Task 3.1: 更新所有 Agent frontmatter `name:`

**Files:** `D:\su_workspace\cursor\zzz_vibe\supermalo\agents\malo-*.md`（9 个文件）

- [ ] **Step 1: 逐个替换 frontmatter 中的 `name:`**

各文件当前 frontmatter 第 2 行为 `name: xxx`，修改为 `name: malo-xxx`：

| 文件 | 修改前 | 修改后 |
|------|--------|--------|
| `malo-planner.md` | `name: planner` | `name: malo-planner` |
| `malo-architect.md` | `name: architect` | `name: malo-architect` |
| `malo-tdd-guide.md` | `name: tdd-guide` | `name: malo-tdd-guide` |
| `malo-code-reviewer.md` | `name: code-reviewer` | `name: malo-code-reviewer` |
| `malo-security-reviewer.md` | `name: security-reviewer` | `name: malo-security-reviewer` |
| `malo-build-error-resolver.md` | `name: build-error-resolver` | `name: malo-build-error-resolver` |
| `malo-e2e-runner.md` | `name: e2e-runner` | `name: malo-e2e-runner` |
| `malo-refactor-cleaner.md` | `name: refactor-cleaner` | `name: malo-refactor-cleaner` |
| `malo-doc-updater.md` | `name: doc-updater` | `name: malo-doc-updater` |

- [ ] **Step 2: 验证**

```powershell
Select-String -Path "D:\su_workspace\cursor\zzz_vibe\supermalo\agents\*.md" -Pattern "^name:" | ForEach-Object { $_.Line }
```

Expected: 全部 9 个 `name: malo-*`，无短名残留。

### Task 3.2: 更新所有 Skill frontmatter `name:`

**Files:** `D:\su_workspace\cursor\zzz_vibe\supermalo\skills\malo-*/**`（7 个条目）

- [ ] **Step 1: 替换目录型 skill 的 frontmatter**

`skills/malo-tdd-workflow/SKILL.md` L2: `name: tdd-workflow` → `name: malo-tdd-workflow`

`skills/malo-security-review/SKILL.md` L2: `name: security-review` → `name: malo-security-review`

- [ ] **Step 2: 替换单文件型 skill 的 frontmatter**

`skills/malo-coding-standards.md` L2: `name: coding-standards` → `name: malo-coding-standards`

`skills/malo-backend-patterns.md` L2: `name: backend-patterns` → `name: malo-backend-patterns`

`skills/malo-frontend-patterns.md` L2: `name: frontend-patterns` → `name: malo-frontend-patterns`

`skills/malo-clickhouse-io.md` L2: `name: clickhouse-io` → `name: malo-clickhouse-io`

`skills/malo-project-guidelines-example.md`：当前**无** frontmatter，需在文件开头新增：

```markdown
---
name: malo-project-guidelines-example
description: 项目特定 skill 模板，适用于为你的项目定制化 skill 配置。
---

# 项目指南 Skill（示例）
```

- [ ] **Step 3: 验证**

```powershell
Select-String -Path "D:\su_workspace\cursor\zzz_vibe\supermalo\skills\**\*.md" -Pattern "^name:" | ForEach-Object { $_.Line }
```

Expected: 全部 7 个 `name: malo-*`。

### Task 3.3: 更新 `commands/malo-auto.md` 路由表与全文

**File:** `D:\su_workspace\cursor\zzz_vibe\supermalo\commands\malo-auto.md`

- [ ] **Step 1: 替换 frontmatter description**

```markdown
// 修改前:
description: 智能命令选择器，自动分发到合适的 aimax 命令。

// 修改后:
description: 智能命令选择器，自动分发到合适的 malo 命令。
```

- [ ] **Step 2: 替换路由表中所有命令引用**

路由表（约 L27-37）中的 `命令` 列当前值为短名（如 `build-fix`），需要全部改为 `malo-*`：

| 当前值 | 替换为 |
|--------|--------|
| `build-fix` | `malo-build-fix` |
| `e2e` | `malo-e2e` |
| `test-coverage` | `malo-test-coverage` |
| `tdd` | `malo-tdd` |
| `code-review` | `malo-code-review` |
| `refactor-clean` | `malo-refactor-clean` |
| `update-docs` | `malo-update-docs` |
| `update-codemaps` | `malo-update-codemaps` |
| `plan` | `malo-plan` |

- [ ] **Step 3: 替换输出模板和示例中的 `/aimax:` → `/malo:malo-`**

全文搜索 `/aimax:` 替换为 `/malo:malo-`。示例：
- `/aimax:[命令]` → `/malo:malo-[命令]`
- `/aimax:auto` → `/malo:malo-auto`
- 所有示例中的 `/aimax:plan` → `/malo:malo-plan` 等

- [ ] **Step 4: 替换底部「相关命令」表**

约 L178-189 行，所有 `/aimax:xxx` → `/malo:malo-xxx`。

- [ ] **Step 5: 全文验证**

```powershell
Select-String -Path "D:\su_workspace\cursor\zzz_vibe\supermalo\commands\malo-auto.md" -Pattern "/aimax:"
```

Expected: 无任何匹配。

### Task 3.4: 更新 `rules/malo-agents.md` 的 agent 引用

**File:** `D:\su_workspace\cursor\zzz_vibe\supermalo\rules\malo-agents.md`

- [ ] **Step 1: 替换 Agent 名称表**

所有 agent 名称加上 `malo-` 前缀：

| 当前 | 替换 |
|------|------|
| `planner` | `malo-planner` |
| `architect` | `malo-architect` |
| `tdd-guide` | `malo-tdd-guide` |
| `code-reviewer` | `malo-code-reviewer` |
| `security-reviewer` | `malo-security-reviewer` |
| `build-error-resolver` | `malo-build-error-resolver` |
| `e2e-runner` | `malo-e2e-runner` |
| `refactor-cleaner` | `malo-refactor-cleaner` |
| `doc-updater` | `malo-doc-updater` |

同时替换路径中的 `~/.claude/agents/` 引用（文件名已带 `malo-` 前缀）。

- [ ] **Step 2: 替换「立即使用 Agent」部分的 agent 名称**

```markdown
// 修改前:
1. 复杂功能请求 - 使用 **planner** agent
2. 刚编写/修改的代码 - 使用 **code-reviewer** agent
3. Bug 修复或新功能 - 使用 **tdd-guide** agent
4. 架构决策 - 使用 **architect** agent

// 修改后:
1. 复杂功能请求 - 使用 **malo-planner** agent
2. 刚编写/修改的代码 - 使用 **malo-code-reviewer** agent
3. Bug 修复或新功能 - 使用 **malo-tdd-guide** agent
4. 架构决策 - 使用 **malo-architect** agent
```

- [ ] **Step 3: 全文验证**

```powershell
Select-String -Path "D:\su_workspace\cursor\zzz_vibe\supermalo\rules\malo-agents.md" -Pattern "`planner`|`architect`|`tdd-guide`|`code-reviewer`|`security-reviewer`|`build-error-resolver`|`e2e-runner`|`refactor-cleaner`|`doc-updater`"
```

Expected: 无任何匹配（所有 agent 名均已带 `malo-` 前缀）。

### Task 3.5: 更新各 command 文件内的 agent/skill 引用

**Files:** `D:\su_workspace\cursor\zzz_vibe\supermalo\commands\malo-*.md`（除 `malo-auto.md` 外的 9 个文件）

- [ ] **Step 1: 搜索所有 command 文件中的 agent/skill 引用**

```powershell
Select-String -Path "D:\su_workspace\cursor\zzz_vibe\supermalo\commands\*.md" -Pattern "planner|architect|tdd-guide|code-reviewer|security-reviewer|build-error-resolver|e2e-runner|refactor-cleaner|doc-updater|tdd-workflow|security-review|coding-standards|backend-patterns|frontend-patterns|clickhouse-io" -List
```

- [ ] **Step 2: 逐文件替换**

对每个匹配文件，将无前缀的 agent/skill 名称替换为 `malo-` 前缀版本。同时更新路径引用如 `~/.claude/agents/planner.md` → `~/.claude/agents/malo-planner.md`。

---

## Phase 4: 测试文件修正

### Task 4.1: 修改 `tests/utils.test.js`

**File:** `D:\su_workspace\cursor\zzz_vibe\supermalo\tests\utils.test.js`

- [ ] **Step 1: 更新导入**

```javascript
// 修改前（L5-15）:
import {
  getClaudeDir,
  getAimaxDir,
  getCustomDir,
  getVersionFilePath,
  ...
} from '../src/utils.js';

// 修改后:
import {
  getClaudeDir,
  getSupermaloDir,   // ← 改名
  getCustomDir,
  getVersionFilePath,
  ...
} from '../src/utils.js';
```

- [ ] **Step 2: 替换 `describe('getAimaxDir')` → `describe('getSupermaloDir')`**

```javascript
// 修改前（L26-31）:
describe('getAimaxDir', () => {
    it('should return path to aimax directory inside .claude', () => {
      const result = getAimaxDir();
      const expected = path.join(os.homedir(), '.claude', 'aimax');
      expect(result).toBe(expected);
    });
});

// 修改后:
describe('getSupermaloDir', () => {
    it('should return path to supermalo directory inside .claude', () => {
      const result = getSupermaloDir();
      const expected = path.join(os.homedir(), '.claude', 'supermalo');
      expect(result).toBe(expected);
    });
});
```

- [ ] **Step 3: 替换 `getVersionFilePath` 测试**

```javascript
// 修改前（L42-47）:
describe('getVersionFilePath', () => {
    it('should return path to .aimax-version file', () => {
      const result = getVersionFilePath();
      const expected = path.join(os.homedir(), '.claude', '.aimax-version');
      expect(result).toBe(expected);
    });
});

// 修改后:
describe('getVersionFilePath', () => {
    it('should return path to .supermalo-version file', () => {
      const result = getVersionFilePath();
      const expected = path.join(os.homedir(), '.claude', '.supermalo-version');
      expect(result).toBe(expected);
    });
});
```

- [ ] **Step 4: 替换 `COMPONENTS.commands` 断言**

```javascript
// 修改前（L76-82）:
it('should have commands component with correct structure', () => {
      expect(COMPONENTS.commands).toMatchObject({
        name: 'aimax 斜杠指令',
        source: 'commands',
        target: 'commands/aimax',
        pattern: '*.md'
      });
});

// 修改后:
it('should have commands component with correct structure', () => {
      expect(COMPONENTS.commands).toMatchObject({
        name: 'Supermalo 斜杠指令',
        source: 'commands',
        target: 'commands/malo',
        pattern: '*.md'
      });
});
```

- [ ] **Step 5: 修正预存的版本断言 bug**

```javascript
// 修改前（L53）:
expect(result).toBe('0.0.1');

// 修改后:
expect(result).toBe('0.1.0');
```

- [ ] **Step 6: 替换 `.aimax-version` → `.supermalo-version`**

`saveInstalledVersion` 测试（约 L164, L186）中的 `expect.stringContaining('.aimax-version')` → `expect.stringContaining('.supermalo-version')`

- [ ] **Step 7: 替换 `getInstalledVersion` 测试中的 `.aimax-version`**

`describe('getInstalledVersion')` 块内（约 L107-114）：
- `path.join(os.tmpdir(), '.aimax-test-version')` → `path.join(os.tmpdir(), '.supermalo-test-version')`
- `if (p.includes('.aimax-version'))` → `if (p.includes('.supermalo-version'))`

- [ ] **Step 8: 全文验证**

```powershell
Select-String -Path "D:\su_workspace\cursor\zzz_vibe\supermalo\tests\utils.test.js" -Pattern "aimax|AI MAX|0\.0\.1"
```

Expected: 无任何匹配。

### Task 4.2: 修改 `tests/installer.test.js`

**File:** `D:\su_workspace\cursor\zzz_vibe\supermalo\tests\installer.test.js`

- [ ] **Step 1: 替换测试目录名**

```javascript
// 修改前（L31）:
const testDir = path.join(os.tmpdir(), 'aimax-test-' + Date.now());

// 修改后:
const testDir = path.join(os.tmpdir(), 'supermalo-test-' + Date.now());
```

- [ ] **Step 2: 替换 `AI MAX` 注释引用**

```javascript
// 修改前（约 L139、L142-143）中的 "AI MAX" → "Supermalo"
```

```javascript
// 修改前（L154）:
const versionFile = path.join(mockClaudeDir, '.aimax-version');

// 修改后:
const versionFile = path.join(mockClaudeDir, '.supermalo-version');
```

- [ ] **Step 3: 全文验证**

```powershell
Select-String -Path "D:\su_workspace\cursor\zzz_vibe\supermalo\tests\installer.test.js" -Pattern "aimax|AI MAX"
```

Expected: 无任何匹配。

---

## Phase 5: 文档与示例文件

### Task 5.1: 新建 `NOTICE` 文件

**File:** `D:\su_workspace\cursor\zzz_vibe\supermalo\NOTICE`

- [ ] **Step 1: 创建 NOTICE 文件**

```markdown
# NOTICE

Supermalo is derived from the following open source projects:

## ai-max (MIT)
- Repository: https://github.com/zhukunpenglinyutong/ai-max
- Copyright (c) 2026 朱昆鹏
- Provided the npm CLI installer, Chinese command system, and out-of-the-box component structure.

## everything-claude-code (MIT)
- Repository: https://github.com/affaan-m/everything-claude-code
- Copyright (c) 2026 everything-claude-code
- Provided the original agents, skills, hooks methodology and initial patterns.

Supermalo is an independent fork and is not affiliated with the upstream projects.
```

### Task 5.2: 更新 `examples/CLAUDE.md`

**File:** `D:\su_workspace\cursor\zzz_vibe\supermalo\examples\CLAUDE.md`

- [ ] **Step 1: 替换可用命令部分**

```markdown
// 修改前（L90-94）:
- `/aimax:tdd` - 测试驱动开发工作流
- `/aimax:plan` - 创建实现计划
- `/aimax:code-review` - 审查代码质量
- `/aimax:build-fix` - 修复构建错误

// 修改后:
- `/malo:malo-tdd` - 测试驱动开发工作流
- `/malo:malo-plan` - 创建实现计划
- `/malo:malo-code-review` - 审查代码质量
- `/malo:malo-build-fix` - 修复构建错误
```

- [ ] **Step 2: 全文验证**

```powershell
Select-String -Path "D:\su_workspace\cursor\zzz_vibe\supermalo\examples\CLAUDE.md" -Pattern "/aimax:"
```

### Task 5.3: 更新 `examples/user-CLAUDE.md`

**File:** `D:\su_workspace\cursor\zzz_vibe\supermalo\examples\user-CLAUDE.md`

- [ ] **Step 1: 替换规则表引用**

```markdown
// 修改前（L31-37）中的 rules 引用:
| security.md | ... |
| coding-style.md | ... |

// 修改后:
| malo-security.md | ... |
| malo-coding-style.md | ... |
```

全部 7 个规则文件名加上 `malo-` 前缀。

- [ ] **Step 2: 替换 Agent 表**

```markdown
// 修改前（L46-55）中的 agent 名:
| planner | 功能实现规划 |
| architect | 系统设计和架构 |
...

// 修改后:
| malo-planner | 功能实现规划 |
| malo-architect | 系统设计和架构 |
...
```

全部 9 个 agent 名加上 `malo-` 前缀。

- [ ] **Step 3: 全文验证**

```powershell
Select-String -Path "D:\su_workspace\cursor\zzz_vibe\supermalo\examples\user-CLAUDE.md" -Pattern "`planner`|`architect`|`tdd-guide`|`code-reviewer`|`security-reviewer`|`build-error-resolver`|`e2e-runner`|`refactor-cleaner`|`doc-updater`"
```

---

## Phase 6: 全局验证

### Task 6.1: 全文搜索残留

- [ ] **Step 1: 搜索所有残留 `aimax` / `AI MAX` / `/aimax:`**

```powershell
cd D:\su_workspace\cursor\zzz_vibe\supermalo
Get-ChildItem -Recurse -Include *.js,*.json,*.md -Exclude NOTICE,命名规范.md,package-lock.json,node_modules | Select-String -Pattern "aimax|AI MAX|/aimax:" -List
```

Expected 匹配：
- `NOTICE` — 致谢引用（允许）
- `命名规范.md` — 规范说明中提到上游名称（允许）

除此之外无任何匹配。

### Task 6.2: 运行测试

- [ ] **Step 1: 安装依赖并运行测试**

```powershell
cd D:\su_workspace\cursor\zzz_vibe\supermalo
npm install
npm test
```

Expected: 所有测试通过。

### Task 6.3: npm link 验证

- [ ] **Step 1: 创建全局链接**

```powershell
cd D:\su_workspace\cursor\zzz_vibe\supermalo
npm link
```

Expected: 无错误，`supermalo` 命令可用。

- [ ] **Step 2: 验证 CLI**

```powershell
supermalo --version
```

Expected: 显示 `0.1.0`

```powershell
supermalo list
```

Expected: 列出 4 个组件，包含 `Supermalo 斜杠指令`。

---

## 依赖关系图

```
Phase 1 (文件重命名) ──┬── Phase 3 (frontmatter + 交叉引用)
                       │
                       ├── Phase 2 (源码配置，可与 Phase 3 并行)
                       │
                       └── Phase 4 (测试，依赖 Phase 2)
                            │
Phase 5 (文档) ←── 不依赖以上各阶段 ──→ Phase 6 (验证，必须最后)
```

- Phase 1 必须最先完成（后续所有任务引用新文件名）
- Phase 2 与 Phase 3 可并行执行
- Phase 4 依赖 Phase 2（函数名变更）
- Phase 5 可任意时间执行
- Phase 6 必须在所有阶段完成后执行
