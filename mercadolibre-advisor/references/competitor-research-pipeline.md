# 竞品调研 → 标题/描述生成流水线（美客多巴西 / 墨西哥）

> 端到端流水线：从竞品数据 / 类目热搜词 / 产品特征出发，**自动生成关键词矩阵 + 双版本标题 + 描述 + FAQ**。
> 本文件把整套流程系统化，含 5 层验证、7 步执行、Python 代码模板、4 红线合规校验。

---

## 一、触发条件

用户给以下任任时**自动触发**本流水线：
- 1 个或多个竞品 xlsx / xls（热搜词反查导出）
- 1 个类目热搜词文件（xls / xlsx）
- 产品特征清单（材质 / 卖点 / 规格）
- 或直接给一个 MLB/Mercado Libre 产品链接 / ID（高级场景）

典型用户话术：
- "帮我分析这几个竞品，给出标题候选"
- "这份热搜词反查 + 类目热搜词，整理出关键词矩阵"
- "这个产品的 Listing 怎么写？"

---

## 二、5 层数据验证方法论

### 第 1 层：源文件解码与结构校验

| 数据源 | 格式 | 验证动作 |
|--------|------|----------|
| `MLBxxxxx_热搜词反查.xlsx` | xlsx | `openpyxl.load_workbook` 读所有 sheet + row，确认列数一致 |
| `类目热搜词.xls` | **伪装成 xls 的 HTML** | 嗅探文件头 → `decode('gbk', errors='replace')` → `re.findall(r'<td[^>]*>([^<]+)</td>')` 抽取 |

**关键判断点**：**永远不要相信文件后缀**。`xlrd` 报 `Unsupported format, or corrupt file: Expected BOF record; found b'<html xm'` → 这是 HTML 伪装 xls 的铁证。

### 第 2 层：数据字段清洗

```python
hot_search       → kw.strip() + 去重
hot_search_cn    → 多为中文释义，仅作辅助参考
traffic_share    → "2.03%" → float(2.03)
sales_30d        → int()
competitor_count → int()（部分可能是文本 "—"）
```

### 第 3 层：归一化（最关键步骤）

**目的**：本地化买家写法不规范（去重音 / 单复数 / 大小写 / 拼写错误），防止漏匹配。

```python
def normalize(kw: str) -> str:
    """去重音 + 小写 + 简单单复数归一（详见 keyword-localization-matching.md）"""
    import unicodedata, re
    s = kw.lower()
    # 去重音
    nfkd = unicodedata.normalize('NFKD', s)
    s = ''.join(c for c in nfkd if not unicodedata.combining(c))
    s = re.sub(r'[^a-z0-9 ]', ' ', s)
    s = re.sub(r'\s+', ' ', s).strip()
    # 简单单复数归一（不短于 5 字符）
    if len(s) > 4 and s.endswith('s'):
        s = s[:-1]
    return s
```

**关键去重案例**（这步直接关系搜索覆盖）：
- `pomo giratorio` / `pomo giratório` / `pomo-giratorio` → 同一词
- `manopla` / `manopala`（拼错变体）→ 同一词
- `manipulo` / `manípulo` → 同一词

更完整的归一化规则（西语 / 葡语特殊复数 -ões/-ães 类）见 `keyword-localization-matching.md`。

### 第 4 层：交叉验证（三方对比）

```
                竞品 A    竞品 B    竞品 C    类目热搜
pomo volante       ✓        ✓        ✓        ✓       ← 强信号词（4 源均出现）
manopala volante   ✓        ✓        -        ✓       ← 拼写变体（3 源出现）
tomóvi             -        -        ✓        -       ← 噪音词（仅 1 源出现）
```

**筛选规则**：
- 出现次数 **≥ 2 个数据源** + 流量占比累计 **> 1%** → **进关键词矩阵**
- 否则 → **降级为噪音词，丢弃**

### 第 5 层：分类规则（关键词矩阵核心）

按**优先级**分类：

| 优先级 | 标签 | 匹配规则（关键词含以下词根） |
|--------|------|-------------------------------|
| 1 | **痛点 / 转化词** | `economia`、`reduz`、`esforc`、`fadiga`、`conforto`、`auxiliar`、`ajuda` |
| 2 | **属性词** | `360`、`universal`、`preto`、`rolamento`、`remov`、`anti`、`escorr`、`resistente` |
| 3 | **场景词** | `caminhao`、`carro`、`trator`、`pcD`、`isencao`、`baliza`、`manobra` |
| 4 | **核心词** | `pomo`、`manopla`、`manipulo`、`bola`、`volante`、`giratorio`、`asaflex` |
| 5 | **长尾 / 其他** | 未匹配以上 → 兜底类 |

排序：按 `[class_priority, -traffic_share, -appearances]` 排序。

---

## 三、7 步文件导出模板（流水线）

```
┌─────────────────┐   ┌─────────────────┐   ┌─────────────────┐
│ 1. 读取源数据    │ → │ 2. 聚合归一化    │ → │ 3. 分类打标      │
│   (xlsx/xls)    │   │   (normalize)   │   │   (classify)    │
└─────────────────┘   └─────────────────┘   └─────────────────┘
                                                      ↓
┌─────────────────┐   ┌─────────────────┐   ┌─────────────────┐
│ 7. 输出 Markdown │ ← │ 6. 合规校验      │ ← │ 5. 字符校验      │
│   (描述+对照)    │   │   (forbidden)   │   │   (≤60/≤200)    │
└─────────────────┘   └─────────────────┘   └─────────────────┘
                              ↑
                  ┌─────────────────┐
                  │ 4. 生成候选标题  │
                  │   (pack vars)
```

### 步骤 1：读取源数据
```python
files = {'a': 'competitor_a.xlsx', 'b': 'competitor_b.xlsx', ...}
data = {}
for label, fp in files.items():
    if fp.endswith('.xlsx'):
        wb = openpyxl.load_workbook(fp)
        # ... 读 sheet，提取热搜词
    elif fp.endswith('.xls') or 'html' in open(fp, 'rb').read(64).decode('latin1', errors='replace').lower():
        # 嗅探 HTML 伪装
        raw = open(fp, 'rb').read().decode('gbk', errors='replace')
        rows = re.findall(r'<td[^>]*>([^<]+)</td>', raw)
        # ...
```

### 步骤 2：聚合归一化
```python
from collections import defaultdict
agg = defaultdict(lambda: {'norm': '', 'variants': set(), 'sources': set(),
                            'sales_30d': 0, 'traffic_share': 0.0})
for source, kws in data.items():
    for kw, sales, traffic in kws:
        norm = normalize(kw)
        agg[norm]['norm'] = norm
        agg[norm]['variants'].add(kw)
        agg[norm]['sources'].add(source)
        agg[norm]['sales_30d'] += sales
        agg[norm]['traffic_share'] += traffic
```

### 步骤 3：分类打标
```python
def classify(norm: str) -> str:
    if any(x in norm for x in ['economia','reduz','esforc','fadiga','conforto','auxiliar','ajuda']):
        return '痛点/转化词'
    if any(x in norm for x in ['360','universal','preto','rolamento','remov','anti','escorr','resistente']):
        return '属性词'
    if any(x in norm for x in ['caminhao','carro','trator','pcD','isencao','baliza','manobra']):
        return '场景词'
    if any(x in norm for x in ['pomo','manopla','manipulo','bola','volante','giratorio','asaflex']):
        return '核心词'
    return '长尾'
```

### 步骤 4：生成候选标题（分 4 组）

| 组 | 字符限制 | 数量 |
|----|---------|------|
| 单只装传统链接 | ≤60 | 6 |
| 单只装目录链接 | ≤200 | 5 |
| 双装传统链接 | ≤60 | 5 |
| 双装目录链接 | ≤200 | 5 |

每条标题从关键词矩阵里按优先级 + 字符预算"打包"（pack）变量。

### 步骤 5：字符校验
```python
def validate(title: str, limit: int) -> dict:
    return {
        'chars': len(title),
        'within_limit': len(title) <= limit,
    }
```

### 步骤 6：合规校验（4 项硬红线）

```python
FORBIDDEN = {
    'promo_words':  ['gratis', 'melhor', 'barato', 'oferta', '100%', 'original', '#1'],
    'decor_chars':  ['- ', '• ', '* ', '> ', '| ', '│ ', '┃ ', '1.', '①'],
    'emojis':       ['★', '✅', '™', '®', '⚠️'],
    'product_mismatch': [],  # 动态：rolamento 在无轴承产品中
}

def compliance_check(title: str, product_features: dict) -> list:
    issues = []
    for cat, words in FORBIDDEN.items():
        if cat == 'product_mismatch': continue
        for w in words:
            if w in title.lower():
                issues.append(f'禁词/装饰: {w}')
    # 动态产品特征校验
    for feat_key, expected in product_features.items():
        if feat_key in title.lower() and expected not in title.lower():
            issues.append(f'产品特征不符: {feat_key} 出现但缺少 {expected}')
    return issues
```

**4 红线**：
- ❌ 无禁词：`grátis / melhor / barato / oferta / 100% / original / #1 / envío gratis`
- ❌ 无装饰前缀：`-`、`•`、`*`、`>`、`|`、`│`、`┃`、`1.`、`①`
- ❌ 无 Emoji：★、✅、™、®、⚠️
- ❌ 无产品特征不符词（动态校验）

### 步骤 7：输出 Markdown + CSV

| 输出 | 编码 | 用途 |
|------|------|------|
| 关键词矩阵 | CSV **utf-8-sig** | Excel 直接打开中文不乱码 |
| 单只装 / 双装标题 | .md | 候选方案对照 |
| 描述 + FAQ | .md | 套用 8 章顺序 + 6 条强制规则 + FAQ 模板 |

---

## 四、与现有 skill 的衔接

| 流水线步骤 | 复用现有 skill 模块 |
|-----------|------------------|
| 步骤 3 归一化 | `keyword-localization-matching.md`（更完整的归一化规则） |
| 步骤 4 候选标题 | `keyword-embedding-guide.md`（埋词方案） |
| 步骤 6 合规校验的红线部分 | `brazil-listing-guide.md` / `mexico-listing-guide.md` 的 6 条强制规则 + 美客多埋词红线 |
| 步骤 7 输出描述 | `brazil-listing-guide.md` 的 6 条强制规则 + 7. FAQ 自问自答章节 |
| 字符限制（≤60 / ≤200） | SKILL.md Skills 二的标题双版本规则 |

---

## 五、注意事项

- **永远不要相信文件后缀** —— 必须嗅探文件头判断真实格式
- **归一化是核心** —— 这步直接决定搜索覆盖质量
- **三方对比筛选强信号词** —— 出现次数 ≥ 2 源 + 流量 > 1% 才进关键词矩阵
- **4 红线必须全部通过** —— 任一项不过则该标题作废
- **输出 CSV 用 utf-8-sig 编码** —— 保证 Excel 打开中文不乱码

---

## 六、Skill 触发条件

当用户提到以下任何场景，**自动调用本流水线**：
- "竞品调研 / 分析竞品"
- "MLB ID / 热搜词反查 / 类目热搜词"
- "标题候选 / Listing 关键词"
- 直接给 xlsx / xls 文件让 AI 分析

执行流程：按 7 步流水线依次进行，最后输出关键词矩阵 CSV + 标题 / 描述 .md。

---

## 七、输出动作强约束（不要询问"要不要导出"）

完成 7 步流水线后，**自动输出**以下文件，**不询问用户**"要不要导出"：

| 输出文件 | 编码 / 格式 | 内容 |
|---------|------------|------|
| 关键词矩阵 | CSV（**utf-8-sig**） | 归一化关键词 / traffic_share / sales_30d / 分类标签 / variants |
| 候选标题 | .md | 单只装 / 双装 × 传统 ≤60 / 目录 ≤200 = 4 组 |
| 描述 + FAQ | .md | 套用 8 章顺序 + 6 条强制规则 + FAQ 模板 |

**输出位置**：当前工作目录的子目录 `mercadolibre-output/<产品名>/` 下。

**错误示范**（❌ 禁止）：
> "要不要我导出成 CSV 文件？"
> "你想要我把结果保存到文件吗？"

**正确做法**（✅ 必须）：
> "已生成文件：
> - `mercadolibre-output/pomo-volante/keywords.csv`
> - `mercadolibre-output/pomo-volante/titles.md`
> - `mercadolibre-output/pomo-volante/description.md`"

**理由**：用户使用本 skill 的预期就是"做完直接给文件"，询问"要不要导出"既打断工作流又增加摩擦。