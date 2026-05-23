# Bilingual i18n Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add Chinese/English bilingual support to the SREFlow landing page with a navbar language switcher.

**Architecture:** Single HTML file approach using a JS translation object and `data-i18n` attributes on DOM elements. A `setLang()` function replaces innerHTML for all marked elements. Language preference is persisted in localStorage, with browser language detection as fallback.

**Tech Stack:** Vanilla JS, HTML data attributes, localStorage

---

### Task 1: Add language switcher to navbar

**Files:**
- Modify: `index.html` (nav section, lines ~96-122)

- [ ] **Step 1: Add the language switcher button and CSS**

Insert the language switcher button in the nav, between the nav-links and the GitHub link. Also add the CSS for it.

In the nav `.container`, add the switcher before the GitHub link:

```html
<button class="nav-lang" onclick="toggleLang()" aria-label="Switch language">
  <span class="lang-label" data-i18n="nav.lang">中</span>
</button>
```

Add this CSS inside the `<style>` block, after the `.nav-github svg` rule (around line 122):

```css
.nav-lang {
  display: flex; align-items: center; gap: 6px;
  padding: 8px 16px; border-radius: 8px;
  background: rgba(255,255,255,0.06);
  border: 1px solid var(--border);
  color: var(--text); font-weight: 600; font-size: 0.9rem;
  cursor: pointer; transition: all 0.2s;
  font-family: var(--font);
}
.nav-lang:hover { border-color: #fff; background: rgba(255,255,255,0.1); }
```

- [ ] **Step 2: Verify the button appears in the navbar**

Open `index.html` in a browser. Confirm the language button appears in the nav bar between the links and the GitHub button.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add language switcher button to navbar"
```

---

### Task 2: Add data-i18n attributes to all translatable elements

**Files:**
- Modify: `index.html` (all content sections)

This task walks through each section, adding `data-i18n` attributes. Elements that contain HTML (like `<span class="text-gradient">`) use innerHTML replacement.

- [ ] **Step 1: Add data-i18n to the `<html>` tag and Hero section**

Change `<html lang="zh-CN">` to `<html lang="zh-CN" id="html-root">`.

Add `data-i18n` to Hero elements:

```html
<h1 class="hero-title fade-up fade-up-d1" data-i18n="hero.title">
  AI 驱动的<br><span class="text-gradient">SRE 工作流</span>自动化平台
</h1>
<p class="hero-subtitle fade-up fade-up-d2" data-i18n="hero.subtitle">
  将告警响应、故障诊断与根因分析编排为可复用的 Workflow。<br>从第一个 Workflow 开始，让告警响应快人一步。
</p>
```

Update the "申请试用" button text:

```html
<a href="..." target="_blank" class="btn btn-primary">
  <svg ...></svg>
  <span data-i18n="hero.cta">申请试用</span>
</a>
```

- [ ] **Step 2: Add data-i18n to the Features section**

```html
<span class="section-label" data-i18n="features.label">核心特性</span>
<h2 class="section-title" data-i18n="features.title">为 <span class="text-gradient">智能运维</span> 而生</h2>
<p class="section-desc" data-i18n="features.desc">从告警触发到根因定位，SREFlow 让每一步都可编排、可观测、可复用。</p>
```

Feature cards (all 6):

```html
<div class="feature-card">
  <div class="feature-icon mono">⚡</div>
  <h3 data-i18n="features.workflow.title">可视化 Workflow 引擎</h3>
  <p data-i18n="features.workflow.desc">基于 DAG 的层级执行模型，支持并发节点、条件分支、变量传递与重试策略，让复杂运维流程一目了然。</p>
</div>
```

Keys for all 6 cards:
- `features.workflow.title` / `features.workflow.desc`
- `features.nodes.title` / `features.nodes.desc`
- `features.mcp.title` / `features.mcp.desc`
- `features.tenant.title` / `features.tenant.desc`
- `features.topology.title` / `features.topology.desc`
- `features.integrations.title` / `features.integrations.desc`

- [ ] **Step 3: Add data-i18n to the Workflow editor screenshot section**

```html
<span class="section-label" data-i18n="workflow.label">Workflow 编辑器</span>
<h2 class="section-title" data-i18n="workflow.title">拖拽编排，<span class="text-gradient">所见即所得</span></h2>
<p class="section-desc" style="margin: 0 auto 36px;" data-i18n="workflow.desc">可视化构建告警响应流程，DAG 拓扑实时渲染。</p>
<img src="sre-workflow.png" data-i18n-alt="workflow.img.alt" alt="SREFlow Workflow 编辑器" ...>
```

- [ ] **Step 4: Add data-i18n to the Node Types section**

```html
<span class="section-label" data-i18n="nodes.label">内置节点</span>
<h2 class="section-title" data-i18n="nodes.title">四大类别，<span class="text-gradient">16+ 节点</span></h2>
<p class="section-desc" data-i18n="nodes.desc">从数据采集到智能分析，从逻辑判断到结果输出，覆盖 SRE 全场景。</p>
```

Node categories:

```html
<div class="node-category-title"><span class="emoji">📥</span> <span data-i18n="nodes.collect">数据采集</span></div>
```

Keys:
- `nodes.collect` → 数据采集
- `nodes.analyze` → 处理分析
- `nodes.io` → 输入输出

Node chips (add `data-i18n` to the `.chip-desc` spans):

```html
<div class="node-chip"><span class="chip-name">prometheus</span><span class="chip-desc" data-i18n="nodes.chip.prometheus">PromQL 查询</span></div>
```

Keys for all chips:
- `nodes.chip.prometheus` → PromQL 查询 / PromQL Query
- `nodes.chip.jaeger` → 链路追踪 / Tracing
- `nodes.chip.loki` → 日志查询 / Log Query
- `nodes.chip.elasticsearch` → 全文检索 / Full-text Search
- `nodes.chip.clickhouse` → SQL 查询 / SQL Query
- `nodes.chip.kubernetes` → K8s 资源查询 / K8s Resource Query
- `nodes.chip.ssh` → 远程命令 / Remote Command
- `nodes.chip.topology` → 拓扑关系 / Topology
- `nodes.chip.llm` → 大模型推理 / LLM Inference
- `nodes.chip.condition` → 条件分支 / Condition Branch
- `nodes.chip.goscript` → Go 脚本 / Go Script
- `nodes.chip.mcptool` → MCP 工具 / MCP Tool
- `nodes.chip.start` → 工作流入口 / Workflow Entry
- `nodes.chip.output` → 结果输出 / Result Output
- `nodes.chip.http` → HTTP 请求 / HTTP Request

- [ ] **Step 5: Add data-i18n to the Workflow management screenshot section**

```html
<span class="section-label" data-i18n="wfmgmt.label">工作流管理</span>
<h2 class="section-title" data-i18n="wfmgmt.title">统一管理，<span class="text-gradient">一键复用</span></h2>
<p class="section-desc" style="margin: 0 auto 36px;" data-i18n="wfmgmt.desc">工作空间级别的工作流管理，支持模板化复用与版本追踪。</p>
<img src="workflows.png" data-i18n-alt="wfmgmt.img.alt" alt="SREFlow 工作流列表" ...>
```

- [ ] **Step 6: Add data-i18n to the Architecture section**

```html
<span class="section-label" data-i18n="arch.label">架构设计</span>
<h2 class="section-title" data-i18n="arch.title">分层架构，<span class="text-gradient">职责清晰</span></h2>
<p class="section-desc" data-i18n="arch.desc">从前端控制台到工作流引擎，每一层独立演进。</p>
```

- [ ] **Step 7: Add data-i18n to the Integrations screenshot section**

```html
<span class="section-label" data-i18n="integrations.label">数据源集成</span>
<h2 class="section-title" data-i18n="integrations.title">开箱即用，<span class="text-gradient">无缝对接</span></h2>
<p class="section-desc" style="margin: 0 auto 36px;" data-i18n="integrations.desc">Prometheus、Loki、Jaeger、Kubernetes 等主流数据源一键接入。</p>
<img src="integrations.png" data-i18n-alt="integrations.img.alt" alt="SREFlow 数据源集成" ...>
```

- [ ] **Step 8: Add data-i18n to the MCP section**

```html
<span class="section-label" data-i18n="mcp.label">MCP 集成</span>
<h2 class="section-title" data-i18n="mcp.title"><span class="text-gradient">AI 原生</span>工作流编排</h2>
<p class="section-desc" data-i18n="mcp.desc">通过 MCP 协议，LLM Agent 可直接调用 SREFlow 的工作流能力。</p>
```

MCP cards:

```html
<div class="mcp-card">
  <div class="mcp-name">workflow_list</div>
  <div class="mcp-desc" data-i18n="mcp.card.list">列出工作空间下所有工作流</div>
</div>
<div class="mcp-card">
  <div class="mcp-name">workflow_execute</div>
  <div class="mcp-desc" data-i18n="mcp.card.execute">执行指定工作流</div>
</div>
<div class="mcp-card">
  <div class="mcp-name">workflow_status</div>
  <div class="mcp-desc" data-i18n="mcp.card.status">查询执行状态</div>
</div>
<div class="mcp-card">
  <div class="mcp-name">workflow_result</div>
  <div class="mcp-desc" data-i18n="mcp.card.result">获取执行结果</div>
</div>
```

Also the MCP image:

```html
<img src="mcp.png" data-i18n-alt="mcp.img.alt" alt="SREFlow MCP 集成" ...>
```

- [ ] **Step 9: Add data-i18n to the CTA and Footer sections**

CTA:

```html
<h2 class="section-title" data-i18n="cta.title">准备好 <span class="text-gradient">重新定义</span> 你的 SRE 工作流了吗？</h2>
<p class="section-desc" data-i18n="cta.desc">从第一个 Workflow 开始，让告警响应快人一步。</p>
```

CTA button:

```html
<a href="..." target="_blank" class="btn btn-outline">
  <svg ...></svg>
  <span data-i18n="cta.btn">申请试用</span>
</a>
```

Footer:

```html
<p data-i18n="footer.text">SREFlow &mdash; AI 驱动的 SRE 工作流自动化平台</p>
```

- [ ] **Step 10: Add data-i18n to the hero screenshot image**

```html
<img src="home.png" data-i18n-alt="hero.img.alt" alt="SREFlow 产品界面" ...>
```

- [ ] **Step 11: Commit**

```bash
git add index.html
git commit -m "feat: add data-i18n attributes to all translatable elements"
```

---

### Task 3: Add the i18n translation data object and switching logic

**Files:**
- Modify: `index.html` (script section, lines ~865-887)

- [ ] **Step 1: Add the complete i18n data object and functions**

Replace the entire `<script>` block at the end of `index.html` with:

```html
<script>
// ========== i18n ==========
const i18n = {
  zh: {
    'nav.lang': 'EN',
    'hero.title': 'AI 驱动的<br><span class="text-gradient">SRE 工作流</span>自动化平台',
    'hero.subtitle': '将告警响应、故障诊断与根因分析编排为可复用的 Workflow。<br>从第一个 Workflow 开始，让告警响应快人一步。',
    'hero.cta': '申请试用',
    'hero.img.alt': 'SREFlow 产品界面',
    'features.label': '核心特性',
    'features.title': '为 <span class="text-gradient">智能运维</span> 而生',
    'features.desc': '从告警触发到根因定位，SREFlow 让每一步都可编排、可观测、可复用。',
    'features.workflow.title': '可视化 Workflow 引擎',
    'features.workflow.desc': '基于 DAG 的层级执行模型，支持并发节点、条件分支、变量传递与重试策略，让复杂运维流程一目了然。',
    'features.nodes.title': '丰富的内置节点',
    'features.nodes.desc': '覆盖数据采集、AI 分析、逻辑处理与输出四大类别，开箱即用，也支持自定义扩展。',
    'features.mcp.title': 'MCP Server',
    'features.mcp.desc': '内置 Model Context Protocol 服务端，LLM Agent 可直接调用编排工作流，实现 AI 原生集成。',
    'features.tenant.title': '多租户 Workspace',
    'features.tenant.desc': '工作空间级别的资源隔离，集成与工作流按空间管理，满足多团队协作需求。',
    'features.topology.title': '拓扑管理',
    'features.topology.desc': '可视化定义服务拓扑关系，供 Workflow 节点引用，实现基于拓扑的智能故障定位。',
    'features.integrations.title': '多数据源集成',
    'features.integrations.desc': 'Prometheus、Jaeger、Loki、Elasticsearch、ClickHouse、Kubernetes、SSH 开箱即用。',
    'workflow.label': 'Workflow 编辑器',
    'workflow.title': '拖拽编排，<span class="text-gradient">所见即所得</span>',
    'workflow.desc': '可视化构建告警响应流程，DAG 拓扑实时渲染。',
    'workflow.img.alt': 'SREFlow Workflow 编辑器',
    'nodes.label': '内置节点',
    'nodes.title': '四大类别，<span class="text-gradient">16+ 节点</span>',
    'nodes.desc': '从数据采集到智能分析，从逻辑判断到结果输出，覆盖 SRE 全场景。',
    'nodes.collect': '数据采集',
    'nodes.analyze': '处理分析',
    'nodes.io': '输入输出',
    'nodes.chip.prometheus': 'PromQL 查询',
    'nodes.chip.jaeger': '链路追踪',
    'nodes.chip.loki': '日志查询',
    'nodes.chip.elasticsearch': '全文检索',
    'nodes.chip.clickhouse': 'SQL 查询',
    'nodes.chip.kubernetes': 'K8s 资源查询',
    'nodes.chip.ssh': '远程命令',
    'nodes.chip.topology': '拓扑关系',
    'nodes.chip.llm': '大模型推理',
    'nodes.chip.condition': '条件分支',
    'nodes.chip.goscript': 'Go 脚本',
    'nodes.chip.mcptool': 'MCP 工具',
    'nodes.chip.start': '工作流入口',
    'nodes.chip.output': '结果输出',
    'nodes.chip.http': 'HTTP 请求',
    'wfmgmt.label': '工作流管理',
    'wfmgmt.title': '统一管理，<span class="text-gradient">一键复用</span>',
    'wfmgmt.desc': '工作空间级别的工作流管理，支持模板化复用与版本追踪。',
    'wfmgmt.img.alt': 'SREFlow 工作流列表',
    'arch.label': '架构设计',
    'arch.title': '分层架构，<span class="text-gradient">职责清晰</span>',
    'arch.desc': '从前端控制台到工作流引擎，每一层独立演进。',
    'integrations.label': '数据源集成',
    'integrations.title': '开箱即用，<span class="text-gradient">无缝对接</span>',
    'integrations.desc': 'Prometheus、Loki、Jaeger、Kubernetes 等主流数据源一键接入。',
    'integrations.img.alt': 'SREFlow 数据源集成',
    'mcp.label': 'MCP 集成',
    'mcp.title': '<span class="text-gradient">AI 原生</span>工作流编排',
    'mcp.desc': '通过 MCP 协议，LLM Agent 可直接调用 SREFlow 的工作流能力。',
    'mcp.card.list': '列出工作空间下所有工作流',
    'mcp.card.execute': '执行指定工作流',
    'mcp.card.status': '查询执行状态',
    'mcp.card.result': '获取执行结果',
    'mcp.img.alt': 'SREFlow MCP 集成',
    'cta.title': '准备好 <span class="text-gradient">重新定义</span> 你的 SRE 工作流了吗？',
    'cta.desc': '从第一个 Workflow 开始，让告警响应快人一步。',
    'cta.btn': '申请试用',
    'footer.text': 'SREFlow &mdash; AI 驱动的 SRE 工作流自动化平台'
  },
  en: {
    'nav.lang': '中',
    'hero.title': 'AI-Powered<br><span class="text-gradient">SRE Workflow</span> Automation',
    'hero.subtitle': 'Orchestrate alert response, incident diagnosis, and root cause analysis into reusable Workflows.<br>Start with your first Workflow and respond to alerts faster.',
    'hero.cta': 'Request Trial',
    'hero.img.alt': 'SREFlow Product Interface',
    'features.label': 'Core Features',
    'features.title': 'Built for <span class="text-gradient">Intelligent Operations</span>',
    'features.desc': 'From alert triggers to root cause identification, SREFlow makes every step orchestrable, observable, and reusable.',
    'features.workflow.title': 'Visual Workflow Engine',
    'features.workflow.desc': 'DAG-based hierarchical execution model with concurrent nodes, conditional branching, variable passing, and retry strategies — making complex operations crystal clear.',
    'features.nodes.title': 'Rich Built-in Nodes',
    'features.nodes.desc': 'Covering data collection, AI analysis, logic processing, and output — ready to use out of the box with custom extension support.',
    'features.mcp.title': 'MCP Server',
    'features.mcp.desc': 'Built-in Model Context Protocol server enables LLM Agents to directly invoke orchestrated workflows for AI-native integration.',
    'features.tenant.title': 'Multi-tenant Workspace',
    'features.tenant.desc': 'Workspace-level resource isolation with per-space integration and workflow management for multi-team collaboration.',
    'features.topology.title': 'Topology Management',
    'features.topology.desc': 'Visually define service topology relationships for Workflow nodes to reference, enabling topology-based intelligent fault localization.',
    'features.integrations.title': 'Multi-source Integration',
    'features.integrations.desc': 'Prometheus, Jaeger, Loki, Elasticsearch, ClickHouse, Kubernetes, and SSH — ready to use out of the box.',
    'workflow.label': 'Workflow Editor',
    'workflow.title': 'Drag & Drop, <span class="text-gradient">What You See Is What You Get</span>',
    'workflow.desc': 'Visually build alert response workflows with real-time DAG topology rendering.',
    'workflow.img.alt': 'SREFlow Workflow Editor',
    'nodes.label': 'Built-in Nodes',
    'nodes.title': '4 Categories, <span class="text-gradient">16+ Nodes</span>',
    'nodes.desc': 'From data collection to intelligent analysis, from logic decisions to result output — covering the full SRE spectrum.',
    'nodes.collect': 'Data Collection',
    'nodes.analyze': 'Processing & Analysis',
    'nodes.io': 'Input & Output',
    'nodes.chip.prometheus': 'PromQL Query',
    'nodes.chip.jaeger': 'Tracing',
    'nodes.chip.loki': 'Log Query',
    'nodes.chip.elasticsearch': 'Full-text Search',
    'nodes.chip.clickhouse': 'SQL Query',
    'nodes.chip.kubernetes': 'K8s Resource Query',
    'nodes.chip.ssh': 'Remote Command',
    'nodes.chip.topology': 'Topology',
    'nodes.chip.llm': 'LLM Inference',
    'nodes.chip.condition': 'Condition Branch',
    'nodes.chip.goscript': 'Go Script',
    'nodes.chip.mcptool': 'MCP Tool',
    'nodes.chip.start': 'Workflow Entry',
    'nodes.chip.output': 'Result Output',
    'nodes.chip.http': 'HTTP Request',
    'wfmgmt.label': 'Workflow Management',
    'wfmgmt.title': 'Unified Management, <span class="text-gradient">One-click Reuse</span>',
    'wfmgmt.desc': 'Workspace-level workflow management with template reuse and version tracking.',
    'wfmgmt.img.alt': 'SREFlow Workflow List',
    'arch.label': 'Architecture',
    'arch.title': 'Layered Architecture, <span class="text-gradient">Clear Responsibilities</span>',
    'arch.desc': 'From the frontend console to the workflow engine, each layer evolves independently.',
    'integrations.label': 'Data Source Integration',
    'integrations.title': 'Ready to Use, <span class="text-gradient">Seamless Integration</span>',
    'integrations.desc': 'One-click access to mainstream data sources including Prometheus, Loki, Jaeger, and Kubernetes.',
    'integrations.img.alt': 'SREFlow Data Source Integration',
    'mcp.label': 'MCP Integration',
    'mcp.title': '<span class="text-gradient">AI-Native</span> Workflow Orchestration',
    'mcp.desc': 'Through the MCP protocol, LLM Agents can directly invoke SREFlow workflow capabilities.',
    'mcp.card.list': 'List all workflows in workspace',
    'mcp.card.execute': 'Execute a specified workflow',
    'mcp.card.status': 'Query execution status',
    'mcp.card.result': 'Get execution result',
    'mcp.img.alt': 'SREFlow MCP Integration',
    'cta.title': 'Ready to <span class="text-gradient">Redefine</span> Your SRE Workflows?',
    'cta.desc': 'Start with your first Workflow and respond to alerts faster.',
    'cta.btn': 'Request Trial',
    'footer.text': 'SREFlow &mdash; AI-Powered SRE Workflow Automation Platform'
  }
};

let currentLang = 'zh';

function setLang(lang) {
  currentLang = lang;
  document.querySelectorAll('[data-i18n]').forEach(el => {
    const key = el.dataset.i18n;
    if (i18n[lang][key]) el.innerHTML = i18n[lang][key];
  });
  document.querySelectorAll('[data-i18n-alt]').forEach(el => {
    const key = el.dataset.i18nAlt;
    if (i18n[lang][key]) el.setAttribute('alt', i18n[lang][key]);
  });
  document.documentElement.lang = lang === 'zh' ? 'zh-CN' : 'en';
  localStorage.setItem('lang', lang);
}

function toggleLang() {
  setLang(currentLang === 'zh' ? 'en' : 'zh');
}

// Init
(function() {
  const saved = localStorage.getItem('lang');
  const browserLang = navigator.language.startsWith('zh') ? 'zh' : 'en';
  setLang(saved || browserLang);
})();

// Intersection Observer for fade-in
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.style.animation = 'fade-up 0.6s ease-out both';
      observer.unobserve(entry.target);
    }
  });
}, { threshold: 0.1 });

document.querySelectorAll('.feature-card, .node-chip, .tech-item, .mcp-card').forEach(el => {
  el.style.opacity = '0';
  observer.observe(el);
});
</script>
```

- [ ] **Step 2: Open the page in a browser and test language switching**

Click the language switcher button. Verify:
1. All Chinese text switches to English
2. Clicking again switches back to Chinese
3. Refresh the page — the last selected language persists
4. The `<html lang>` attribute updates correctly

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add i18n translation data and language switching logic"
```

---

### Task 4: Final polish and verification

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Test in a browser with English browser settings**

Set browser language to English. Clear localStorage. Open the page. Verify it defaults to English.

- [ ] **Step 2: Test in a browser with Chinese browser settings**

Set browser language to Chinese. Clear localStorage. Open the page. Verify it defaults to Chinese.

- [ ] **Step 3: Test mobile responsiveness**

Resize to mobile width. Verify the language switcher is still visible and functional.

- [ ] **Step 4: Final commit**

```bash
git add index.html
git commit -m "feat: complete bilingual i18n support for SREFlow landing page"
```
