# LOCAL-ADDON · 本地增补说明（hjhsky fork）

本文件是本 fork 相对上游 op7418 原版的全部增补的底稿，与 SKILL.md 末尾「本地增补」节内容一致。上游同步后若增补被覆盖，照本文件重新附加即可。

## 增补内容

- `scripts/render-shot.mjs`：全页长截图脚本（新增文件）。
- `references/layouts-longform.md`：长页版式规范（新增文件）——字号阶梯、间距系统、九类组件骨架、密度下限、交付自检清单。补上增补节第 1 条缺失的「版式」层约束。
- `SKILL.md` 末尾「本地增补 · 渲染与交付规矩」一节（原文未改，仅追加）。

## 四条规矩

1. 长页形态：长文、评审稿、讲义做一页到底的长页，沿用本 Skill 设计语言，不硬塞翻页版式。**版式执行 `references/layouts-longform.md`**。
2. 固定画布：viewport 锁设计宽，禁止响应式断点，设备只等比缩放不重排；关键内容静态写死。
3. 渲染自检：交付前必须用 scripts/render-shot.mjs 渲全页 PNG 并逐段目检（溢出、截断、错位、丢图、对比度），修完重渲再交付；只交 HTML 视为未完成。
4. 内容保真：先提取全文与原图列清单，排版后逐项核对；不概括、不删节、不改数字；原文矛盾只指出不擅改。

## 为什么补 layouts-longform.md

deck 形态有 `assets/template-swiss.html` + 22 个登记版式（S01–S22）+ `data-layout` 必填，产出高度可复现。长页形态原本只有「沿用设计语言（配色、网格、字体）」一句，**版式完全由 Agent 自由发挥**——实测同一 skill、同一题材在不同会话产出风格差异明显（有无导航栏、KPI 几栏、字号阶梯、间距全都不同）。

本文件把长页版式固定下来，让长页产出可复现。规格来源：2026-10-18 三份实测长页（五胡乱华、五四运动、印尼 1998 教案）的收敛值，密度下限参考 social-card validator 的 R5 density 规则。

## render-shot.mjs 用法

```bash
# 在仓库根目录安装依赖（playwright-core，不下载浏览器包）
npm i playwright-core   # 社交卡仓库已有 playwright 依赖，npm install 亦可

node scripts/render-shot.mjs <输入.html> <输出.png> [宽度=1080] [倍率=2]
```

脚本自动探测系统浏览器，**已覆盖 Edge**（macOS 与 Linux 均含）、Chrome、Chrome Canary、Chromium、Brave；探测不到时再用 `CHROME_PATH` 环境变量指定可执行文件。

```bash
# 一般直接跑即可，Edge 机器无需额外变量
node scripts/render-shot.mjs <输入.html> <输出.png> 1080 2
```

不要改用 Chromium 命令行截图（部分环境损坏），不要走 Playwright 官方浏览器下载（CDN 易中断）。
