# Superpowers (Optimized)

> 基于 [obra/superpowers](https://github.com/obra/superpowers) 优化的企业级 AI 编程工作流系统

**核心理念**：文档驱动开发 + 严格 TDD，确保每一步有据可查、质量可控。

---

## 安装

### Claude Code

**第一步：添加 marketplace**

编辑 `~/.claude/plugins/known_marketplaces.json`，添加以下内容：

```json
{
  "claude-plugins-official": { ... },
  "jerrydinguk-superpowers": {
    "source": {
      "source": "github",
      "repo": "jerrydinguk-coder/superpowers"
    },
    "installLocation": "/Users/你的用户名/.claude/plugins/marketplaces/jerrydinguk-superpowers",
    "lastUpdated": "2026-01-17T18:00:00.000Z"
  }
}
```

> 注意：将 `你的用户名` 替换为实际的系统用户名

**第二步：安装插件**

```bash
/plugin install superpowers-pro@jerrydinguk-superpowers
```

### OpenCode

```bash
git clone git@github.com:jerrydinguk-coder/superpowers.git ~/.config/opencode/superpowers
```

### 更新

```bash
# Claude Code
/plugin update superpowers-pro@jerrydinguk-superpowers

# OpenCode
cd ~/.config/opencode/superpowers && git pull
```

---

## 工作流程

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                                                                             │
│   ┌───────┐   ┌───────┐   ┌───────┐   ┌───────┐   ┌───────┐   ┌───────┐   │
│   │ INPUT │──▶│DESIGN │──▶│ PLAN  │──▶│EXECUTE│──▶│REVIEW │──▶│FINISH │   │
│   └───────┘   └───────┘   └───────┘   └───────┘   └───────┘   └───────┘   │
│       │           │           │           │           │           │        │
│   feature-    /brainstorm  test-plan   SDD+TDD    batch-rev   verify      │
│   key                      impl-plan   per task   cr-report   merge       │
│                            worktree                                        │
│                                                                            │
│   ═════════   ═════════   ═════════   ═════════   ---------   ═════════   │
│   REQUIRED    REQUIRED    REQUIRED    REQUIRED    optional    REQUIRED    │
│                                                                            │
└─────────────────────────────────────────────────────────────────────────────┘

    Note: /brainstorm 需要人工触发，但在主流程中是必须步骤
```

### 阶段详情

| Phase | Step | Output | Required |
|:------|:-----|:-------|:---------|
| **INPUT** | Enter `feature-key` | - | Required |
| **DESIGN** | `/brainstorm` | business.md, prd.md, tech-design.md | Required |
| **PLAN** | Write Plans | test-plan.md, impl-plan.md | Required |
| | Git Worktree | `.worktrees/<key>/` | Required |
| **EXECUTE** | SDD + TDD (per task) | test-report.md | Required |
| **REVIEW** | batch-review | cr-report.md | Optional |
| | verification | - | Required |
| **FINISH** | finishing-branch | Merge / PR | Required |

### 强制规则

- **TDD**: 必须先写测试看失败 (RED)，再写代码看通过 (GREEN)
- **test-report.md**: 必须有 RED/GREEN 证据才能标记任务完成
- **verification**: Subagent 报告不可信，主 Agent 必须亲自运行验证命令
- **systematic-debugging**: 修 bug 时必须先定位根因，禁止盲目修复

**核心原则：**
- **TDD 强制**：先写测试看失败 (RED)，再写代码看通过 (GREEN)
- **证据驱动**：test-report.md 必须有 RED/GREEN 证据才能标记任务完成
- **独立验证**：Subagent 报告不可信，主 Agent 必须亲自运行验证命令
- **批量高效**：batch-review 替代 per-task review，节省 60% 调用

---

## Skills 列表

| Skill | 用途 |
|:------|:-----|
| `using-superpowers` | 入口，强制检查适用的 skills |
| `brainstorming` | 需求收集与设计 |
| `writing-plans` | 测试计划 + 实施计划 |
| `subagent-driven-development` | 当前会话逐任务执行 |
| `finishing-a-development-branch` | 完成与集成 |
| `test-driven-development` | TDD 工作流（辅助） |
| `systematic-debugging` | 系统化调试（辅助） |
| `verification-before-completion` | 完成前验证（辅助） |
| `batch-review` | 批量代码审查（可选，手动触发） |

---

## 文档结构

每个功能生成 7 份文档，构成完整追溯链：

```
docs/features/<feature-key>/
├── business.md          # 为什么做
├── prd.md               # 做什么
├── tech-design.md       # 怎么做
├── test-plan.md         # 测什么
├── implementation-plan.md # 如何实施
├── test-report.md       # RED/GREEN 证据
└── cr-report.md         # 代码审查结果
```

---

## 版本信息

- **当前版本**: 4.2.0
- **上游版本**: [obra/superpowers](https://github.com/obra/superpowers) v4.0.3
- **许可证**: MIT

---

## 支持

- **Issues**: https://github.com/jerrydinguk-coder/superpowers/issues
- **上游项目**: https://github.com/obra/superpowers
