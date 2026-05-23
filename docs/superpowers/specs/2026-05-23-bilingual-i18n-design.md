# SREFlow 官网中英双语支持设计

## 概述

为 SREFlow 官网（单页 HTML 站点）添加中英双语支持，用户可在导航栏切换语言，页面内容即时切换无需刷新。

## 需求

- 导航栏语言切换器（EN / 中）
- 自动检测浏览器语言偏好，手动切换后记住选择
- localStorage 持久化用户语言偏好
- 单 HTML 文件，翻译集中管理

## 技术方案：数据对象 + DOM 替换

### 语言切换器

- 位置：导航栏右侧，GitHub 链接左侧
- 样式：简洁按钮/下拉，显示当前语言（`EN` / `中`），点击切换
- 优先级：`localStorage` > `navigator.language` > 默认中文

### 翻译数据结构

```js
const i18n = {
  zh: {
    'nav.features': '核心特性',
    'hero.title': 'AI 驱动的<br><span class="text-gradient">SRE 工作流</span>自动化平台',
    'hero.subtitle': '将告警响应、故障诊断与根因分析编排为可复用的 Workflow。<br>从第一个 Workflow 开始，让告警响应快人一步。',
    // ...
  },
  en: {
    'nav.features': 'Features',
    'hero.title': 'AI-Powered<br><span class="text-gradient">SRE Workflow</span> Automation',
    'hero.subtitle': 'Orchestrate alert response, incident diagnosis, and root cause analysis into reusable Workflows.<br>Start with your first Workflow and respond to alerts faster.',
    // ...
  }
};
```

### DOM 标记

所有需翻译的文本元素添加 `data-i18n` 属性：

```html
<span class="section-label" data-i18n="features.label">核心特性</span>
<h2 class="section-title" data-i18n="features.title">为 <span class="text-gradient">智能运维</span> 而生</h2>
```

- `data-i18n="key"` → 替换 `innerHTML`
- `data-i18n-placeholder="key"` → 替换 `placeholder`
- `data-i18n-alt="key"` → 替换 `alt`

### 切换逻辑

```js
function setLang(lang) {
  document.querySelectorAll('[data-i18n]').forEach(el => {
    el.innerHTML = i18n[lang][el.dataset.i18n];
  });
  document.documentElement.lang = lang === 'zh' ? 'zh-CN' : 'en';
  localStorage.setItem('lang', lang);
}
```

### 初始化

```js
const saved = localStorage.getItem('lang');
const browserLang = navigator.language.startsWith('zh') ? 'zh' : 'en';
const lang = saved || browserLang;
setLang(lang);
```

## 不翻译的内容

- SVG 工作流图中的技术标签（Prometheus、Loki、Jaeger 等产品名）
- 架构图中的技术名（Gin、GORM、Next.js 等）
- 代码块
- 图片 alt 文本需翻译

## 翻译范围

全站所有中文文本，包括：
- 导航链接
- Hero 区域标题/副标题/按钮
- 各 section 的 label/title/description
- Feature 卡片标题和描述
- 节点分类名和芯片描述
- MCP 卡片描述
- CTA 区域
- Footer
- 图片 alt 文本
