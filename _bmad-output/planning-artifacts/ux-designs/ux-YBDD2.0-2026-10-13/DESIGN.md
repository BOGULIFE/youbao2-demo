---
name: FR-022 右豹创作者首页金刚区
description: C 端首页可配置快捷入口的视觉设计合同；复用 FR-021 橙系首页 token。
status: final
created: 2026-10-13
updated: 2026-10-13
topic: FR-022 首页金刚区与点击分析
sources:
  - _bmad-output/planning-artifacts/prds/prd-YBDD2.0-2026-10-13/prd.md
  - _bmad-output/planning-artifacts/ux-designs/ux-YBDD2.0-2026-10-07/DESIGN.md
  - _bmad-output/planning-artifacts/ux-designs/ux-YBDD2.0-2026-10-07/EXPERIENCE.md
  - C:\Users\Administrator\AppData\Local\Temp\codex-clipboard-f2523a00-8136-46d8-9ad8-0a16b219f122.png
  - C:\Users\Administrator\AppData\Local\Temp\codex-clipboard-ca6dfdb2-b7ce-483b-82ba-f673a3289e53.png
  - C:\Users\Administrator\AppData\Local\Temp\codex-clipboard-c80e6265-342f-47a4-8e14-6b992611525c.png
---

# DESIGN · FR-022 首页金刚区

**冲突规则：** 本文件只定义 FR-022 金刚区的视觉增量；未覆盖的首页视觉沿用 [FR-021 DESIGN](../ux-YBDD2.0-2026-10-07/DESIGN.md)。本文件与 FR-022 `EXPERIENCE.md` 优先于 mock；与 FR-022 PRD 冲突时 PRD 优先。

## Brand & Style

金刚区是右豹首页的“快捷行动”层，不是第二个 Banner。它沿用首页暖橙、浅灰页面底和白卡片语言，通过稳定的图标容器和短标题让用户快速扫描。入口之间保持同等视觉权重，不使用高饱和大面积渐变制造人为优先级；真正的业务优先级由后台排序决定。

视觉目标：轻、紧凑、可扫读。金刚区不能压过顶部 Banner、三格收益数据或学院/活动双卡，也不能让配置文案改变首页结构高度。

## Colors

- `brand-orange`：`#ff4d22`，入口激活、可交互状态和少量强调。
- `brand-orange-hover`：`#ff6c46`，按压/悬停反馈。
- `brand-orange-soft`：`#fff1ec`，默认图标容器或入口底色。
- `text-primary`：`#252836`，入口标题。
- `text-secondary`：`#9498a8`，副标题和辅助状态。
- `text-muted`：`#9aa0ad`，不可用或降级提示。
- `line-default`：`#eceef3`，模块边界和分隔线。
- `surface-page`：`#f6f7fb`，首页背景。
- `surface-card`：`#ffffff`，金刚区容器。
- `accent-red`：`#ff3157`，仅用于配置提供的角标或告警状态，不作为默认入口底色。

入口图标可按业务使用多色图标，但每个图标必须置于统一尺寸容器中；禁止后台传入颜色覆盖整个入口卡片。

## Typography

- 字体沿用 `{typography.font-family-base.fontFamily}`。
- 模块标题（如使用）沿用 `{typography.title-md}`：15px / 800。
- 入口标题：12px / 700，单行，超出省略。
- 入口副标题：9px / 500，单行，超出省略；为空时不占位。
- 辅助状态：`{typography.caption}`，不依靠颜色单独表达错误或禁用。

## Layout & Spacing

- 手机画布最大宽度 `{spacing.phone-max-width}`：390px。
- 模块左右内边距沿用 `{spacing.page-gutter}`：12px。
- 金刚区与前后模块间距沿用 `{spacing.section-gap}`：12px。
- 金刚区容器建议内边距：12px；入口网格列间距：8px；行间距：12px。
- 建议 4 列布局，入口单元最小宽度 64px；最大 8 个入口，即 2 行。
- 入口单元高度固定：72px；图标容器 36×36px；标题与副标题区域保留固定基线。
- 入口数量少于 4 个时保持左对齐，不拉伸入口宽度；不使用空白占位格模拟未配置入口。
- 入口超过 8 个时默认不在 C 端继续加高；具体裁剪/更多规则由 PRD 开放项确认，UX 当前按后台限制为最多 8 个设计。
- 金刚区不影响底栏和今日路径气泡的安全区；页面滚动时随首页内容滚动，不固定悬浮。

## Elevation & Depth

- 金刚区容器沿用首页白卡轻阴影：`0 3px 10px rgba(45,55,85,.025)`。
- 圆角沿用 `{rounded.md}`：12px；不使用超过 `{rounded.lg}` 的营销大卡圆角。
- 图标容器使用浅色填充，不额外叠加浮雕或强阴影。
- 按压反馈使用轻微透明度/背景变化，避免入口跳动。

## Shapes

- 金刚区容器：`{rounded.md}`，白底。
- 图标容器：10px 圆角；图标资源自身可为圆形或方形，由业务图标决定。
- 入口不可使用胶囊按钮形状承载标题；入口是图标 + 文本的扫描单元。
- 错误反馈采用 Toast 或页面级轻提示，不在金刚区内生成高大的错误卡片。

## Components

### 金刚区容器

- 可选模块标题“快捷入口”或由产品最终确认是否省略；默认不添加营销副标题。
- 白底、12px 圆角、12px 内边距。
- 只有至少 1 个有效入口时渲染。
- 入口排序严格跟随后台下发 `sort`，前端不再二次排序。

### 金刚区入口单元

- 结构：图标容器 → 标题 → 可选副标题。
- 图标容器固定 36×36px，资源使用 `object-fit: contain`。
- 标题单行省略；副标题单行省略。
- `aria-label` 优先使用标题，存在副标题时拼接为“标题，副标题”。
- 按下时图标容器背景使用 `{colors.brand-orange-soft}` 的深一阶反馈或 96% 透明度，不改变布局。
- 单元不显示独立点击次数、人数或配置版本，统计信息不进入 C 端 UI。

### 入口图标资源

- 后台配置的图标需要经过尺寸和格式校验；前端对加载失败提供默认入口图标或隐藏单个入口，具体降级由体验合同定义。
- 图标不得携带影响可读性的文字密图；文字信息由标题承担。

### 加载与空态

- 首屏尚未返回配置时可使用 4 个等宽浅灰 skeleton 单元，但 skeleton 不应撑出超过最终模块高度。
- 配置为空、接口失败或入口全部非法时不渲染金刚区；不展示“暂无入口”大空态。
- 单个图标加载失败不影响其他入口。

## Do's and Don'ts

- **Do** 把金刚区放在 Banner 后、三格数据前，作为首屏快捷行动层。
- **Do** 用统一图标容器和固定高度保证后台文案变化不会让首页跳动。
- **Do** 让入口排序决定业务优先级，视觉上保持入口同权。
- **Do** 使用标题和副标题表达信息，图标只做识别辅助。
- **Don't** 把金刚区做成轮播或大 Banner。
- **Don't** 把点击次数、点击人数、用户信息或报表入口放到 C 端入口单元。
- **Don't** 让一个入口使用全宽色块、强动效或持续呼吸动画抢占首页注意力。
- **Don't** 让配置失败阻塞 Banner、三格数据或后续首页内容。
- **Don't** 使用颜色作为唯一的禁用、错误或状态表达。

