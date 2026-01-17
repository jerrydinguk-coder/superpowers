# Superpowers (Optimized)

> 基于 [obra/superpowers](https://github.com/obra/superpowers) 优化的企业级 AI 编程工作流系统

**核心理念**：文档驱动开发 + 严格 TDD，确保每一步有据可查、质量可控。

---

## 安装

### Claude Code

```bash
# 直接从 GitHub 安装
claude /plugin install superpowers@jerrydinguk-coder/superpowers
```

### OpenCode

```bash
git clone https://github.com/jerrydinguk-coder/superpowers.git ~/.config/opencode/superpowers
```

### 更新

```bash
# Claude Code
claude /plugin update superpowers

# OpenCode
cd ~/.config/opencode/superpowers && git pull
```

---

## 工作流程

```
用户需求 → Brainstorming → Writing Plans → Subagent Execution → Finishing Branch
              ↓                  ↓                  ↓                  ↓
         收集需求/设计      创建测试+实施计划    TDD实现+双重审查     验证/合并/PR
```

**关键特性：**
- RED-GREEN TDD：先写测试看失败，再写代码看通过
- 双重审查：规范合规 + 代码质量
- 完整追溯：需求 → 测试 → 代码 → 审查

---

## Skills 列表

| Skill | 用途 |
|:------|:-----|
| `using-superpowers` | 入口，强制检查适用的 skills |
| `brainstorming` | 需求收集与设计 |
| `writing-plans` | 测试计划 + 实施计划 |
| `subagent-driven-development` | 当前会话逐任务执行 |
| `test-driven-development` | TDD 工作流 |
| `systematic-debugging` | 系统化调试 |
| `verification-before-completion` | 完成前验证 |
| `finishing-a-development-branch` | 完成与集成 |

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

- **当前版本**: 4.1.0
- **上游版本**: [obra/superpowers](https://github.com/obra/superpowers) v4.0.3
- **许可证**: MIT

---

## 支持

- **Issues**: https://github.com/jerrydinguk-coder/superpowers/issues
- **上游项目**: https://github.com/obra/superpowers
