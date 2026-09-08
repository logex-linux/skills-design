# Apple Keynote Slide Style

这是一个面向产品发布会 deck 的设计 skill。它把近三年 Apple 公开发布会的资料源、视觉观察和可执行的 slide recipe 整理成一套可复用工作流。

## 文件结构

- `SKILL.md`：核心工作流、设计 token、叙事结构、实现路径与评分标准。
- `references/event-corpus.md`：8 场公开活动的来源、章节、主视觉观察、跨事件归纳和证据账本。
- `references/slide-atlas-2023-2026.md`：按事件、画面状态、密度曲线、连续 anchor 和反模板规则整理的近三年 slide atlas。
- `references/slide-recipes.md`：hero、product reveal、feature、demo、proof、comparison、platform、availability 等页面配方。
- `references/qa-checklist.md`：叙事、构图、排版、无障碍、动效、素材授权和最终抽查清单。
- `agents/openai.yaml`：Codex skill 的展示名、默认调用提示和隐式调用策略。

## 使用方式

调用 `$apple-keynote-slide-style`，并提供：

1. 产品、受众和演讲时长；
2. 要发布的功能或产品清单；
3. 输出介质（Keynote、PowerPoint、Figma、HTML/Remotion 等）；
4. 可使用的品牌素材、字体、产品图和授权范围。

如果没有指定视觉主题，skill 会先从 `event-corpus.md` 和 `slide-atlas-2023-2026.md` 中确定叙事骨架、画面状态和密度曲线，再选择一个“视觉皮肤”，但不会复制 Apple 的 logo、发布会专属图形、产品截图、音乐、视频或未授权字体。

## 研究边界

资料源以 Apple Events、Apple Podcasts、Apple Developer 和 Apple Newsroom 为主。Apple 没有在公开活动档案中提供一套可直接编辑的官方 `.key` 或 `.pptx` 模板；找到的第三方 PDF 只作为复盘材料，并在研究笔记中单独标注。
