# 产品界面 token（移动端 + B端）

从 `xinxiangsu-uiux-portfolio` 观察到的视觉语言里**抽掉表演性元素**后得到的、能真正用于产品的一组规格。

---

## 1. 结构尺寸

### 移动端

| 项 | 值 |
|---|---|
| 设计稿基准宽 | **390 px**（iPhone 14/15） |
| 左右安全边距 | 20 px |
| 栅格 | 4 列 gutter 12 px |
| 顶栏高 | 56 px（含状态栏 44 px 另计） |
| 底部 tab 高 | 56 px + `env(safe-area-inset-bottom)` |
| 主按钮高 | 52 px，圆角 14 px |
| 输入框高 | 48 px，圆角 12 px |
| 列表行高 | 56 px（单行）/ 72 px（带副标题） |
| 卡片内边距 | 16 px |

### B端桌面端

| 项 | 值 |
|---|---|
| 最小支持宽 | 1280 px |
| 侧边栏 | 展开 232 px / 收起 64 px |
| 顶栏高 | 60 px |
| 内容区边距 | 24 px |
| 内容最大宽 | 不限宽，但单列表格区 ≤ 1600 px |
| 栅格 | 12 列，gutter 16 px |
| 控件高 | 输入/按钮 36 px（B端比移动端紧一档） |
| 表格行高 | 48 px（普通）/ 40 px（紧凑） |

---

## 2. 色彩

### 2.1 品牌主色（按品类选一个）

| 品类 | 主色 | Hover | 浅底（10%） | 深色文字 |
|---|---|---|---|---|
| 生活服务 / 工具 | `#0FB5D6` | `#0A9BBA` | `#E6F9FD` | `#04262F` |
| 社交 / 内容 | `#6B4EFF` | `#5A3EE6` | `#EFECFF` | `#1B0E5C` |
| 交易 / 金融 | `#1668FF` | `#0B54D6` | `#E8F1FF` | `#062A66` |
| 家居 / 硬件 | `#7C3AED` | `#692BD1` | `#F1E9FE` | `#2A0E5E` |
| 招聘 / 促销 | `#FF9500` | `#E88400` | `#FFF3E0` | `#5C3400` |

### 2.2 中性色（两端共用）

| 语义 | 值 | 用途 |
|---|---|---|
| 文字主 | `#111318` | 标题、关键数字 |
| 文字次 | `#5A6070` | 正文、说明 |
| 文字弱 | `#9AA0AE` | 占位符、时间戳 |
| 禁用文字 | `#C2C7D0` | 禁用态 |
| 分隔线 | `#EDEFF3` | 列表分隔、卡片描边 |
| 页面底 | `#F5F6FA` | B端背景、移动端分区底 |
| 卡片底 | `#FFFFFF` | — |
| 遮罩 | `rgba(12,15,20,.45)` | 弹窗 |

### 2.3 语义色

| 语义 | 主色 | 浅底 |
|---|---|---|
| 成功 | `#12B76A` | `#E7F8F0` |
| 警告 | `#F79009` | `#FEF3E2` |
| 危险 / 错误 | `#F04438` | `#FDECEA` |
| 信息 | 主色 | 主色 10% |

### 2.4 金融涨跌（**中国习惯，不可搞反**）

- **涨 / 正向：`#F5222D` 红**
- **跌 / 负向：`#22C55E` 绿**
- 平盘：`#5A6070`
- 图表主序列用主色，对比序列用主色 35% 透明

---

## 3. 字体

字体栈：
```css
--font: -apple-system, "PingFang SC", "Source Han Sans SC", "Noto Sans SC", "Microsoft YaHei", sans-serif;
--font-num: "SF Mono", "Inter", ui-monospace, "Helvetica Neue", monospace; /* 仅用于金额/编号 */
```

### 移动端阶梯

| 角色 | 字号 / 字重 / 行高 |
|---|---|
| 页面大标题 | 26 / 800 / 1.25 |
| 区块标题 | 18 / 700 / 1.4 |
| 卡片标题 | 16 / 600 / 1.45 |
| 正文 | 15 / 400 / **1.65** |
| 辅助说明 | 13 / 400 / 1.6 |
| 标签 / 徽章 | 11 / 600 / 1.3 |
| 金额（大） | 28 / 800 / 1.1，用 `--font-num` |
| 按钮 | 16 / 600 / 1 |

### B端阶梯

| 角色 | 字号 / 字重 |
|---|---|
| 页面标题 | 22 / 700 |
| 卡片标题 | 15 / 600 |
| 表格头 | 13 / 600，`letter-spacing:.02em`，文字次 |
| 表格体 | 14 / 400 |
| KPI 数值 | 30 / 700，`--font-num` |
| KPI 标签 | 12 / 500，文字弱 |

**数字规范**：金额、数量、百分比一律用 `--font-num` + `font-variant-numeric: tabular-nums`，保证列对齐；金额带千分位，单位（元 / % / 单）比数字小 2 号且颜色降一档。

---

## 4. 圆角 / 阴影 / 动效

```css
--r-xs: 6px;    /* 标签、徽章 */
--r-sm: 10px;   /* B端控件 */
--r-md: 14px;   /* 移动端按钮、输入框 */
--r-lg: 18px;   /* 卡片 */
--r-full: 999px;/* pill、头像 */

--sh-1: 0 1px 2px rgba(16,24,40,.05);                 /* B端卡片：配合 1px 描边 */
--sh-2: 0 4px 12px rgba(16,24,40,.07);                /* 移动端卡片 */
--sh-3: 0 8px 24px rgba(16,24,40,.10);                /* 浮层、底部固定条 */
--sh-4: 0 12px 24px rgba(16,24,40,.10);               /* 上限，超出即错 */

--ease: cubic-bezier(.22,1,.36,1);
--t-fast: 120ms;
--t-base: 200ms;
--t-page: 260ms;   /* 页面转场 */
```

**B端卡片**：`background:#fff; border:1px solid #EDEFF3; box-shadow:var(--sh-1)` —— 靠描边分层，不靠大阴影。
**移动端卡片**：`background:#fff; border-radius:18px; box-shadow:var(--sh-2)` —— 靠圆角 + 轻阴影分层。

---

## 5. 组件规格

### 5.1 按钮

| 类型 | 移动端 | B端 |
|---|---|---|
| 主按钮 | 高 52 / 圆角 14 / 主色填充 / 白字 16-600 | 高 36 / 圆角 10 / 主色填充 / 白字 14-600 |
| 次按钮 | 高 52 / 主色 10% 底 / 主色字 | 高 36 / 白底 + 描边 / 主色字 |
| 文字按钮 | 主色字 15-600，无底 | 主色字 14-500 |
| 危险按钮 | `#F04438` 填充 | 同左 |
| 禁用 | 底色 `#EDEFF3` + 文字 `#C2C7D0`，**不降透明度** | 同左 |

交互：`:active { transform: scale(.97) }`（移动端）/ `:hover { background: 主色-hover }`（B端）。移动端**禁用 hover 依赖**。

### 5.2 输入框

```
高度 48 / 圆角 12 / 底 #F5F6FA / 无边框
focus: 底 #fff + 1.5px 主色描边 + 3px 主色 12% 外发光
error: 1.5px #F04438 描边 + 下方 12px 红字错误提示
```

B端：高度 36 / 圆角 10 / 白底 + 1px `#EDEFF3` 描边，focus 同主色。

### 5.3 标签 / 徽章

- **Pill 标签**：高 24 / 圆角 full / 主色 10% 底 + 主色字 / 12px-600
- **状态徽章**：语义色浅底 + 语义色字，前置 6px 圆点
- **数字角标**：红底白字，min-width 18，圆角 full

### 5.4 列表 / 表格

移动端列表行：`padding:14px 20px`，底部分隔线用 `border-bottom:1px solid #EDEFF3` 且**左缩进 20px**（`background-clip` 技巧），右侧 chevron 12px 灰。

B端表格：
- 表头底色 `#FAFBFC`，文字次色 13-600
- 行 hover 底色 `#FAFBFC`
- **数字列右对齐**，文本列左对齐，操作列右对齐
- 排序：点列头切换 升 / 降 / 无，当前排序列头显示箭头且主色
- 空态：居中插画 + 一句说明 + 一个主按钮

---

## 6. 移动端骨架（可直接抄）

```html
<div class="phone">
  <div class="status-bar">9:41 ⟋ 5G ▮</div>
  <div class="app">
    <header class="app-bar"><!-- 返回 / 标题 / 操作 --></header>
    <main class="app-body"><!-- 可滚动 --></main>
    <nav class="tabbar"><!-- 4-5 项，active 主色 --></nav>
    <div class="safe-bottom"></div>
  </div>
</div>
```

页面栈：`.view { position:absolute; inset:0; }`，切换用 `transform:translateX()` + `opacity`，`--t-page` 时长。

## 7. B端骨架

```html
<div class="shell">
  <aside class="sidebar"><!-- logo + 菜单，收起 64px --></aside>
  <div class="main">
    <header class="topbar"><!-- 面包屑 / 搜索 / 用户 --></header>
    <div class="content">
      <div class="kpi-grid"><!-- 4 张 KPI 卡 --></div>
      <div class="chart-grid"><!-- 折线 + 柱状 / 环形 --></div>
      <section class="table-card"><!-- 筛选 + 表格 + 分页 --></section>
    </div>
  </div>
</div>
```

## 8. 可访问性底线

- 正文与背景对比度 ≥ 4.5:1，弱化文字 ≥ 3:1
- 可点区域 ≥ 44×44（移动端）/ 32×32（B端）
- 焦点可见：`:focus-visible { outline:2px solid 主色; outline-offset:2px }`
- 动效包在 `@media (prefers-reduced-motion: reduce)` 里归零
- 图标按钮必须带 `aria-label`
