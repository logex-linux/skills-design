---
name: xinxiangsu-product-ui
description: "把「新像素系」视觉语言落到真正能跑的产品界面上 —— 移动端 App 与 B端后台的 HTML/CSS/JS 实现。当用户要写真实可交互的产品 UI（不是作品集海报）、要移动端 App 界面、要 B端工作台/数据看板、要把设计稿做成能点的原型、或说『用新像素那套风格做个能跑的界面』『作品集里的界面要真的能交互』时使用。产出单文件、可直接打开的交互式 HTML。"
description_zh: "新像素系产品界面实现：移动端 App + B端后台，真可交互的单文件 HTML"
description_en: "Product UI implementation in Xinxiangsu visual language: interactive single-file HTML for mobile & B-end"
display_name: "新像素系产品界面实现"
display_name_en: "Xinxiangsu Product UI"
visibility: "public"
allowed-tools: Read,Write,Bash
agent_created: true
---

# 新像素系产品界面实现

> 姊妹 skill：`xinxiangsu-uiux-portfolio` 负责**作品集长图**（可截图的展示页）。
> 本 skill 负责**能跑的产品界面**。两者共用同一套视觉语言，但 token 与写法完全不同 —— 不要混用。

## 与作品集 skill 的分工

| | 作品集长图 | 产品界面（本 skill） |
|---|---|---|
| 产物 | 1716px 海报，截图为交付 | 可交互 HTML，点击可走流程 |
| 尺寸 | 固定宽、高度自由生长 | 响应式（390 宽移动端 / ≥1280 桌面端） |
| 内容 | 设计说明、研究过程、Before/After | 真实功能、真实数据、真实状态 |
| 字体 | 标题 64–96px 表演型 | 移动端标题 22–28px，B端 20–24px |
| 阴影 | 可到 32px | **移动端 ≤ 12px，B端卡片用描边而非阴影** |
| 装饰 | 水印字、3D 吉祥物、手绘箭头 | 一律去掉，只留品牌色和圆角 |

**记住：作品集里的样机截图 = 本 skill 的产物。** 先做产品界面，再拿去贴进作品集，这才是正常工序。

## 何时使用

- 要能点、能跳转、能筛选的产品原型
- 移动端 App 界面（带底部 tab、页面栈、动效）
- B端工作台 / 数据看板 / 管理后台
- 「把设计稿做成能跑的原型」「要能发给别人点开看的那种」

**不适用**：海报、作品集长图、PPT、营销落地页（那些走 `xinxiangsu-uiux-portfolio` 或 `awesome-design-md`）。

## 技术约束（硬性）

1. **单文件 HTML**，内联 CSS/JS，零外部依赖、零 CDN、零图片文件。所有图标用**内联 SVG**，所有插画/图片位用 **CSS 渐变或 SVG 绘制**。
2. 字体只用系统栈，不引外部字体。
3. 移动端 demo 用 `.phone` 容器模拟机身（390px 宽），页面在容器内切换；B端 demo 全宽铺满，侧边栏 + 内容区。
4. 交互必须有：**页面栈切换（带转场）、tab 切换、表单状态、空/加载/错误态至少各一处**。
5. 交付前必须用无头浏览器截图自查渲染结果，不能凭代码判断。

## 工作流

1. **读规范**：先读 `references/product-tokens.md`（两端的 token 与组件规格）和 `references/ui-patterns.md`（页面模式）。
2. **定主色**：从 `product-tokens.md` 的选型表按品类选一个主色。全套只 1 个主色 + 1 个强调色。
3. **列页面清单**：移动端建议 4 屏（首页 / 详情 / 表单确认 / 结果），B端建议 1 屏工作台（侧栏 + KPI + 图表 + 表格）。
4. **出稿**：复制 `assets/app-demo.html` 或 `assets/dashboard-demo.html` 改内容，**不要从零写版式**。
5. **自查**：跑验收清单 + 无头浏览器截图。
6. **（可选）贴回作品集**：把渲染出的界面截进 `xinxiangsu-uiux-portfolio` 的样机位。

## 验收清单

- [ ] 零外部依赖：断网打开依然完整可用
- [ ] 每个可点元素都有 `:active` 反馈（缩放 0.97 或背景变化）
- [ ] 页面切换有转场（≤260ms，`cubic-bezier(.22,1,.36,1)`）
- [ ] 移动端所有可点区域 ≥ 44×44 px
- [ ] 正文对比度 ≥ 4.5:1；禁用态文字 ≥ 3:1
- [ ] 有 `prefers-reduced-motion` 兜底
- [ ] 列表有 3 条以上真实数据，不用 Lorem / 占位符文字
- [ ] 表单有校验反馈（至少一处错误态）
- [ ] B端表格支持点列头排序，数字列**右对齐**
- [ ] 涨跌配色遵循中国习惯：**涨红跌绿**
- [ ] 移动端安全区：底部 tab 预留 `env(safe-area-inset-bottom)`

## 反面清单

- ❌ 直接缩放作品集海报当产品界面（字号、间距、阴影全不对）
- ❌ 卡片阴影超过 `0 12px 24px rgba(0,0,0,.10)`
- ❌ 用 emoji 当功能图标
- ❌ 界面里出现「点击此处」「示例文本」「Lorem ipsum」
- ❌ B端表格数字左对齐、金额不带千分位
- ❌ 移动端用 hover 做唯一反馈（触屏没有 hover）
- ❌ 引入 Tailwind / Bootstrap / 任何 CDN

## 素材

| 文件 | 说明 |
|---|---|
| `references/product-tokens.md` | 色彩 / 字体 / 间距 / 圆角 / 阴影 / 栅格 / 组件规格（移动端 + B端两套） |
| `references/ui-patterns.md` | 8 类页面模式：首页信息流 / 详情页 / 表单页 / 结果页 / 列表表格 / 数据看板 / 空状态 / 设置页 |
| `assets/app-demo.html` | 移动端 App 四屏可交互 demo（清野露营预约） |
| `assets/dashboard-demo.html` | B端工作台可交互 demo（营地运营后台） |
