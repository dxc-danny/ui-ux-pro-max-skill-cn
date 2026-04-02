<!--
  翻译说明：
  本文件为组件规范参考文档的中文翻译版。
  原文：Component Specifications - Detailed specs for core components with states and variants.
  翻译：组件规范 - 带有状态和变体的核心组件详细规范。
-->

# 组件规范

带有状态和变体的核心组件详细规范。

## 按钮

### 变体

| 变体 | 背景 | 文字 | 边框 | 使用场景 |
|---------|------------|------|--------|----------|
| default | primary | white | none | 主要操作 |
| secondary | gray-100 | gray-900 | none | 次要操作 |
| outline | transparent | foreground | border | 第三级操作 |
| ghost | transparent | foreground | none | 轻微操作 |
| link | transparent | primary | none | 导航链接 |
| destructive | red-600 | white | none | 危险操作 |

### 尺寸

| 尺寸 | 高度 | 内边距X | 内边距Y | 字体大小 | 图标大小 |
|------|--------|-----------|-----------|-----------|-----------|
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
│  [icon]  Label Text  [icon]         │
└─────────────────────────────────────┘
     ↑                      ↑
  前置图标              后置图标
```

---

## 输入框

### 变体

| 变体 | 描述 |
|---------|-------------|
| default | 标准文本输入 |
| textarea | 多行文本 |
| select | 下拉选择 |
| checkbox | 布尔切换 |
| radio | 单选 |
| switch | 开关切换 |

### 尺寸

| 尺寸 | 高度 | 内边距 | 字体大小 |
|------|--------|---------|-----------|
| sm | 32px | 8px 12px | 14px |
| default | 40px | 8px 12px | 14px |
| lg | 48px | 12px 16px | 16px |

### 状态

| 状态 | 边框 | 背景 | 轮廓环 |
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
│ [icon] Placeholder/Value   [action] │
└─────────────────────────────────────┘
辅助文本或错误消息
```

---

## 卡片

### 变体

| 变体 | 阴影 | 边框 | 使用场景 |
|---------|--------|--------|----------|
| default | sm | 1px | 标准卡片 |
| elevated | lg | none | 突出内容 |
| outline | none | 1px | 轻微容器 |
| interactive | sm→md | 1px | 可点击卡片 |

### 结构

```
┌─────────────────────────────────────┐
│ 卡片头部                            │
│   标题                              │
│   描述                              │
├─────────────────────────────────────┤
│ 卡片内容                            │
│   主要内容区域                       │
│                                     │
├─────────────────────────────────────┤
│ 卡片底部                            │
│   操作按钮                           │
└─────────────────────────────────────┘
```

### 间距

| 区域 | 内边距 |
|------|---------|
| 头部 | 24px 24px 0 |
| 内容 | 24px |
| 底部 | 0 24px 24px |
| 间距 | 16px |

---

## 徽章

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

| 尺寸 | 内边距 | 字体大小 | 高度 |
|------|---------|-----------|--------|
| sm | 4px 8px | 11px | 20px |
| default | 4px 10px | 12px | 24px |
| lg | 6px 12px | 14px | 28px |

---

## 提示/警告

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
│ [icon]  标题                     [×]│
│         描述文本                      │
└─────────────────────────────────────┘
```

---

## 对话框/模态框

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
│ 对话框头部                         [×]│
│   标题                                │
│   描述                                │
├───────────────────────────────────────┤
│ 对话框内容                            │
│   必要时可滚动                         │
│                                       │
├───────────────────────────────────────┤
│ 对话框底部                            │
│                      [取消] [确认]    │
└───────────────────────────────────────┘
```

---

## 表格

### 行状态

| 状态 | 背景 | 使用场景 |
|-------|------------|----------|
| default | white | 普通行 |
| hover | gray-50 | 鼠标悬停 |
| selected | primary/10% | 选中行 |
| striped | gray-50/white | 交替行 |

### 单元格对齐

| 内容类型 | 对齐方式 |
|--------------|-----------|
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
