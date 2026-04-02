---
name: ckm:design-system
description: Token架构、组件规范和幻灯片生成。三层tokens（primitive→semantic→component）、CSS变量、间距/排版规范、组件规范、战略性幻灯片创建。用于设计tokens、系统化设计、品牌合规的演示文稿。
argument-hint: "[component or token]"
license: MIT
metadata:
  author: claudekit
  version: "1.0.0"
---

<!--
  翻译说明：
  本文件为设计系统技能文档的中文翻译版。
  原文：Design System - Token architecture, component specifications, systematic design, slide generation.
  翻译：设计系统 - Token架构、组件规范、系统化设计、幻灯片生成。
-->

# 设计系统

Token架构、组件规范、系统化设计、幻灯片生成。

## 使用场景

- 设计Token创建
- 组件状态定义
- CSS变量系统
- 间距/排版规范
- 设计到代码的交接
- Tailwind主题配置
- **幻灯片/演示文稿生成**

## Token架构

加载：`references/token-architecture.md`

### 三层结构

```
原始层（原始值）
       ↓
语义层（用途别名）
       ↓
组件层（组件特定）
```

**示例：**
```css
/* 原始层 */
--color-blue-600: #2563EB;

/* 语义层 */
--color-primary: var(--color-blue-600);

/* 组件层 */
--button-bg: var(--color-primary);
```

## 快速开始

**生成Tokens：**
```bash
node scripts/generate-tokens.cjs --config tokens.json -o tokens.css
```

**验证使用：**
```bash
node scripts/validate-tokens.cjs --dir src/
```

## 参考文档

| 主题 | 文件 |
|-------|------|
| Token架构 | `references/token-architecture.md` |
| 原始Tokens | `references/primitive-tokens.md` |
| 语义Tokens | `references/semantic-tokens.md` |
| 组件Tokens | `references/component-tokens.md` |
| 组件规范 | `references/component-specs.md` |
| 状态与变体 | `references/states-and-variants.md` |
| Tailwind集成 | `references/tailwind-integration.md` |

## 组件规范模式

| 属性 | 默认 | 悬停 | 激活 | 禁用 |
|----------|---------|-------|--------|----------|
| 背景 | primary | primary-dark | primary-darker | muted |
| 文字 | white | white | white | muted-fg |
| 边框 | none | none | none | muted-border |
| 阴影 | sm | md | none | none |

## 脚本

| 脚本 | 用途 |
|--------|---------|
| `generate-tokens.cjs` | 从JSON token配置生成CSS |
| `validate-tokens.cjs` | 检查代码中的硬编码值 |
| `search-slides.py` | BM25搜索 + 上下文推荐 |
| `slide-token-validator.py` | 验证幻灯片HTML的token合规性 |
| `fetch-background.py` | 从Pexels/Unsplash获取图片 |

## 模板

| 模板 | 用途 |
|--------|---------|
| `design-tokens-starter.json` | 具有三层结构的起始JSON |

## 集成

**与品牌集成：** 从品牌色彩/排版中提取原始值
**与ui-styling集成：** 组件Tokens → Tailwind配置

**技能依赖：** brand, ui-styling
**主要代理：** ui-ux-designer, frontend-developer

## 幻灯片系统

使用设计Tokens + Chart.js + 上下文决策系统生成品牌合规的演示文稿。

### 真相来源

| 文件 | 用途 |
|------|---------|
| `docs/brand-guidelines.md` | 品牌标识、声音、色彩 |
| `assets/design-tokens.json` | Token定义（primitive→semantic→component） |
| `assets/design-tokens.css` | CSS变量（在幻灯片中导入） |
| `assets/css/slide-animations.css` | CSS动画库 |

### 幻灯片搜索（BM25）

```bash
# 基础搜索（自动检测领域）
python scripts/search-slides.py "investor pitch"

# 领域特定搜索
python scripts/search-slides.py "problem agitation" -d copy
python scripts/search-slides.py "revenue growth" -d chart

# 上下文搜索（高级系统）
python scripts/search-slides.py "problem slide" --context --position 2 --total 9
python scripts/search-slides.py "cta" --context --position 9 --prev-emotion frustration
```

### 决策系统CSV

| 文件 | 用途 |
|------|---------|
| `data/slide-strategies.csv` | 15种幻灯片结构 + 情感弧线 + 火花线节奏 |
| `data/slide-layouts.csv` | 25种布局 + 组件变体 + 动画 |
| `data/slide-layout-logic.csv` | 目标 → 布局 + break_pattern标志 |
| `data/slide-typography.csv` | 内容类型 → 排版规范 |
| `data/slide-color-logic.csv` | 情感 → 色彩处理 |
| `data/slide-backgrounds.csv` | 幻灯片类型 → 图片类别（Pexels/Unsplash） |
| `data/slide-copy.csv` | 25种文案公式（PAS, AIDA, FAB） |
| `data/slide-charts.csv` | 25种图表类型及Chart.js配置 |

### 上下文决策流程

```
1. 解析目标/上下文
        ↓
2. 搜索 slide-strategies.csv → 获取策略 + 情感节奏
        ↓
3. 对于每张幻灯片：
   a. 查询 slide-layout-logic.csv → 布局 + break_pattern
   b. 查询 slide-typography.csv → 类型规范
   c. 查询 slide-color-logic.csv → 色彩处理
   d. 查询 slide-backgrounds.csv → 图片（如需要）
   e. 应用 slide-animations.css 中的动画类
        ↓
4. 使用设计Tokens生成HTML
        ↓
5. 使用 slide-token-validator.py 验证
```

### 模式打破（Duarte火花线）

高级演示文稿通过情感交替来保持参与度：
```
"是什么"（挫折感）↔ "可以是什么"（希望）
```

系统在1/3和2/3位置计算模式打破点。

### 幻灯片要求

**所有幻灯片必须：**
1. 导入 `assets/design-tokens.css` - 单一真相来源
2. 使用CSS变量：`var(--color-primary)`、`var(--slide-bg)` 等
3. 使用Chart.js绘制图表（不是纯CSS条形图）
4. 包含导航（键盘方向键、点击、进度条）
5. 内容居中对齐
6. 专注于说服/转化

### Chart.js集成

```html
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.1/dist/chart.umd.min.js"></script>

<canvas id="revenueChart"></canvas>
<script>
new Chart(document.getElementById('revenueChart'), {
    type: 'line',
    data: {
        labels: ['Sep', 'Oct', 'Nov', 'Dec'],
        datasets: [{
            data: [5, 12, 28, 45],
            borderColor: '#FF6B6B',  // 使用品牌珊瑚色
            backgroundColor: 'rgba(255, 107, 107, 0.1)',
            fill: true,
            tension: 0.4
        }]
    }
});
</script>
```

### Token合规性

```css
/* 正确 - 使用token */
background: var(--slide-bg);
color: var(--color-primary);
font-family: var(--typography-font-heading);

/* 错误 - 硬编码 */
background: #0D0D0D;
color: #FF6B6B;
font-family: 'Space Grotesk';
```

### 参考实现

具有所有功能的完整示例：
```
assets/designs/slides/claudekit-pitch-251223.html
```

### 命令

```bash
/slides:create "为ClaudeKit Marketing创建一个10页投资者路演"
```

## 最佳实践

1. 组件中永远不要使用原始hex值 - 始终引用tokens
2. 语义层支持主题切换（浅色/深色）
3. 组件Tokens支持组件级自定义
4. 使用HSL格式控制透明度
5. 记录每个token的用途
6. **幻灯片必须导入design-tokens.css并仅使用var()**
