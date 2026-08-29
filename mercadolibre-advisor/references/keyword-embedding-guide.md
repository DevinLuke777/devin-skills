# 关键词埋词实战指南（美客多巴西 / 墨西哥双站）

> 本文件系统化整理"埋词"方法论——从挖词到分配到验证的完整流程。配套 SKILL.md Skills 八使用。

## 一、什么是埋词

在 Listing 的**标题、属性、描述、图片**等位置策略性地嵌入搜索关键词，让平台搜索算法识别商品与买家搜索意图的匹配，提升曝光与精准度。

美客多埋词的两个特殊点：
- **葡语=巴西、西语=墨西哥**——语言必须按站点对应，不混用
- **不超字符限制**——传统链接标题 ≤60 字符，目录链接标题 ≤200 字符

## 二、挖词（关键词来源）

按优先级：

1. **热搜词反查导出**（如 `MLB*_热搜词反查.xlsx`）—— 买家真实搜词，按曝光 / 点击 / 转化排序，最有价值
2. **同类目竞品 Listing 标题与属性** —— 同类目卖得好的商品，他们埋了什么词
3. **Mercado Ads 关键词工具** —— 投放时系统给的搜索词建议（投放阶段必须做）
4. **平台搜索下拉补全** —— 在搜索框输入词根，看自动补全的高频词
5. **同义词扩展** —— 参考 `brazil-listing-guide.md` / `mexico-listing-guide.md` 的「本地用词对照」

| 中文 | 🇧🇷 巴西葡语 | 🇲🇽 墨西哥西语 |
|------|-------------|----------------|
| 蓝牙耳机 | fone bluetooth / fone de ouvido sem fio / fone tws / headset | audífonos bluetooth / audífonos inalámbricos / audífonos tws / auriculares |
| 运动鞋 | tênis | tenis |
| 手机 | celular / smartphone | celular / smartphone |
| 数据线 | cabo / cabo carregador | cable / cable cargador |

## 三、关键词分类

收集 20-30 个候选关键词后，按角色分类：

| 类别 | 特征 | 用法 |
|------|------|------|
| **核心词** | 大流量、精准匹配 | 标题前置位置 |
| **长尾词** | 小流量、高转化 | 目录标题中段、描述 |
| **同义词 / 近义词** | 覆盖搜索变体 | 目录标题、属性字段、描述 |

## 四、分配（按位置权重）

| 位置 | 权重 | 容量 | 写法 |
|------|------|------|------|
| **传统链接标题** | ⭐⭐⭐⭐⭐ | ≤60 字符 | 核心词 1-2 + 卖点 1-2，前置最关键词 |
| **目录链接标题** | ⭐⭐⭐⭐ | ≤200 字符 | 核心 + 长尾 + 同义词 + 属性 + 卖点，尽量填满 |
| **描述首段 / 卖点** | ⭐⭐⭐ | 自然融入 | 1-2 个核心词在首句自然出现 |
| **类目属性** | ⭐⭐⭐⭐ | 按字段 | 所有必填 / 推荐字段全填（含关键词变体） |
| **图片文件名 / alt** | ⭐ | 文件命名 | 葡语 / 西语含关键词 |

## 五、美客多埋词红线

### 禁词（绝不写入标题 / 描述）

| 类型 | 🇧🇷 巴西 | 🇲🇽 墨西哥 |
|------|---------|----------|
| 最好 | `o melhor` | `el mejor` |
| 便宜 | `barato` | `barato` |
| 免费 / 免运费 | `grátis`、`frete grátis`（入标题） | `gratis`、`envío gratis`（入标题） |
| 排名 | `#1`、`número 1` | `#1`、`número 1` |
| 夸大 | `100% original`、`o mais vendido`、`o único` | `100% original`、`el más vendido`、`el único` |

免运费靠平台开关（Mercado Envíos），不要在标题里写 `frete grátis / envío gratis` 字样。

### 格式红线

- ❌ **不堆砌**：重复同一关键词（"fone fone fone"）
- ❌ **不用 Emoji / 特殊符号**：★ ✅ ⚠️ ™ ® 等
- ❌ **不超字符**：标题 ≤60 / ≤200
- ❌ **不写"BULLET n —"标注**
- ❌ **不写编号前缀**：`1.` / `1、` / `①`

### 格式正确

- ✅ 核心词前置（标题前 40-60 字符权重最高）
- ✅ 同义词 / 近义词覆盖（让买家不同搜索词都能找到）
- ✅ Title Case，每词首字母大写
- ✅ 关键词之间空格分隔
- ✅ 用当地消费者用词（巴西葡语 / 墨西哥西语，不混用）

## 六、Model 字段填词（双重角色）

Model 字段在美客多承担两个角色：

| 角色 | 作用 |
|------|------|
| **聚合值** | 让同型号不同变体（颜色 / 尺寸）聚合到同一 Listing 下 |
| **埋词位置** | 平台把 Model 字段作为搜索匹配信号 |

### 按场景填

| 场景 | Model 填法 | 示例（乒乓球套装） |
|------|-----------|---------------------|
| **A. 单一规格（无变体）** | 含核心关键词的产品描述性短语 | `Kit Ping Pong 2 Raquetes 6 Bolinhas Rede Retrátil` |
| **B. 多变体** | Model 简短通用；变体字段（Cor / Tamanho）填差异 | `Kit Ping Pong Completo`（Cor：Preto e Vermelho） |

**先问卖家**：这个品有几种颜色 / 尺寸 / 容量变体？

## 七、实战步骤（拿到一个产品后）

1. **挖词**：从热搜词反查 + 竞品标题 + 广告工具 + 平台搜索补全 + 同义词扩展 → 20-30 个候选关键词
2. **分类**：核心词 / 长尾词 / 同义词
3. **分配**：按位置权重分配到标题 / 目录标题 / 描述 / 属性 / 图片命名
4. **检查**：不超字符、不触红线、不堆砌、Title Case、用当地语言
5. **验证**：发布后用关键词在站内搜索，看能否找到自己的 Listing；查 Mercado Ads 关键词工具看搜索量

## 八、案例（蓝牙耳机，双站）

### 巴西站 🇧🇷

**挖词清单**：
- 核心：`fone bluetooth`、`fone TWS`
- 长尾：`fone de ouvido sem fio`、`fone bluetooth 5.3`、`fone tws ipx5`
- 同义词：`headset`

**分配**：

| 位置 | 内容 |
|------|------|
| 传统链接标题（≤60） | `Fone Bluetooth TWS 5.3 Sem Fio À Prova D'Água Estuche Carga` |
| 目录链接标题（≤200） | `Fone De Ouvido Bluetooth TWS 5.3 Sem Fio À Prova D'Água IPX5 Com Estuche De Carga Recarregável USB-C Controle Touch Redução De Ruído Até 30 Horas De Bateria Compatível Com Android E iPhone Preto` |
| 描述首句 | 含 "Bluetooth TWS 5.3 sem fio" 自然融入 |
| 属性 | Marca + Modelo + 颜色 + 蓝牙版本 + 防水等级 全部填齐 |
| 图片命名 | `fone-bluetooth-tws-5-3-ipx5-preto.jpg` |

### 墨西哥站 🇲🇽

**挖词清单**：
- 核心：`audífonos bluetooth`、`audífonos TWS`
- 长尾：`audífonos bluetooth 5.3`、`audífonos inalámbricos IPX5`
- 同义词：`auriculares`

**分配**：同样逻辑，西语版用墨西哥本地用词。

## 九、埋词效果监测

- **搜索结果页**：发布后用 5-10 个核心关键词在站内搜，看是否能搜到自己的 Listing
- **Mercado Ads 后台**：看各关键词的曝光 / 点击 / 转化 / CPC
- **Listing 流量报告**：观察自然流量（Mercado Ads 之外）是否随埋词调整而提升
- **定期复盘**：每 2-4 周根据表现调整关键词列表
