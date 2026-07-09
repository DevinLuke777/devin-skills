# multi-lang-translator

> 通用语境自适应翻译器 — 多语言高保真翻译 / 地区变体适配 / 绝对零冗余 / 自动中文对照

## 这是什么？

`multi-lang-translator` 是一个高精度翻译 Skill，核心特色：
- **绝对零冗余**：只输出译文，不附加任何解释或元评论
- **地区变体适配**：严格区分美式/英式英语、巴西/欧洲葡语等
- **自动中文对照**：非中文译文自动附带中文版本供校验
- **占位保护**：代码变量、URL、品牌名等元素保持原样不翻译

## 适用场景

- 需要将产品标题/描述翻译为多国语言（电商运营场景）
- 技术文档多语言本地化，保留代码与标记不被破坏
- 品牌文案需精准适配地区变体（比如巴西葡语 vs 欧洲葡语）
- 需要纯译文输出，不接受任何额外解释或元信息

## 安装

### 方式一：AI 助手安装

把以下内容发给你的 AI 编程助手即可：

> 帮我安装这个 skill：https://github.com/DevinLuke777/devin-skills 里的 `multi-lang-translator`

### 方式二：手动安装

1. 下载本仓库到本地
2. 将 `multi-lang-translator` 整个文件夹复制到你使用的 AI 编程工具的 skills 目录下
3. 重启工具即可生效

## 双版本说明

| 版本 | 文件 | 适用场景 |
|------|------|----------|
| **Skill 版** | `SKILL.md` | 编程助手 Skill 功能，自动触发 |
| **Chat 版** | `prompt-for-chat.md` | ChatGPT / claude.ai / DeepSeek 等聊天 AI，作为 System Prompt 或首条消息使用 |

## 文件说明

| 文件 | 用途 |
|------|------|
| `SKILL.md` | Skill 主体（工具版），含完整工作流与输出模板 |
| `prompt-for-chat.md` | Chat 版（角色版），可复制到各类聊天 AI 中直接使用 |
| `README.md` | 本文件，项目说明 |

## 许可

MIT License

---

[← 返回根目录](../README.md)
