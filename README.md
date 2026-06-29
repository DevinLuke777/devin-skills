# Claude Code Skills

我个人编写和维护的 [Claude Code](https://claude.ai) Skill 集合。

## 目录

| Skill | 用途 |
|-------|------|
| [prompt-architect](prompt-architect/README.md) | 高级提示词架构工具——将模糊想法转化为结构化 AI 指令 |
| [code-assistant-skill](code-assistant-skill/README.md) | Windows 编程助手——零基础用户的自动化方案选型与代码产出 |
| [cross-border-ecommerce-advisor](cross-border-ecommerce-advisor/README.md) | 跨境电商全平台运营顾问——Amazon / noon / Mercado Libre / Walmart / Takealot |

## 文件说明

每个 Skill 文件夹内包含：

| 文件 | 用途 |
|------|------|
| `SKILL.md` | Claude Code 的 Skill 主体（工具版），安装至 `.claude/skills/` 后自动触发 |
| `prompt-for-chat.md` | 同内容的聊角色版，可复制到 ChatGPT / claude.ai / DeepSeek 等直接使用 |
| `README.md` | 每个 Skill 的详细说明与使用方式 |

## 安装

将对应 Skill 的文件夹复制到 `.claude/skills/` 目录下即可：

```bash
cp -r prompt-architect ~/.claude/skills/
```

重启 Claude Code 后自动生效。

## 许可

MIT
