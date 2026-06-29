# Windows Code Assistant

面向零基础 Windows 用户的编程助手 Skill。自动根据任务需求选择最省力的工具（Python / 影刀RPA / Batch / PowerShell / VBA / SQL），产出的代码附带详细中文注释与运行环境自检。

## 适用场景

- 网页数据采集（爬虫、API 调用）
- Excel / CSV 批量处理与清洗
- 文件批量重命名、复制、归档
- 影刀RPA 流程设计与 Python 代码块
- 系统管理脚本（PowerShell / Batch）
- 数据库查询与报表统计

## 双版本说明

本项目提供两个版本，按场景选用：

| 版本 | 文件 | 适用场景 |
|------|------|----------|
| **Skill 版** | `SKILL.md` | 编程助手 Skill 功能，自动触发选型 |
| **Chat 版** | `prompt-for-chat.md` | ChatGPT / claude.ai / DeepSeek 等聊天 AI，作为 System Prompt 或首条消息使用 |

Chat 版引入了角色「程启」——一位实用主义编程顾问。在聊天场景中，有名有姓的角色能更好地维持「务实、耐心、零基础友好」的交互风格一致性。

## 安装方式

### 方式一：AI 助手安装

把以下内容发给你的 AI 编程助手即可：

> 帮我安装这个 skill：https://github.com/DevinLuke777/devin-skills 里的 `code-assistant-skill`

### 方式二：手动安装

1. 下载本仓库到本地
2. 将 `code-assistant-skill` 整个文件夹复制到你使用的 AI 编程工具的 skills 目录下
3. 重启工具即可生效

## 使用方式

在对话中输入（Skill 会自动触发）：

```
/code-assistant-skill 帮我把这个文件夹里所有 .txt 文件合并成一个 .csv
```

或直接描述你的编程需求（Skill 会自动触发）：

```
我需要从这 500 个 Excel 文件里提取 A 列去重后汇总到一个新表里
```

## 文件结构

```
code-assistant-skill/
├── SKILL.md                        # Skill 主体（工具版），含完整工作流与输出规范
├── prompt-for-chat.md              # Chat 版（角色版：「程启」），可复制到各类聊天 AI 中直接使用
├── references/
│   └── tool-capabilities.md        # 六大工具详细能力参考
└── README.md                       # 本文件
```

## 六大工具路线

| 工具 | 后缀 | 最佳场景 |
|------|------|---------|
| **Python** | `.py` | 复杂数据处理、API 调用、网页爬虫 |
| **影刀RPA** | 流程文件 | 桌面 GUI 操作、无 API 的网页自动化 |
| **Batch** | `.bat` | 简单文件批量操作（一行命令级别） |
| **PowerShell** | `.ps1` | Windows 系统管理、中复杂度文本处理 |
| **VBA** | `.xlsm` | Excel/Office 深度操作 |
| **SQL** | `.sql` | 数据库查询与数据聚合 |

## 许可证

MIT License

---

[← 返回根目录](../README.md)
