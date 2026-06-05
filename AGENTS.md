# AGENTS.md — my-skills

## What this is

A monorepo of 14 OpenCode AI **skills** that collectively implement a software engineering pipeline. Each skill is an instruction set (not executable code) that guides an AI agent through a specific phase of software development.

## Pipeline architecture

```
prd-writer → system-architect → task-decomposer → code-developer → code-reviewer → tester → devops-engineer → doc-writer
                ↑                    ↑                ↑               |              |
           review-expert         review-expert    review-expert    bug-fixer    review-expert
                                                                      ↓
                                                                 code-reviewer (re-review)
```

- **review-expert**: document quality gate (PRD → arch → detailed design → test cases). Loop-fix mode auto-invokes upstream skills to fix P0 problems (max 3 rounds).
- **bug-fixer**: code quality gate. Fixes bugs reported by tester, then triggers code-reviewer re-review.

## Skill directory layout

```
{skill-name}/
├── SKILL.md          # Main instructions (YAML front-matter + workflow)
├── resources/        # Reference docs (glossary, patterns, checklists)
└── templates/        # Output templates
```

Every `SKILL.md` has YAML front-matter with exactly 5 fields: `name`, `description`, `inputs`, `outputs`, `allowed-tools`. Description always includes "适用场景:" and "不适用场景（勿触发）:" bullet lists.

## Document artifact paths

| Path | Owner | Content |
|------|-------|---------|
| `doc/prd/` | prd-writer | Product requirements |
| `doc/arch/` | system-architect | Architecture (SAD) |
| `doc/detailed/` | task-decomposer | Detailed design per module |
| `doc/detailed/项目规则.md` | task-decomposer | Project rules (LC-*, BR-*, ER-*) |
| `src/` | code-developer | Source code |
| `doc/review/` | review-expert / code-reviewer | Review reports |
| `doc/tester/` | tester | Test cases & reports |
| `doc/fix/` | bug-fixer | Bug fix reports |
| `doc/db/` | dba-designer | DDL scripts |
| `deploy/` | devops-engineer | Deployment configs |

## Key conventions

- **LC-001**: Primary backend language (Java/Python/Go/Node.js). Must exist in `项目规则.md` before code generation.
- **LC-FE-001**: Frontend framework (Vue3/React/无).
- **P0/P1/P2**: Problem severity. P0 blocks everything, P1 is high risk, P2 is suggestion.
- **Status emojis**: `🟡 草稿` → `🟢 已确认` (document lifecycle).
- **Single-item pacing**: Generate one module at a time, pause for user "继续" before next. Exceptions only if user explicitly says "全部自动生成".
- **Checklist-driven**: Every output has a pre-write checklist. All items must pass before writing.
- **Progress files**: `_PROGRESS.md` tracks multi-module progress across skill sessions.

## Memory protocol (required)

From `.workspace/project_rules.md`:
- Every session starts with `init_session`
- Save summaries at milestones with `save_summary`
- Record decisions immediately with `add_decision`
- Search history with `search_summaries` / `search_summaries_fts` before repeating work
- Before each response, emit `<memory_plan>{...}</memory_plan>` with planned memory operations

## Optimization in progress (source of truth: `.optimization-plan.md`)

Some skills still use **old structure** that is in the process of being migrated:
- `references/` → should be `resources/`
- `assets/` → should be `templates/`
- `evals/` → should be removed (eval content merged elsewhere)
- Target: max 6 files per skill (SKILL.md + resources/* + templates/*)

Current state: first 7 skills already have glossary.md and updated front-matter. Remaining 6 (devops-engineer, bug-fixer, tester, prd-writer, doc-writer, code-archaeologist) still in old layout or pending optimization.

**Do not trigger the optimization plan unless user explicitly asks to continue optimization work. The plan is a reference, not an active task.**

## SKILL.md 优化方法论（两原则）

对所有 skill 进行结构优化时遵循以下两条原则：

### 原则 1：按需加载（Lazy Loading）

参考文件的加载时机必须与工作流步骤严格对齐，仅在需要时加载，完成后释放。

**判断标准**：
- 如果一个文件中包含在不同步骤加载的内容 → **拆分**
- 如果文件行数 > 200 且包含多个独立生命周期 → **优先拆分**
- 如果文件 < 30 行且生命周期独立 → 保持独立（无需拆分）
- 释放时机在 SKILL.md 中必须显式定义（懒加载表 + 参考表）

**SKILL.md 必含 3 个表**：
- `懒加载原则` 表：步骤 | 保持加载 | 释放
- `合并原则` 表/说明：解释 merge/split 决策
- `参考文件` 表：文件 | 行数 | 步骤 | 加载时机 | 释放时机

### 原则 2：合并原则（Merge）

相同生命周期的内容必须合并在同一个文件中，用 `##` 标题导航。

**判断标准**：
- 多个文件在同一步骤被加载且互斥使用（如不同语言的代码模板）→ **合并到一个文件，用 `## {语言名}` 导航**
- 多个文件内容属于同一操作流程 → **不要拆分**
- 内容重复 → 统一到一个文件，其余改为引用

### 文件命名与引用规范

- 资源文件：`resources/{功能描述}.md`（用连字符分隔词）
- 模板文件：`templates/{功能描述}-template.md`
- 拆分后文件名用描述性名称（不用 `-p1`、`-part1` 等序号）
- 跨文件引用使用路径：如 `见 \`table-rules.md\``
- SKILL.md 保留原始文件名（不含路径），参考表使用完整路径（`resources/`、`templates/`）

### 优化验证清单

- [ ] 懒加载表加载/释放时机与工作流步骤对齐
- [ ] 参考表行数与实际 `wc -l` 一致
- [ ] 无死引用（指向已删除或重命名文件的引用）
- [ ] 无内容重复（同一份数据定义只出现在一个文件）
- [ ] 合并原则中有分拆说明（原文件名 + 行数 + 理由）

## Testing & validation

There are no tests, linter configs, or build scripts in this repo. This is an instruction-only repository. Validation is done by loading a skill and checking if OpenCode triggers it correctly.
