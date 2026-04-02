# 状态与变体

组件状态定义和变体模式。

## 交互状态

### 状态定义

| 状态 | 触发 | 视觉变化 |
|-------|---------|---------------|
| default | 无 | 基础外观 |
| hover | 鼠标悬停 | 轻微颜色变化 |
| focus | Tab/点击 | 焦点环 |
| active | 鼠标按下 | 最深颜色 |
| disabled | disabled 属性 | 降低透明度 |
| loading | 异步操作 | 加载动画 + 透明度 |

### 状态优先级

当多个状态同时应用时，优先级（从高到低）：

1. disabled
2. loading
3. active
4. focus
5. hover
6. default

### 状态过渡

```css
/* 交互元素的标准过渡 */
.interactive {
  transition-property: color, background-color, border-color, box-shadow;
  transition-duration: var(--duration-fast);
  transition-timing-function: ease-in-out;
}
```

| 过渡 | 时长 | 缓动 |
|------------|----------|--------|
| 颜色变化 | 150ms | ease-in-out |
| 背景 | 150ms | ease-in-out |
| 变形 | 200ms | ease-out |
| 透明度 | 150ms | ease |
| 阴影 | 200ms | ease-out |

## 焦点状态

### 焦点环规范

```css
/* 标准焦点环 */
.focusable:focus-visible {
  outline: none;
  box-shadow: 0 0 0 var(--ring-offset) var(--color-background),
              0 0 0 calc(var(--ring-offset) + var(--ring-width)) var(--ring-color);
}
```

| 属性 | 值 |
|----------|-----|
| 环宽度 | 2px |
| 环偏移 | 2px |
| 环颜色 | primary（blue-500） |
| 偏移颜色 | background |

### 容器焦点

```css
/* 当子元素获得焦点时容器也获得焦点 */
.container:focus-within {
  border-color: var(--color-ring);
}
```

## 禁用状态

### 视觉处理

```css
.disabled {
  opacity: var(--opacity-disabled); /* 0.5 */
  pointer-events: none;
  cursor: not-allowed;
}
```

| 属性 | 禁用时的值 |
|----------|----------------|
| 透明度 | 50% |
| 指针事件 | none |
| 光标 | not-allowed |
| 背景 | muted |
| 颜色 | muted-foreground |

### 无障碍

- 使用 `aria-disabled="true"` 表示语义上的禁用
- 对表单元素使用 `disabled` 属性
- 保持足够的对比度（最小 3:1）

## 加载状态

### 加载动画位置

| 组件 | 加载动画位置 |
|-----------|----------------|
| Button | 替换图标或居中 |
| Input | 尾部位置 |
| Card | 居中覆盖 |
| Page | 视口中心 |

### 加载处理

```css
.loading {
  position: relative;
  pointer-events: none;
}

.loading::after {
  content: '';
  /* 加载动画样式 */
}

.loading > * {
  opacity: 0.7;
}
```

## 错误状态

### 视觉指示器

```css
.error {
  border-color: var(--color-error);
  color: var(--color-error);
}

.error:focus-visible {
  box-shadow: 0 0 0 2px var(--color-background),
              0 0 0 4px var(--color-error);
}
```

| 元素 | 错误处理 |
|---------|-----------------|
| 输入框边框 | red-500 |
| 输入框焦点环 | red/20% |
| 辅助文字 | red-600 |
| 图标 | red-500 |

### 错误消息

- 放在输入框下方
- 使用错误颜色
- 包含图标以提高可访问性
- 输入有效时清除

## 变体模式

### 颜色变体

```css
/* 颜色变体模式 */
.component {
  --component-bg: var(--color-primary);
  --component-fg: var(--color-primary-foreground);
  background: var(--component-bg);
  color: var(--component-fg);
}

.component.secondary {
  --component-bg: var(--color-secondary);
  --component-fg: var(--color-secondary-foreground);
}

.component.destructive {
  --component-bg: var(--color-destructive);
  --component-fg: var(--color-destructive-foreground);
}
```

### 尺寸变体

```css
/* 尺寸变体模式 */
.component {
  --component-height: 40px;
  --component-padding: var(--space-4);
  --component-font: var(--font-size-sm);
}

.component.sm {
  --component-height: 32px;
  --component-padding: var(--space-3);
  --component-font: var(--font-size-xs);
}

.component.lg {
  --component-height: 48px;
  --component-padding: var(--space-6);
  --component-font: var(--font-size-base);
}
```

## 无障碍要求

### 颜色对比

| 元素 | 最小对比度 |
|---------|------------|
| 正常文本 | 4.5:1 |
| 大文本（18px+） | 3:1 |
| UI 组件 | 3:1 |
| 焦点指示器 | 3:1 |

### 状态指示器

- 绝不仅依赖颜色
- 使用图标、文本或图案
- 确保焦点可见
- 提供加载通知

### ARIA 状态

```html
<!-- 禁用 -->
<button disabled aria-disabled="true">提交</button>

<!-- 加载中 -->
<button aria-busy="true" aria-describedby="loading-text">
  <span id="loading-text" class="sr-only">加载中...</span>
</button>

<!-- 错误 -->
<input aria-invalid="true" aria-describedby="error-msg">
<span id="error-msg" role="alert">错误消息</span>
```
