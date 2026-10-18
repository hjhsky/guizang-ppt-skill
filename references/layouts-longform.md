# 长页版式规范 · Longform Layouts

> 适用范围：本文件是 `SKILL.md` 增补节第 1 条「长页形态」的落地细则。
> 增补节规定了长页的**配色、网格、字体**三项约束；本文件补上缺失的第四项——**版式**。
> 配套 `LOCAL-ADDON.md` 同属本 fork 增补，上游同步若覆盖增补节，以本地两文件为准重新附加。

## 为什么需要这份文件

deck 形态有 `assets/template-swiss.html` + 22 个登记版式（S01–S22）+ `data-layout` 必填，几乎不可能跑偏。长页形态原本只有「沿用设计语言」一句话，**版式完全由 Agent 自由发挥**——结果是同一个 skill、同一个题材，不同会话产出长得不一样。

本文件把长页的字号阶梯、间距系统、组件骨架与密度下限固定下来，让长页产出可复现。

---

## 一、适用判断

| 内容形态 | 用什么 |
|---|---|
| 讲义 / 评审稿 / 报告 / 长文章 / 教案 / 白皮书 | **本文件 · 长页** |
| 有观众、需要放映的演讲 | 翻页 deck（`layouts-swiss.md` + S01–S22） |
| 社交媒体图组、竖版卡片 | social-card skill |

判断口诀：**读完再说一遍的，用长页；站着讲的，用 deck。**

---

## 二、画布与骨架

```html
<!doctype html>
<html lang="zh-CN">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=1080">   <!-- 锁死，禁止 device-width -->
<style>
:root{ /* 色值全部取自 references/themes-swiss.md，灰阶跨主题统一，只换 accent */
  --paper:#fafaf8; --ink:#0a0a0a;
  --grey-1:#f0f0ee; --grey-2:#d4d4d2; --grey-3:#737373;
  --accent:#002FA7;
  --sans:"Inter","Helvetica Neue",Helvetica,"Noto Sans SC","PingFang SC","Microsoft YaHei",sans-serif;
  --mono:"IBM Plex Mono","SF Mono",Menlo,Consolas,monospace;
  color-scheme:light;
}
.page{width:1080px;margin:0 auto;background:var(--paper);padding:72px 80px 64px}
@media print{body{background:#fff}.page{margin:0 auto}}   /* 仅允许这一个断点 */
</style>
</head>
<body><main class="page"> … </main></body>
</html>
```

**硬规矩**：
- `viewport` 写 `width=1080`，**禁止** `width=device-width`
- **禁止**任何 `@media (max-width: …)` 断点。设备差异只允许浏览器等比缩放
- 关键内容静态写死在 HTML 里，**禁止**用 JS 动态生成
- 正文 `background` 用 `--paper`，页面外框用 `--grey-2`（制造纸张浮起的观感）

---

## 三、字号阶梯

从大到小，**越大越细**（瑞士风核心）。下限不得突破。

| 角色 | 字号 | 字重 | 说明 |
|---|---|---|---|
| `h1` 主标题 | **104px** | **200** | 超长标题降至 92px；行高 1.12 |
| `h1 .em` 标题内强调 | 104px | 600 | 全页 ≤ 1 处，用于强调年份或关键数字 |
| `.kpi .num` 数据大字 | **84px** | **200** | line-height 1，字距 -0.03em |
| `.kpi .num em` 单位 | 32px | 300 | accent 色，紧跟数字 |
| `.sec-nb` 章节序号 | **76px** | **200** | accent 色，line-height 0.9 |
| `.pull .q` 黑底引文 | 36px | **200** | 2–3 行 |
| `.sec-head h2` 章节标题 | 38px | **500** | line-height 1.3 |
| `.lead` 导语 | 28px | **500** | max-width 27em |
| `p.body` 正文 | **23px** | **400** | line-height 1.72，max-width 40em |
| `.sub` 副文本 | 22px | **400** | grey-3 色 |
| `.card .lead` 卡片主文 | 22px | **500** | |
| `.quiz li` 讨论题 | 22px | **400** | |
| `.steps .w` 环节名 | 27px | **500** | |
| `.tl .what` 时间轴事件名 | 26px | **500** | |
| `.g4 h3` 四栏标题 | 26px | **500** | |
| `footer .srcs` 脚注 | 17px | **400** | grey-3 色 |
| `.chrome-min` / `.meta-row` | 18px / 16px | 500 | mono，uppercase，字距 0.1–0.18em |
| `.t-cat` / `.sec-head .en` / `.kpi .lbl` | **15px** | **500–600** | mono，uppercase |

**绝对下限**：`p.body` ≥ 18px，卡片描述/表格/脚注 ≥ 16px，meta/标签 ≥ 14px（对齐 Swiss 演示最小字号）。**放不下就删文案或拆 section，不要压字号。**

---

## 四、间距系统

| 位置 | 值 |
|---|---|
| `section` 上下间距 | **84px** |
| `.sec-head` 下边距 | 34px |
| `.sec-head` 上边线 | 3px solid ink（章节头）/ 2px（区块内表格、时间线） |
| 内容块之间（grid 间距） | 28–32px |
| 卡片内边距 | 26px 28px |
| `.page` 上下 padding | 72px / 64px |
| `.page` 左右 padding | 80px |
| `.callout` 外边距 | 34px 0 |
| 黑底 `.pull` 外边距 | 84px 0 0 |

**间距只用 8 的倍数或上表的值**，不要随手写 30px、45px 这类数字。

---

## 五、组件骨架

### 5.1 顶部栏 chrome-min（可选但推荐）

长页文档常有 8000px 以上，**强烈建议加顶部锚点导航**——这是长页相比 deck 的最大优势，不用它就浪费了。

```html
<nav class="chrome-min">
  <span>Teaching Plan · 高中历史</span>
  <span class="r">1998 · INDONESIA</span>
</nav>
```
内容多时升级为多段锚点导航（如 `起因 / 经过 / 数据 / 善后 / 教学`），用 flex + 等宽字体 + accent 高亮当前项。

### 5.2 封面区

```html
<p class="kicker">Lesson Plan · 课堂教案</p>
<h1>黑色五月<br><span class="em">1998</span> 印尼反华骚乱</h1>
<div class="rule"></div>          <!-- height:2px; background:var(--ink) -->
<p class="lead">导语：<strong>用 accent 色做强调</strong>……</p>
<p class="sub">副文本，grey-3 色，交代阅读前提……</p>
<div class="meta-row"><span>课时 · <b>2 节</b></span>…</div>
```

### 5.3 数据 KPI 行

**4 栏起步；数据超过 4 条用 3×2 六格排布**（`grid-template-columns:repeat(3,1fr)`），比硬塞 4 栏更稳。

```html
<div class="kpi-row">
  <div class="kpi"><div class="num">50<em> 小时</em></div><div class="lbl">持续时间<br>5.13 — 5.15</div></div>
  …
</div>
```

⚠️ **数字语义要防误读**：并列的多个口径用 `85 / 468` + caption「两种口径」，**不要写成 `85—468`**——后者会被读成区间。

### 5.4 章节头 sec-head

```html
<div class="sec-head">
  <span class="sec-nb">01</span>
  <h2>章节标题</h2>
  <span class="en">SECTION NAME</span>
</div>
```

### 5.5 三类正文组件

**对照表 versus**——并列呈现不同口径/观点时用，**这是史料对比的核心组件**：
```html
<div class="versus">
  <div class="row">
    <p class="a">1250 人<small>官方调查委员会</small></p>
    <p class="mid">vs</p>
    <p class="b">1200 人<small>新华社报道</small></p>
  </div>
</div>
```

**时间线 tl**——事件演进，`when` 用超大字（200 字重）作视觉锚点：
```html
<div class="tl">
  <div class="row">
    <p class="when">5.12<span>TRISAKTI</span></p>
    <p class="what">6 名学生被枪杀<small>导火索</small></p>
    <p class="src">来源说明</p>
  </div>
</div>
```

**四栏 grid-4**——成因/特征并列。仅一项需强调时给该项加 `.a`（accent 上边线），**只允许一处**。

### 5.6 强调块 callout

课堂提问、逻辑链、教学提示统一用它。**别用第二种强调样式**，长页里强调块超过一种就会失去强调意义。

```html
<div class="callout">核心提问：<b>用 accent 色</b>……
<span class="src">一句话说明设计意图</span></div>
```

### 5.7 黑底引文 pull

全页 **1 处**，放在倒数第二节（意义/结论前）。≥2 处就不再是强调。

### 5.8 讨论块 quiz

accent 底白字，放全页末尾。`li::marker` 用 mono + 半透明白色。

### 5.9 脚注 footer

```html
<footer>
  <div class="srcs"><b>史料来源</b>：……<br><br><b>存疑说明</b>：①②③</div>
  <p class="mark">Title<br>2026.10</p>
</footer>
```

**存疑说明必写**：多来源数据分歧、年代争议、人名异写，逐条列出并注明采信哪一说。**分歧不得私自取舍。**

---

## 六、密度下限

deck 有 validator 测底部空白，长页没有——所以必须自查：

| 检查项 | 阈值 |
|---|---|
| 单个 section 可见内容高度 | **≥ 320px**。低于此值说明该节太空，合并或补内容 |
| 全页正文密度 | 3000px 高的长图内，正文 ≥ 3 屏 |
| 连续两个 section 之间的视觉落差 | 不应出现「上一屏很满、下一屏只有两行」 |
| 四栏 grid 单格字数 | ≥ 12 字。少于则改两栏 |

**太空不是留白美学，是内容不足。** deck 的 R5 density WARN 若移植到长页，低于 45% 基本意味着该节需要补内容。

---

## 七、交付前自检

- [ ] `viewport` 写的是 `width=1080`，不是 `device-width`
- [ ] 全文件只有 1 个 `@media`（print），无任何 max-width 断点
- [ ] 所有色值来自 `references/themes-swiss.md` 原值，未自创
- [ ] `p.body` ≥ 18px，meta/标签 ≥ 14px
- [ ] 章节序号 / 数据大字字重为 200–300，正文 400–500（**没有 600+ 的大字**）
- [ ] 每个 section 内容 ≥ 320px，无「两行就结束」的节
- [ ] 并列口径用 `/` 分隔，不用 `—`（防读成区间）
- [ ] callout 样式全页统一，黑底 pull 仅 1 处
- [ ] 脚注列全所有来源分歧，注明采信口径
- [ ] 关键内容静态写死，无 JS 动态生成
- [ ] **已用 `scripts/render-shot.mjs` 渲全页 PNG 并逐段目检，HTML 与 PNG 一起交付**

最后一条是硬门槛。**只交 HTML 视为未完成。**

---

## 八、渲染命令

```bash
# 必须设 CHROME_PATH 指向本机浏览器（脚本候选路径只含 Chrome / Chromium）
CHROME_PATH="/Applications/Microsoft Edge.app/Contents/MacOS/Microsoft Edge" \
  node scripts/render-shot.mjs <输入.html> <输出.png> 1080 2
```

渲染后**必须逐段打开图片目检**：内容溢出、底部截断、表格错位、图片丢失、深色块内文字对比度、串字（中文文案里混入英文单词是高频事故）。发现问题改完重渲，干净了才交付。