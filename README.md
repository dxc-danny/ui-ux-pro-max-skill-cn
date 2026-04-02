# UI/UX Pro Max Skill - 中文版

<p align="center">
  <img src="https://img.shields.io/badge/设计系统-中文版-blue?style=for-the-badge" alt="中文版">
  <img src="https://img.shields.io/badge/许可证-MIT-green?style=for-the-badge" alt="MIT">
</p>

> 🎨 **Design System 技能的中文本地化版本**
>
> 原始项目：[nextlevelbuilder/ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill)

## 📖 简介

本项目是 [UI/UX Pro Max Skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) 的中文本地化版本。

Design System 技能提供了：

- 🎯 **Token 架构**：三层 Token 系统（Primitive → Semantic → Component）
- 🧩 **组件规范**：组件状态定义和规范
- 🎨 **样式系统**：CSS 变量、排版、间距规范
- 📊 **幻灯片生成**：品牌合规的演示文稿制作系统

## 📁 项目结构

```
.claude/
└── skills/
    └── design-system/
        ├── SKILL.md                    # 技能主文件
        ├── data/                       # CSV 数据文件
        │   ├── slide-backgrounds.csv   # 幻灯片背景
        │   ├── slide-charts.csv       # 图表配置
        │   ├── slide-color-logic.csv  # 颜色逻辑
        │   ├── slide-copy.csv          # 文案公式
        │   ├── slide-layout-logic.csv # 布局逻辑
        │   ├── slide-layouts.csv      # 布局模板
        │   ├── slide-strategies.csv   # 策略结构
        │   └── slide-typography.csv   # 排版规范
        ├── references/                 # 参考文档
        │   ├── component-specs.md
        │   ├── component-tokens.md
        │   ├── primitive-tokens.md
        │   ├── semantic-tokens.md
        │   ├── states-and-variants.md
        │   ├── tailwind-integration.md
        │   └── token-architecture.md
        ├── scripts/                    # 脚本工具
        │   ├── embed-tokens.cjs
        │   ├── fetch-background.py
        │   ├── generate-slide.py
        │   ├── generate-tokens.cjs
        │   ├── html-token-validator.py
        │   ├── search-slides.py
        │   ├── slide-token-validator.py
        │   ├── slide_search_core.py
        │   └── validate-tokens.cjs
        └── templates/                 # 模板文件
            └── design-tokens-starter.json
```

## 🚀 快速开始

### 安装

将本仓库克隆到你的 OpenClaw skills 目录：

```bash
cd ~/.openclaw/workspace/skills
git clone https://github.com/dxc-danny/ui-ux-pro-max-skill-cn.git design-system-cn
```

### 使用 Design Tokens

```bash
# 生成 Token CSS
node scripts/generate-tokens.cjs --config tokens.json -o tokens.css

# 验证代码中的 Token 使用
node scripts/validate-tokens.cjs --dir src/
```

### 幻灯片搜索

```bash
# 基础搜索
python scripts/search-slides.py "投资者路演"

# 按领域搜索
python scripts/search-slides.py "问题引导" -d copy
python scripts/search-slides.py "收入增长" -d chart

# 上下文搜索
python scripts/search-slides.py "问题幻灯片" --context --position 2 --total 9
```

## 📚 核心概念

### 三层 Token 架构

```
┌─────────────────────┐
│  Primitive Tokens   │  原始值（颜色、字号等）
│  (基元层)           │
└──────────┬──────────┘
           ↓
┌─────────────────────┐
│  Semantic Tokens    │  语义化别名（primary、success）
│  (语义层)            │
└──────────┬──────────┘
           ↓
┌─────────────────────┐
│  Component Tokens  │  组件特定变量
│  (组件层)            │
└─────────────────────┘
```

### 示例

```css
/* 基元层 */
--color-blue-600: #2563EB;

/* 语义层 */
--color-primary: var(--color-blue-600);

/* 组件层 */
--button-bg: var(--color-primary);
```

## 📄 许可证

本项目基于 MIT 许可证，遵循原始项目的许可证条款。

## 🙏 致谢

- 原始项目：[nextlevelbuilder/ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill)
- OpenClaw 平台：[OpenClaw](https://openclaw.ai)
