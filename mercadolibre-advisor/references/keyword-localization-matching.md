# 关键词本地化匹配规则（美客多巴西 / 墨西哥双站）

> 当用户用关键词去搜索、对比竞品、扩展词库、检查埋词覆盖时，必须做**本地化归一化**才能准确匹配。本文件说明为什么要做、怎么做、典型坑。

## 为什么需要本地化匹配

本地买家在搜索框输入时，**写法很不规范**：

- **不带重音**：`cesta`（不带 ~ 也不带 ã）、`pão`（不带 ã 也不带 ~）
- **单复数随便**：`cesta` / `cestas`、`pão` / `pães` 混用
- **大小写不管**：`CAIXA` / `Caixa` / `caixa` 都常见
- **拼写变体**：买家不一定用规范化名词

如果严格按"完整短语匹配"，会**漏掉 30%-50% 的真实搜索词和竞品 Listing**——典型的关键词可见度问题。

## 归一化规则（统一处理）

按以下三步对每个词 / 短语做归一化，得到可比较的"标准形式"：

### 1. 去重音 + 去变音符号

| 原字符 | 归一为 | 适用语言 |
|--------|--------|----------|
| `ã` `á` `â` | `a` | 葡 / 西 |
| `é` `ê` | `e` | 葡 / 西 |
| `í` | `i` | 葡 / 西 |
| `ó` `ô` `õ` | `o` | 葡 / 西 |
| `ú` | `u` | 葡 / 西 |
| `ç` | `c` | 葡（巴西） |
| `ñ` | `n` | 西 |

### 2. 转小写

所有字符转小写（消除 `CAIXA` / `Caixa` 差异）。

### 3. 简单单复数归一（西语 + 葡语）

| 语言 | 规则 | 示例 |
|------|------|------|
| **西语 ES** | 去 `-s` 或 `-es` | `cestas` → `cesta`<br>`audífonos` → `audífono` |
| **葡语 BR（一般）** | 去 `-s` 或 `-es` | `cestas` → `cesta`<br>`fones` → `fone` |
| **葡语 BR（-ões 类）** | 去 `-ões` → `-ão`（去重音后 `-oes` → `ao`） | `pães` → `paes` → `pao`<br>`opções` → `opcoes` → `opcao`<br>`limões` → `limoes` → `limao` |
| **葡语 BR（-ães 类）** | 去 `-ães` → `-ão`（去重音后 `-aes` → `ao`） | `mães` → `maes` → `mao`<br>`pães` 同上 |

**简化版实现思路**：去重音 + 去 `-s/-es` + 检查 `-oes/-aes` 后缀。

## 实战示例：用户输入 `cesta de pão`

### 归一化过程

```
原始:        cesta de pão
去重音:      cesta de pao
小写:        cesta de pao（已小写）
单复数:      cesta de pao（已单数）
最终 key:    "cesta de pao"
```

### 匹配候选（关键词库 / 竞品标题）

| 候选 | 归一化 | 匹配结果 |
|------|--------|---------|
| `cestas de pães` | `cesta de pao` | ✅ 完全匹配 |
| `Cesta Para Pão` | `cesta para pao` | ✅ 匹配（含 `para` 不影响核心词） |
| `Cestas Organizadoras Para Pães` | `cesta organizadora para pao` | ✅ `cesta` + `pão` 同时匹配 |
| `Porta Pão de Mesa` | `porta pao de mesa` | ⚠️ 部分匹配（含 `pão`，但不是 `cesta`） |

## 实战应用：找出"漏掉的高流量词"

**最实用场景**：用户已埋了一组词，怎么检查有没有漏掉高流量词？

### 步骤

1. **用户已埋词归一化集合**（自己埋的清单归一化）：
   ```
   {"cesta de pao", "tampa de acrilico", "vime sintetico", ...}
   ```

2. **目标站点高流量搜索词归一化集合**（从热搜词反查 / 广告工具 / 搜索补全收集）：
   ```
   {"cesta de pao", "cestas de paes", "porta pao", "cesto de paes",
    "suporte para pao", "cesta para mesa", "porta pao de mesa", ...}
   ```

3. **做差集**（高流量搜索词 - 已埋词）：
   ```
   {"porta pao", "cesto de paes", "suporte para pao", "cesta para mesa", ...}
   ```

4. **结果**：用户漏了 `porta pao`、`cesto de paes`、`suporte para pao`、`cesta para mesa` 等高流量词 → **应补入埋词清单**

### 这正是用户反馈的"漏掉高流量词"问题的解法

## 实现要点（伪代码）

```python
import unicodedata

def normalize(text):
    """本地化归一化：去重音 + 小写 + 简单单复数归一"""
    text = text.lower().strip()
    # 去重音（NFKD 分解后过滤组合标记）
    nfkd = unicodedata.normalize('NFKD', text)
    text = ''.join(c for c in nfkd if not unicodedata.combining(c))
    # 简单单复数归一（西/葡通用）
    if text.endswith('s'):
        text = text[:-1]
    if text.endswith('oes') or text.endswith('aes'):
        text = text[:-3] + 'ao'  # 葡语 ões/ães 类复数 → 单数
    return text

def match_coverage(user_keywords, market_keywords):
    """找出用户已埋词 vs 市场高流量词的差集"""
    user_norm = {normalize(k): k for k in user_keywords}
    market_norm = {normalize(k): k for k in market_keywords}
    matched = set(user_norm) & set(market_norm)
    missing = set(market_norm) - set(user_norm)
    return {
        'matched': [user_norm[k] for k in matched],
        'missing': [market_norm[k] for k in missing],  # 用户没埋的高流量词
    }
```

## 美客多场景下的注意事项

1. **同义词 ≠ 复数**：本规则只处理"同一词的不同写法"，**不处理同义词**（cesta ≠ cesto ≠ cesto de pão）。同义词需手工扩展或用关键词工具补充。
2. **变音符号不是匹配差异**：巴西葡语输入法不带重音是常态（搜 `cesta` 而不是 `cestá`），归一化必须支持。
3. **跨西语变体**：墨西哥西语 vs 阿根廷西语，单复数规则一致；但部分词形不同（vosotros vs ustedes），跨站需要确认。
4. **专有名词 / 品牌名**：不做去重音和单复数归一（如 `Mercado Livre`、`Mercado Pago` 保留原样）。
5. **产品型号 / SKU**：不做归一（如 `iPhone 13 Pro Max` 原样保留）。

## 与现有 skill 的衔接

- **关键词生成**（Skills 八）→ 输出关键词后，用本规则归一化，跟竞品 / 搜索词匹配
- **埋词方案**（Skills 八）→ 用户已埋词归一化后，与目标站点高流量搜索词做**差集**，**找出漏掉的高流量词**
- **竞品分析** → 用归一化对比竞品标题与自己 Listing 标题的覆盖差异，找出"竞品有、自己没有"的关键词
- **标题双版本** → 写传统链接 / 目录链接标题时，归一化后核对"是否覆盖了核心关键词的所有本地化变体"
