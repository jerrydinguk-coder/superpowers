# Team Installation Guide / 团队安装指南

## For Claude Code Users / Claude Code 用户

### Method 1: Direct GitHub Installation (Recommended)
### 方法一：直接从 GitHub 安装（推荐）

```bash
# Add the custom marketplace
# 添加自定义市场
claude /plugin install superpowers@jerrydinguk-coder/superpowers

# Or use the full GitHub URL
# 或使用完整的 GitHub URL
claude /plugin install superpowers --source https://github.com/jerrydinguk-coder/superpowers.git
```

### Method 2: Manual Marketplace Registration
### 方法二：手动注册市场

1. Edit `~/.claude/plugins/known_marketplaces.json`:
1. 编辑 `~/.claude/plugins/known_marketplaces.json`：

```json
{
  "superpowers-jerrydinguk": {
    "source": {
      "source": "github",
      "repo": "jerrydinguk-coder/superpowers"
    }
  }
}
```

2. Then install:
2. 然后安装：

```bash
claude /plugin install superpowers@superpowers-jerrydinguk
```

---

## For OpenCode Users / OpenCode 用户

```bash
# Clone the repository
# 克隆仓库
git clone https://github.com/jerrydinguk-coder/superpowers.git ~/.config/opencode/superpowers

# Or add as a plugin source
# 或添加为插件源
opencode plugin add https://github.com/jerrydinguk-coder/superpowers.git
```

---

## Updating / 更新

### Claude Code
```bash
claude /plugin update superpowers
```

### OpenCode
```bash
cd ~/.config/opencode/superpowers && git pull
```

---

## Verifying Installation / 验证安装

After installation, restart your CLI and test:
安装后，重启 CLI 并测试：

```bash
# In Claude Code, try invoking a skill
# 在 Claude Code 中，尝试调用一个技能
/brainstorming
```

---

## Version Information / 版本信息

- **Current Version / 当前版本**: 4.1.0
- **Based on / 基于**: obra/superpowers v4.0.3
- **Repository / 仓库**: https://github.com/jerrydinguk-coder/superpowers

---

## Support / 支持

For issues or questions, please open an issue on GitHub:
如有问题，请在 GitHub 上提交 issue：

https://github.com/jerrydinguk-coder/superpowers/issues
