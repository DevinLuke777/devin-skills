# 🇧🇷 巴西站市场知识（Mercado Livre Brasil / MLB）

> 本文件是双站点深度知识的拆分——SKILL.md 主文件只保留路由与规则，**详细背景知识见本文件**。模型按需读取，避免主文件膨胀。

---

## 站点标签

- **语言**：巴西葡萄牙语（pt-BR）
- **货币**：BRL（R$）
- **站点代号**：MLB（Mercado Livre Brasil）

## 平台基因

美客多全球最大市场、拉美最大电商。价格敏感、**分期购买（parcelamento）为刚需**、移动端 App 购物占比极高。

## 支付生态

- **Pix**（巴西央行即时支付，免手续费、秒到账）—— 已是巴西电商绝对主流支付方式
- Mercado Pago 信用卡 / 借记卡
- Boleto（银行缴费单，通常可分期）
- 消费金融由 Mercado Pago / Mercado Crédito 支撑

## 履约体系（Mercado Envíos）

- **Full（平台仓，类 FBA）**：货入 Mercado Livre 仓库，平台负责仓储 / 打包 / 配送 / 退换，解锁 Full 标识与「Receba em X dia(s)」时效承诺，排名加权最高、转化最优。
- **Flex（本地自配 + 上门揽收）**：货在卖家手里，订单产生后 Mercado Envíos 即时上门揽收配送，2-5 天达，免入仓，适合新品 / 低动量 / 退货率需控的品。
- **Cross-docking（集货转运）**：大批量从中国头程到美客多指定区域集货枢纽，平台再分拨配送，适合跨境卖家规模化铺货。
- **Standard（标准模式）**：卖家自选物流商发货并回填追踪号，须符合时效要求，适合尚未通过 Flex 门槛或大件商品。

## 信誉体系

- **Reputação**（正 / 中 / 负评价 + 取消率 + 纠纷 + 延迟率）
- 绿色信誉（MercadoLíder 准入门槛）
- **MercadoLíder**（绿色信誉 + 销量 / 活跃度达标，享专属顾问、流量加权）
- **Líder Gold / Líder Platinum**（更高销量门槛）
- 信誉直接影响搜索排名、Buy Box（「Comprado」）与活动报名资格

## Listing 类型

- **Premium**（高级版）：支持更多免息分期期数、视频、自定义详情模板、更强曝光、Ad 资格
- **Clásica**（经典版）：基础功能
- 同类目下 Premium 加权通常优于 Clásica

## 费用结构

- 类目佣金（按类目 10%-20% 上下浮动）+ Full 履约费（如使用 Full，仓储 + 配送）
- 具体数值 / 最新机制见 `references/rate-card.md`

## 广告体系

- **Mercado Ads**（Product Ads 按点击付费，出现在搜索与详情页）
- 须用巴西葡语关键词；ACOS 优化为运营常驻课题
- 实战打法详见 `references/mercado-ads-playbook.md`

## 大促日历

- **Black Friday**（11 月底，年度最大）→ **Cyber Monday**（随后周一）
- **Hot Sale**（约 4-5 月）
- **Dia das Mães** 母亲节（5 月第 2 周日）
- **Dia dos Namorados** 情人节（**6 月 12 日**，巴西专属大促）
- **Dia das Crianças** 儿童节（**10 月 12 日**，玩具 / 电子强节点）
- **Natal** 圣诞（12 月）

## 合规要点

- 跨境直发个人包裹走 **Remessa Conforme**（合规寄递计划）：
  - ≤ US$50：免关税，仅 17% ICMS
  - US$50–3000：60% 进口关税 + 17% ICMS
  - 美客多为主要认证平台之一
- 商业批量进口须 CNPJ + SISCOMEX 清关
- **产品认证**：
  - **ANATEL**（无线电 / 电信设备，如耳机、路由器、智能设备）
  - **INMETRO**（安全类，如电源、玩具）
  - **ANVISA**（化妆品 / 卫生用品）
- **数据隐私**：LGPD《通用数据保护法》
- 消费者 7 天无理由退货权（CDC）与平台「Compra Garantizada」买家保障机制
- 完整政策速查见 `references/rate-card.md`
