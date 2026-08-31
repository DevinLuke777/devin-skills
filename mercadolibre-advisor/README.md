# mercadolibre-advisor

> 美客多（Mercado Libre）巴西·墨西哥双站点运营顾问 — 专注巴西站（MLB）与墨西哥站（MLM）深度运营决策支持

## 这是什么？

`mercadolibre-advisor` 是一个 Skill，为中国跨境卖家提供**美客多巴西站（MLB）与墨西哥站（MLM）双站点**的结构化运营决策支持。覆盖选品、Listing优化（葡语/西语）、Mercado Ads 广告投放、Mercado Envíos 物流履约（Full/Flex/Cross-docking）、定价与分期支付策略（Pix/parcelamento/meses sin intereses）、税务与合规（Remessa Conforme/ICMS/IVA/ANATEL/NOM）、大促策略（Black Friday/Buen Fin/Hot Sale）、双站点协同等全链路。

> 本 Skill 由 `cross-border-ecommerce-advisor`（全平台版）改写聚焦而来，目前**仅覆盖美客多巴西站与墨西哥站**。

## 适用场景

- 单站点运营问题诊断（如 "巴西站我的 Listing 转化率低怎么优化"）
- 双站点对比与战略决策（如 "同一个品在巴西和墨西哥应该怎么定价"）
- 物流履约模式选择（如 "美客多用 Full 还是 Flex"）
- Mercado Ads 广告投放与 ACOS 优化
- 付款与分期策略（巴西 Pix 折扣、墨西哥免息分期）
- 合规风险排查（认证、税务、平台红线）
- 多语文案产出（葡语/西语标题、卖点，外文 + 中文对照）

## 安装

### 方式一：AI 助手安装

> 帮我安装这个 skill：https://github.com/DevinLuke777/devin-skills 里的 `mercadolibre-advisor`

### 方式二：手动安装

1. 下载本仓库到本地
2. 将 `mercadolibre-advisor` 整个文件夹复制到你使用的 AI 编程工具的 skills 目录下
3. 重启工具即可生效

## 使用方式

在对话中直接表达需求即可自动触发，例如：

- "我在美客多巴西站卖家居用品，怎么提升转化率？"
- "美客多墨西哥站 Listing 标题怎么写？帮我出一组西语卖点"
- "巴西站用 Full 还是 Flex 发货？"
- "巴西站和墨西哥站，同一个品类先攻哪个？"
- "美客多巴西站跨境合规要做哪些认证？Remessa Conforme 是什么？"

Skill 会自动先锁定巴西站/墨西哥站，再给出针对性分析。

## 双版本说明

| 版本 | 文件 | 适用场景 |
|------|------|----------|
| **Skill 版** | `SKILL.md` | 编程助手 Skill 功能，自动触发 |
| **Chat 版** | `prompt-for-chat.md` | ChatGPT / claude.ai / DeepSeek 等聊天 AI，作为 System Prompt 或首条消息使用 |

## 双站点覆盖

| 站点 | 代码 | 语言 | 货币 | 年度最大节点 |
|------|------|------|------|--------------|
| **巴西站** | MLB | 巴西葡萄牙语 | BRL | Black Friday（11月底） |
| **墨西哥站** | MLM | 墨西哥西班牙语 | MXN | El Buen Fin（11月中旬） |

## 文件说明

| 文件 | 用途 |
|------|------|
| `SKILL.md` | Skill 主体（工具版），含巴西/墨西哥双站点深度知识与六步工作流 |
| `prompt-for-chat.md` | Chat 版（角色版），可复制到各类聊天 AI 中直接使用 |
| `README.md` | 本文件，项目说明 |
| `references/brazil-listing-guide.md` | 巴西站 Listing 写作参考（葡语，含本地用词对照 + 描述输出格式规范 + FAQ 品类候选库） |
| `references/mexico-listing-guide.md` | 墨西哥站 Listing 写作参考（西语，含本地用词对照 + 描述输出格式规范 + FAQ 品类候选库） |
| `references/brazil-market-knowledge.md` | 巴西站深度市场知识（支付/履约/信誉/合规/大促） |
| `references/mexico-market-knowledge.md` | 墨西哥站深度市场知识（支付/履约/信誉/合规/大促） |
| `references/site-comparison.md` | 双站点核心差异速查表（含跨站协同建议） |
| `references/rate-card.md` | 双站时效政策速查表（费率/税率/认证/时效，含官方核对入口） |
| `references/product-selection-filter.md` | 双站选品合规门槛过滤清单（低门槛/需认证/高风险分级） |
| `references/mercado-ads-playbook.md` | Mercado Ads 实操（新品冷启动、大促节奏、ACOS 优化、后台复盘） |
| `references/reduction-cancellation-returns.md` | 双站跨境降取消/退货率专项 |
| `references/keyword-embedding-guide.md` | 关键词埋词实战指南（挖词 / 分类 / 位置分配 / 双站红线 / 案例） |
| `references/keyword-localization-matching.md` | 关键词本地化匹配规则（去重音 + 单复数归一 + 找出漏掉的高流量词） |
| `references/competitor-research-pipeline.md` | 竞品调研 → 标题/描述生成流水线（5 层验证 + 7 步执行 + Python 模板 + 4 红线） |

## 许可

MIT License

---

[← 返回根目录](../README.md)