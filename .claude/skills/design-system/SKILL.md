---
name: ckm:design-system
description: Token 架构、组件规范和幻灯片生成。三层 Token（Primitive→Semantic→Component）、CSS 变量、间距/字体系统、组件规范、策略性幻灯片创建。适用于设计 Token、系统化设计、品牌合规演示文稿。
argument-hint: "[组件或 Token]"
license: MIT
metadata:
  author: claudekit
  version: "1.0.0"
---

# 设计系统

Token 架构、组件规范、系统化设计、幻灯片生成。

## 何时使用

- 设计 Token 创建
- 组件状态定义
- CSS 变量系统
- 间距/字体系统
- 设计到代码的交接
- Tailwind 主题配置
- **幻灯片/演示文稿生成**

## Token 架构

参考：`references/token-architecture.md`

### 三层结构

```
Primitive（原始值）
       ↓
Semantic（语义别名）
       ↓
Component（组件特定）
```

**示例：**
```css
/* Primitive */
--color-blue-600: #2563EB;

/* Semantic */
--color-primary: var(--color-blue-600);

/* Component */
--button-bg: var(--color-primary);
```

## 快速开始

**生成 Token：**
```bash
node scripts/generate-tokens.cjs --config tokens.json -o tokens.css
```

**验证使用情况：**
```bash
node scripts/validate-tokens.cjs --dir src/
```

## 参考文档

| 主题 | 文件 |
|-------|------|
| Token 架构 | `references/token-architecture.md` |
| Primitive Token | `references/primitive-tokens.md` |
| Semantic Token | `references/semantic-tokens.md` |
| Component Token | `references/component-tokens.md` |
| 组件规范 | `references/component-specs.md` |
| 状态与变体 | `references/states-and-variants.md` |
| Tailwind 集成 | `references/tailwind-integration.md` |

## 组件规范模式

| 属性 | 默认 | Hover | Active | Disabled |
|----------|---------|-------|--------|----------|
| 背景 | primary | primary-dark | primary-darker | muted |
| 文字 | white | white | white | muted-fg |
| 边框 | none | none | none | muted-border |
| 阴影 | sm | md | none | none |

## 脚本

| 脚本 | 用途 |
|--------|--------|
| `generate-tokens.cjs` | 从 JSON Token 配置生成 CSS |
| `validate-tokens.cjs` | 检查代码中的硬编码值 |
| `search-slides.py` | BM25 搜索 + 上下文推荐 |
| `slide-token-validator.py` | 验证幻灯片 HTML 的 Token 合规性 |
| `fetch-background.py` | 从 Pexels/Unsplash 获取图片 |

## 模板

| 模板 | 用途 |
|----------|--------|
| `design-tokens-starter.json` | 含三层结构的入门 JSON |

## 集成

**与品牌：** 从品牌颜色/字体中提取 Primitive
**与 UI 样式：** Component Token → Tailwind 配置

**技能依赖：** brand、ui-styling
**主要 Agent：** ui-ux-designer、frontend-developer

## 幻灯片系统

使用设计 Token + Chart.js + 上下文决策系统生成品牌合规的演示文稿。

### 真相来源

| 文件 | 用途 |
|------|--------|
| `docs/brand-guidelines.md` | 品牌身份、声音、颜色 |
| `assets/design-tokens.json` | Token 定义（Primitive→Semantic→Component） |
| `assets/design-tokens.css` | CSS 变量（在幻灯片中导入） |
| `assets/css/slide-animations.css` | CSS 动画库 |

### 幻灯片搜索（BM25）

```bash
# 基础搜索（自动检测领域）
python scripts/search-slides.py "投资人推介"

# 领域特定搜索
python scripts/search-slides.py "问题渲染" -d copy
python scripts/search-slides.py "收入增长" -d chart

# 上下文搜索（高级系统）
python scripts/search-slides.py "问题幻灯片" --context --position 2 --total 9
python scripts/search-slides.py "CTA" --context --position 9 --prev-emotion frustration
```

### 决策系统 CSV

| 文件 | 用途 |
|------|--------|
| `data/slide-strategies.csv` | 15 种幻灯片结构 + 情感弧线 + 火花线节奏 |
| `data/slide-layouts.csv` | 25 种布局 + 组件变体 + 动画 |
| `data/slide-layout-logic.csv` | 目标 → 布局 + break_pattern 标志 |
| `data/slide-typography.csv` | 内容类型 → 字体系统 |
| `data/slide-color-logic.csv` | 情感 → 颜色处理 |
| `data/slide-backgrounds.csv` | 幻灯片类型 → 图片分类（Pexels/Unsplash） |
| `data/slide-copy.csv` | 25 种文案公式（PAS、AIDA、FAB） |
| `data/slide-charts.csv` | 25 种图表类型及 Chart.js 配置 |

### 上下文决策流程

```
1. 解析目标/上下文
        ↓
2. 搜索 slide-strategies.csv → 获取策略 + 情感节奏
        ↓
3. 对于每张幻灯片：
   a. 查询 slide-layout-logic.csv → 布局 + break_pattern
   b. 查询 slide-typography.csv → 字体系统
   c. 查询 slide-color-logic.csv → 颜色处理
   d. 查询 slide-backgrounds.csv → 需要时获取图片
   e. 从 slide-animations.css 应用动画类
        ↓
4. 使用设计 Token 生成 HTML
        ↓
5. 用 slide-token-validator.py 验证
```

### 模式打破（Duarte 火花线）

高级演示通过情感交替保持参与度：
```
"现状"（挫折）↔ "未来愿景"（希望）
```

系统在 1/3 和 2/3 位置计算模式打破点。

### 幻灯片要求

**所有幻灯片必须：**
1. 导入 `assets/design-tokens.css` - 单一真相来源
2. 使用 CSS 变量：`var(--color-primary)`、`var(--slide-bg)` 等
3. 使用 Chart.js 绘制图表（不是纯 CSS 条形图）
4. 包含导航（键盘方向键、点击、进度条）
5. 居中对齐内容
6. 聚焦于说服/转化

### Chart.js 集成

```html
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.1/dist/chart.umd.min.js"></script>

<canvas id="revenueChart"></canvas>
<script>
new Chart(document.getElementById('revenueChart'), {
    type: 'line',
    data: {
        labels: ['9月', '10月', '11月', '12月'],
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

### Token 合规性

```css
/* 正确 - 使用 Token */
background: var(--slide-bg);
color: var(--color-primary);
font-family: var(--typography-font-heading);

/* 错误 - 硬编码 */
background: #0D0D0D;
color: #FF6B6B;
font-family: 'Space Grotesk';
```

### 参考实现

具有所有功能的工作示例：
```
assets/designs/slides/claudekit-pitch-251223.html
```

### 命令

```bash
/slides:create "为 ClaudeKit Marketing 制作10页投资人推介"
```

## 最佳实践

1. 组件中绝不使用原始 hex 值 - 始终引用 Token
2. Semantic 层支持主题切换（浅色/深色）
3. Component Token 支持按组件自定义
4. 使用 HSL 格式控制透明度
5. 记录每个 Token 的用途
6. **幻灯片必须导入 design-tokens.css 并专门使用 var()**

# 组件规范

核心组件的详细规范，包含状态和变体。

## Button（按钮）

### 变体

| 变体 | 背景 | 文字 | 边框 | 使用场景 |
|---------|------------|------|--------|----------|
| default | primary | white | none | 主要操作 |
| secondary | gray-100 | gray-900 | none | 次要操作 |
| outline | transparent | foreground | border | 第三级操作 |
| ghost | transparent | foreground | none | 细微操作 |
| link | transparent | primary | none | 导航 |
| destructive | red-600 | white | none | 危险操作 |

### 尺寸

| 尺寸 | 高度 | 内边距X | 内边距Y | 字号 | 图标尺寸 |
|------|--------|-----------|-----------|-----------|--------|
| sm | 32px | 12px | 6px | 14px | 16px |
| default | 40px | 16px | 8px | 14px | 18px |
| lg | 48px | 24px | 12px | 16px | 20px |
| icon | 40px | 0 | 0 | - | 18px |

### 状态

| 状态 | 背景 | 文字 | 透明度 | 光标 |
|-------|------------|------|---------|--------|
| default | token | token | 1 | pointer |
| hover | darker | token | 1 | pointer |
| active | darkest | token | 1 | pointer |
| focus | token | token | 1 | pointer |
| disabled | muted | muted-fg | 0.5 | not-allowed |
| loading | token | token | 0.7 | wait |

### 结构

```
┌─────────────────────────────────────┐
│  [icon]  标签文字  [icon]            │
└─────────────────────────────────────┘
     ↑                      ↑
  前置图标              后置图标
```

---

## Input（输入框）

### 变体

| 变体 | 描述 |
|---------|-------------|
| default | 标准文本输入 |
| textarea | 多行文本 |
| select | 下拉选择 |
| checkbox | 布尔切换 |
| radio | 单选 |
| switch | 开关 |

### 尺寸

| 尺寸 | 高度 | 内边距 | 字号 |
|------|--------|---------|--------|
| sm | 32px | 8px 12px | 14px |
| default | 40px | 8px 12px | 14px |
| lg | 48px | 12px 16px | 16px |

### 状态

| 状态 | 边框 | 背景 | 环 |
|-------|--------|------------|------|
| default | gray-300 | white | none |
| hover | gray-400 | white | none |
| focus | primary | white | primary/20% |
| error | red-500 | white | red/20% |
| disabled | gray-200 | gray-100 | none |

### 结构

```
标签（可选）
┌─────────────────────────────────────┐
│ [icon] 占位符/值        [操作]       │
└─────────────────────────────────────┘
辅助文字或错误消息
```

---

## Card（卡片）

### 变体

| 变体 | 阴影 | 边框 | 使用场景 |
|---------|--------|--------|----------|
| default | sm | 1px | 标准卡片 |
| elevated | lg | none | 突出内容 |
| outline | none | 1px | 细微容器 |
| interactive | sm→md | 1px | 可点击卡片 |

### 结构

```
┌─────────────────────────────────────┐
│ 卡片头部                             │
│   标题                               │
│   描述                               │
├─────────────────────────────────────┤
│ 卡片内容                             │
│   主要内容区域                        │
│                                      │
├─────────────────────────────────────┤
│ 卡片底部                             │
│   操作                               │
└─────────────────────────────────────┘
```

### 间距

| 区域 | 内边距 |
|------|--------|
| header | 24px 24px 0 |
| content | 24px |
| footer | 0 24px 24px |
| gap | 16px |

---

## Badge（徽章）

### 变体

| 变体 | 背景 | 文字 |
|---------|------------|------|
| default | primary | white |
| secondary | gray-100 | gray-900 |
| outline | transparent | foreground |
| destructive | red-600 | white |
| success | green-600 | white |
| warning | yellow-500 | gray-900 |

### 尺寸

| 尺寸 | 内边距 | 字号 | 高度 |
|------|---------|-----------|--------|
| sm | 4px 8px | 11px | 20px |
| default | 4px 10px | 12px | 24px |
| lg | 6px 12px | 14px | 28px |

---

## Alert（提示）

### 变体

| 变体 | 图标 | 背景 | 边框 |
|---------|------|------------|--------|
| default | info | gray-50 | gray-200 |
| destructive | alert | red-50 | red-200 |
| success | check | green-50 | green-200 |
| warning | warning | yellow-50 | yellow-200 |

### 结构

```
┌─────────────────────────────────────┐
│ [icon]  标题                      [×]│
│         描述文字                      │
└─────────────────────────────────────┘
```

---

## Dialog（对话框）

### 尺寸

| 尺寸 | 最大宽度 | 使用场景 |
|------|-----------|----------|
| sm | 384px | 简单确认 |
| default | 512px | 标准对话框 |
| lg | 640px | 复杂表单 |
| xl | 768px | 数据密集型对话框 |
| full | 100% - 32px | 移动端全屏 |

### 结构

```
┌───────────────────────────────────────┐
│ 对话框头部                          [×]│
│   标题                                │
│   描述                                │
├───────────────────────────────────────┤
│ 对话框内容                            │
│   如需要可滚动                         │
│                                        │
├───────────────────────────────────────┤
│ 对话框底部                            │
│                     [取消] [确认]      │
└───────────────────────────────────────┘
```

---

## Table（表格）

### 行状态

| 状态 | 背景 | 使用场景 |
|-------|------------|----------|
| default | white | 正常行 |
| hover | gray-50 | 鼠标悬停 |
| selected | primary/10% | 选中行 |
| striped | gray-50/white | 交替行 |

### 单元格对齐

| 内容类型 | 对齐方式 |
|--------------|--------|
| 文本 | 左对齐 |
| 数字 | 右对齐 |
| 状态/徽章 | 居中 |
| 操作 | 右对齐 |

### 间距

| 元素 | 值 |
|---------|-----|
| 单元格内边距 | 12px 16px |
| 表头内边距 | 12px 16px |
| 行高（紧凑） | 40px |
| 行高（默认） | 48px |
| 行高（宽松） | 56px |
