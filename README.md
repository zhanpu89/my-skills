# my-skills

OpenCode AI 技能集合 —— 8 个指导 AI 代理完成软件工程各阶段的指令集。

## 技能一览

| 技能 | 角色 | 主要文档 |
|------|------|---------|
| **prd-writer** | 需求分析 → PRD | `doc/prd/` |
| **review-expert** | 文档/用例评审 | `doc/review/` |
| **system-architect** | 架构设计 (SAD) | `doc/arch/` |
| **task-decomposer** | 详设拆分 + 项目规则 | `doc/detailed/` |
| **code-reviewer** | 代码评审 | `doc/review/` |
| **tester** | 测试用例 + 测试代码 | `doc/tester/` |
| **dba-designer** | 数据库 DDL 设计 | `doc/db/` |
| **ai-memory** | AI 记忆持久化管理 | — |

## 目录结构

```
{skill}/
├── SKILL.md          # 技能指令（YAML + 工作流）
├── resources/        # 参考文档（术语表、检查清单、模式参考）
└── templates/        # 输出模板
```

## 更多信息

详见 [AGENTS.md](AGENTS.md)：
- 关键约定（LC-001 语言约束、P0/P1/P2 严重等级等）
- 内存协议与会话管理
- SKILL.md 优化方法论（按需加载 + 合并两原则）
