# Asset Directory Reorganization Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Consolidate 8 asset directories (`agents/`, `commands/`, `rules/`, `skills/`, `hooks/`, `mcp-configs/`, `plugins/`, `examples/`) into a single `assets/` parent directory to declutter the repo root.

**Architecture:** Create `assets/` directory, move all 8 asset subdirectories into it via `git mv`, update `COMPONENTS.source` paths in `src/utils.js`, update `package.json` `"files"` field, update test assertions, and refresh documentation references.

**Tech Stack:** Node.js ESM, Git, vitest

---

## Impact Analysis

### Code changes (3 files, ~15 lines total)
| File | Change | Reason |
|------|--------|--------|
| `src/utils.js:82,89,96,103` | 4 `source` values | Installer must find assets under `assets/` |
| `package.json:29-39` | `"files"` array | npm publish must include `assets/` |
| `tests/utils.test.js:61,70,79,88` | 4 test assertions | Match new source path values |

### Doc changes (2 files)
| File | Change | Reason |
|------|--------|--------|
| `CLAUDE.md:50-53` | Table source column | Reflect repo structure |
| `CLAUDE.md:73` | hooks.json link | Fix broken link |
| `README.md:138-144` | Project tree | Reflect repo structure |

### NOT changing (intentionally)
- `命名规范.md` — documents install targets (`~/.claude/...`), not repo source paths
- All asset `.md` files — reference `~/.claude/...` install paths, not repo layout
- `examples/` — reference install paths
- `docs/superpowers/plans/` — historical records
- `tests/installer.test.js` — creates mock dirs independent of real source layout; no changes needed

---

### Task 1: Create `assets/` directory and move all asset subdirectories

**Files:**
- Move: `agents/` → `assets/agents/`
- Move: `commands/` → `assets/commands/`
- Move: `rules/` → `assets/rules/`
- Move: `skills/` → `assets/skills/`
- Move: `hooks/` → `assets/hooks/`
- Move: `mcp-configs/` → `assets/mcp-configs/`
- Move: `plugins/` → `assets/plugins/`
- Move: `examples/` → `assets/examples/`

- [ ] **Step 1: Create `assets/` directory**

```powershell
New-Item -ItemType Directory -Path "assets"
```

- [ ] **Step 2: Move all 8 directories into `assets/` using git mv**

```powershell
git mv agents assets/
git mv commands assets/
git mv rules assets/
git mv skills assets/
git mv hooks assets/
git mv mcp-configs assets/
git mv plugins assets/
git mv examples assets/
```

- [ ] **Step 3: Verify the move**

```powershell
Get-ChildItem assets/ -Name -Directory
```

Expected output (8 directories):
```
agents
commands
examples
hooks
mcp-configs
plugins
rules
skills
```

- [ ] **Step 4: Verify root directory is clean**

```powershell
Get-ChildItem -Name -Directory
```

Expected: `agents/`, `commands/`, `rules/`, `skills/`, `hooks/`, `mcp-configs/`, `plugins/`, `examples/` should NO LONGER appear at root level.

- [ ] **Step 5: Commit the move**

```bash
git add -A
git commit -m "refactor: move asset directories into assets/

Consolidate agents, commands, rules, skills, hooks, mcp-configs,
plugins, and examples under a single assets/ parent directory to
declutter the repository root.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"
```

---

### Task 2: Update `COMPONENTS.source` paths in `src/utils.js`

**Files:**
- Modify: `src/utils.js:82,89,96,103`

- [ ] **Step 1: Update agents source path**

In `src/utils.js`, change line 82:
```js
// Before:
    source: 'agents',
// After:
    source: 'assets/agents',
```

- [ ] **Step 2: Update rules source path**

In `src/utils.js`, change line 89:
```js
// Before:
    source: 'rules',
// After:
    source: 'assets/rules',
```

- [ ] **Step 3: Update commands source path**

In `src/utils.js`, change line 96:
```js
// Before:
    source: 'commands',
// After:
    source: 'assets/commands',
```

- [ ] **Step 4: Update skills source path**

In `src/utils.js`, change line 103:
```js
// Before:
    source: 'skills',
// After:
    source: 'assets/skills',
```

- [ ] **Step 5: Verify COMPONENTS object looks correct**

Read `src/utils.js:78-108` — the COMPONENTS block should now show:

```js
export const COMPONENTS = {
  agents: {
    name: 'Agents（代理）',
    description: '专用子代理（malo-planner, malo-architect, malo-tdd-guide 等）',
    source: 'assets/agents',
    target: 'agents',
    pattern: '*.md'
  },
  rules: {
    name: 'Rules（规则）',
    description: '必须遵循的准则（malo-security, malo-testing, malo-coding-style 等）',
    source: 'assets/rules',
    target: 'rules',
    pattern: '*.md'
  },
  commands: {
    name: 'Supermalo 斜杠指令',
    description: '斜杠命令（/malo:malo-plan, /malo:malo-tdd, /malo:malo-code-review 等）',
    source: 'assets/commands',
    target: 'commands/malo',
    pattern: '*.md'
  },
  skills: {
    name: 'Skills（技能）',
    description: '工作流定义和领域知识',
    source: 'assets/skills',
    target: 'skills',
    pattern: '**/*',
    recursive: true
  }
};
```

- [ ] **Step 6: Commit**

```bash
git add src/utils.js
git commit -m "fix: update COMPONENTS.source paths to reflect assets/ directory

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"
```

---

### Task 3: Update `package.json` files field for npm publish

**Files:**
- Modify: `package.json:29-39`

- [ ] **Step 1: Update the "files" array**

In `package.json`, change lines 29-39:
```json
// Before:
  "files": [
    "bin",
    "src",
    "agents",
    "rules",
    "commands",
    "skills",
    "hooks",
    "mcp-configs",
    "examples"
  ],
// After:
  "files": [
    "bin",
    "src",
    "assets"
  ],
```

- [ ] **Step 2: Verify package.json is valid JSON**

```powershell
node -e "JSON.parse(require('fs').readFileSync('package.json','utf8')); console.log('OK')"
```

Expected: `OK`

- [ ] **Step 3: Commit**

```bash
git add package.json
git commit -m "fix: update npm files field to use assets/ directory

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"
```

---

### Task 4: Update test assertions in `tests/utils.test.js`

**Files:**
- Modify: `tests/utils.test.js:61,70,79,88`

- [ ] **Step 1: Update agents test assertion**

In `tests/utils.test.js`, change line 61:
```js
// Before:
        source: 'agents',
// After:
        source: 'assets/agents',
```

- [ ] **Step 2: Update rules test assertion**

In `tests/utils.test.js`, change line 70:
```js
// Before:
        source: 'rules',
// After:
        source: 'assets/rules',
```

- [ ] **Step 3: Update commands test assertion**

In `tests/utils.test.js`, change line 79:
```js
// Before:
        source: 'commands',
// After:
        source: 'assets/commands',
```

- [ ] **Step 4: Update skills test assertion**

In `tests/utils.test.js`, change line 88:
```js
// Before:
        source: 'skills',
// After:
        source: 'assets/skills',
```

- [ ] **Step 5: Run tests to verify they pass**

```powershell
npm test
```

Expected: All tests PASS.

- [ ] **Step 6: Commit**

```bash
git add tests/utils.test.js
git commit -m "test: update COMPONENTS.source assertions for assets/ path

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"
```

---

### Task 5: Update documentation references

**Files:**
- Modify: `CLAUDE.md:50-53,57,59,73`
- Modify: `README.md:138-144`

- [ ] **Step 1: Update CLAUDE.md architecture table (source column)**

In `CLAUDE.md`, change lines 50-53:
```markdown
<!-- Before: -->
| Agents | `agents/` | `agents/` | `.md` 文件，含 YAML frontmatter（`name`、`description`、`tools`、`model`） |
| Rules | `rules/` | `rules/` | `.md` 文件（无 frontmatter） |
| Commands | `commands/` | `commands/malo/` | `.md` 文件，含 YAML frontmatter（`description`）。隔离在 `malo/` 子目录以实现 `/malo:malo-*` 命名空间 |
| Skills | `skills/` | `skills/` | 混合格式：部分为独立 `.md` 文件，部分为子目录中的 `SKILL.md` |

<!-- After: -->
| Agents | `assets/agents/` | `agents/` | `.md` 文件，含 YAML frontmatter（`name`、`description`、`tools`、`model`） |
| Rules | `assets/rules/` | `rules/` | `.md` 文件（无 frontmatter） |
| Commands | `assets/commands/` | `commands/malo/` | `.md` 文件，含 YAML frontmatter（`description`）。隔离在 `malo/` 子目录以实现 `/malo:malo-*` 命名空间 |
| Skills | `assets/skills/` | `skills/` | 混合格式：部分为独立 `.md` 文件，部分为子目录中的 `SKILL.md` |
```

- [ ] **Step 2: Update CLAUDE.md line 57 — Commands description**

```markdown
<!-- Before: -->
1. **Commands 隔离在 `commands/malo/` 中**：与 agents/rules/skills 直接放入共享的 `~/.claude/` 目录不同，commands 拥有自己的 `malo/` 子目录。

<!-- After: -->
1. **Commands 隔离在 `commands/malo/` 中**：与 agents/rules/skills 直接放入共享的 `~/.claude/` 目录不同，commands 拥有自己的 `malo/` 子目录。仓库中所有资产统一存放在 `assets/` 目录下。
```

- [ ] **Step 3: Update CLAUDE.md line 59 — shared directory note**

```markdown
<!-- Before: -->
2. **精准卸载**：安装器在 `~/.claude/.supermalo-version` 中记录复制的每一个文件。卸载时仅删除这些精确记录的文件——绝不会删除共享同一目录（agents/、rules/、skills/）的其他插件文件。

<!-- After: -->
2. **精准卸载**：安装器在 `~/.claude/.supermalo-version` 中记录复制的每一个文件。卸载时仅删除这些精确记录的文件——绝不会删除共享同一目录（`agents/`、`rules/`、`skills/`）的其他插件文件。
```

- [ ] **Step 4: Update CLAUDE.md line 73 — hooks link**

```markdown
<!-- Before: -->
[hooks/hooks.json](hooks/hooks.json) 定义了 Claude Code 的钩子：

<!-- After: -->
[assets/hooks/hooks.json](assets/hooks/hooks.json) 定义了 Claude Code 的钩子：
```

- [ ] **Step 5: Update README.md project structure tree**

In `README.md`, change lines 138-144:
```
<!-- Before: -->
├── agents/              # 专用子代理（malo-planner、malo-code-reviewer 等）
├── commands/            # 斜杠命令（malo-plan.md、malo-tdd.md 等）
├── rules/               # 强制性准则（malo-security.md 等）
├── skills/              # 工作流与领域知识（malo-tdd-workflow/ 等）
├── hooks/               # 事件触发器（hooks.json）
├── mcp-configs/         # MCP 服务器配置
├── examples/            # 项目级 / 用户级 CLAUDE.md 示例

<!-- After: -->
├── assets/              # Claude Code 资产集合
│   ├── agents/          # 专用子代理（malo-planner、malo-code-reviewer 等）
│   ├── commands/        # 斜杠命令（malo-plan.md、malo-tdd.md 等）
│   ├── rules/           # 强制性准则（malo-security.md 等）
│   ├── skills/          # 工作流与领域知识（malo-tdd-workflow/ 等）
│   ├── hooks/           # 事件触发器（hooks.json）
│   ├── mcp-configs/     # MCP 服务器配置
│   ├── plugins/         # 社区插件注册表
│   └── examples/        # 项目级 / 用户级 CLAUDE.md 示例
```

- [ ] **Step 6: Commit**

```bash
git add CLAUDE.md README.md
git commit -m "docs: update paths to reflect assets/ directory reorganization

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"
```

---

### Task 6: Final verification

- [ ] **Step 1: Run full test suite**

```powershell
npm test
```

Expected: All tests PASS (same as Task 4 step 5 — re-verify after all changes).

- [ ] **Step 2: Verify git status is clean**

```powershell
git status
```

Expected: `nothing to commit, working tree clean`

- [ ] **Step 3: Spot-check a source path resolves correctly**

```powershell
node -e "import('./src/utils.js').then(m => { const p = require('path'); const fs = require('fs-extra'); const d = m.getSourceDir(); for (const [k,v] of Object.entries(m.COMPONENTS)) { const full = p.join(d, v.source); console.log(k + ':', full, '→ exists:', fs.pathExistsSync(full)); } })"
```

Expected: All 4 components should show `exists: true`.

---

## Self-Review

### 1. Spec coverage
- ✅ Move 8 directories into `assets/` — Task 1
- ✅ Update installer code — Task 2
- ✅ Update npm publish config — Task 3
- ✅ Update tests — Task 4
- ✅ Update documentation — Task 5
- ✅ End-to-end verification — Task 6

### 2. Placeholder scan
- ✅ No "TBD", "TODO", or "implement later"
- ✅ Every code step has actual code shown
- ✅ Every command has expected output
- ✅ No "add appropriate error handling" vagueness

### 3. Type/name consistency
- ✅ `assets/` prefix used consistently across all tasks
- ✅ Source path format: `assets/<component>` with no trailing slash
- ✅ COMPONENTS keys unchanged: `agents`, `rules`, `commands`, `skills`
- ✅ Target paths unchanged — only source paths affected
