# my-skills

OpenCode AI 技能集合 —— 一个实现完整软件工程流水线的指令集仓库。

## 技能一览

| 技能 | 职责 | 输入 | 输出 |
|------|------|------|------|
| prd-writer | 产品需求分析 | 业务想法 | PRD 文档 |
| system-architect | 系统架构设计 | PRD | 架构文档 (SAD) |
| task-decomposer | 详细设计拆分 | 架构文档 | 模块详设 + 项目规则 |
| code-developer | 代码生成 | 详设文档 | 生产级代码 |
| code-reviewer | 代码评审 | 源代码 | 评审报告 |
| reviewer-expert | 文档/用例评审 | PRD/架构/详设/用例 | 评审报告 |
| tester | 测试用例 + 测试代码 | 详设文档 | 用例 + 测试代码 + 报告 |
| bug-fixer | Bug 修复 | 缺陷报告 | 修复代码 |
| devops-engineer | 部署配置 | 架构文档 | 部署脚本 |
| doc-writer | 文档生成 | 代码/配置 | 用户文档 |
| dba-designer | 数据库设计 | 详设 DDL 节 | 建表脚本 |
| ai-memory | AI 记忆持久化 | — | 会话摘要/决策 |
| code-archaeologist | 代码分析 | 遗留代码 | 分析报告 |

## 快速开始

详见 [AGENTS.md](AGENTS.md)：

- 流水线架构与技能依赖关系
- SKILL.md 约定的文档产出路径
- 关键项目规则（LC-001 语言约束等）
- SKILL.md 优化方法论（按需加载 + 合并两原则）
- 内存协议与会话管理
