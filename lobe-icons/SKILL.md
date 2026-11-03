---
name: lobe-icons
version: 1.0.0
author: Hermes
license: MIT
description: Use when 给 AI 面板/网页加厂商或模型 logo 图标。
metadata:
  hermes:
    tags: [icons, design, ai, svg]
    related_skills: [popular-web-designs, logo-mascot-assets]
---

# Lobe Icons（AI 品牌图标库）

200+ 个 AI / LLM 厂商与模型的官方 logo，两种用法：**静态直链**（给单文件 HTML 用，最常走这条）和 **React 组件**（给前端项目用）。

## When to Use（什么时候用）

- 页面/面板要显示模型厂商图标（DeepSeek、OpenAI、Claude、通义、豆包……）
- 做模型选择器、Provider 列表、排行榜、AI 工具导航页
- 没人给 logo 文件，网上搜的是盗图/低清 —— 这个库是官方 SVG

## 用法一：直链（推荐给单文件 HTML）

不装任何依赖，直接 `<img>` 引用。国内优先用 **npmmirror（阿里云）**，GitHub raw 经常拉不动。

```html
<!-- SVG 单色 -->
<img src="https://registry.npmmirror.com/@lobehub/icons-static-svg/latest/files/icons/deepseek.svg" width="24">

<!-- SVG 彩色 -->
<img src="https://registry.npmmirror.com/@lobehub/icons-static-svg/latest/files/icons/deepseek-color.svg" width="24">

<!-- PNG（分 light/dark） -->
<img src="https://unpkg.com/@lobehub/icons-static-png@latest/light/openai-color.png" width="24">
<img src="https://unpkg.com/@lobehub/icons-static-png@latest/dark/openai-color.png" width="24">
```

命名规则：`{id}` 全小写；变体后缀 `-color`、`-text`、`-text-cn`、`-brand`、`-brand-color`，单色不带后缀。

GitHub 源（国内慢，备用）：
```
https://raw.githubusercontent.com/lobehub/lobe-icons/refs/heads/master/packages/static-svg/icons/{id}.svg
```

## 用法二：React 组件

```bash
npm install @lobehub/icons
```

```tsx
import { OpenAI, DeepSeek, ModelIcon, ProviderIcon } from '@lobehub/icons';

<DeepSeek size={24} />            // 单色，继承 currentColor
<DeepSeek.Color size={24} />      // 彩色
<DeepSeek.Text size={32} />       // 图标 + 文字
<DeepSeek.Avatar size={64} />     // 方形头像版

<ModelIcon model="deepseek-chat" size={24} />   // 传模型名自动匹配
<ProviderIcon provider="deepseek" size={28} type="color" />
```

用 `toc` 查有哪些图标、支持哪些变体：

```tsx
import { toc } from '@lobehub/icons';
toc.filter(i => i.group === 'provider').map(i => i.id);
```

## 坑

- **PNG/WebP 分 light 与 dark**，深色底用错目录会白底糊成一团；SVG 不分，优先 SVG。
- `<img>` 引 SVG 无法改颜色（CSS fill 不生效）。深色底要白图标时，用单色 SVG + `filter: invert(1)`，或改用 `mask-image`。
- `-text` 变体是「图标+字」横排组合，当纯图标用会变形。
- npm 包是 ESM + tree-shakable，别整包 require。

## 找图标 id

全量 id 与品牌色见 `references/providers.md`（ModelProvider 枚举），或浏览 https://lobehub.com/icons。
常见：`openai`、`anthropic`、`deepseek`、`gemini`、`qwen`、`zhipu`、`moonshot`、`baidu`、`bytedance`、`alibabacloud`。

## 相关包

| 包 | 用途 |
|---|---|
| `@lobehub/icons` | React 组件 |
| `@lobehub/icons-static-svg` | 纯 SVG 文件 |
| `@lobehub/icons-static-png` | PNG（light/dark） |
| `@lobehub/icons-static-webp` | WebP（light/dark） |
