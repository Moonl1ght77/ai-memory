# David Lynch Tribute 拆解

> 网址：https://davidlynch.uprock.pro/ ｜ 类型：致敬专题长页（设计学校 Uprock 出品，单页滚动叙事；Awwwards Honorable Mention 2025-11-02） ｜ 拆解：2026-11-05 ｜ 证据：桌面 56 帧、手机 13 帧（扫 2 次，第 2 次 `--steps=104` 才滚到 69003px 底）、CSS/JS 49 个文件（13 CSS + 36 JS，含 14.9 KB 主脚本 inline-script-2）

## 一句话
把林奇的 6 部作品做成一卷往下滚就往后放的片子：每章一张巨型红字片名卡，接一段被滚动逐帧拖着走的黑白颗粒画面；全站只有黑、白、红三色（Blue Velvet 一章换成蓝），颗粒、尘点、网点全部长在素材里。

## 架构与技术栈
- 框架 / 渲染：Taptop 无代码建站（`<meta name="generator" content="Taptop">`，资源名 tt_default_styles.css、do.*.js），jQuery 1.10.2；纯 DOM，没有 canvas、没有 WebGL（summary.canvases 为空）。原生滚动，桌面页高 69003px（约 77 屏），手机 20911px。
- 动效 / 滚动 / 3D / 声音：GSAP 3.12.5 + ScrollTrigger（js-16、js-17）；lottie-web 5.12.2，SVG 渲染器（js-15）；SplitType（拆字变色）。无平滑滚动库，无声音。
- 托管与内容来源：nginx（响应头）；内容写死在页面。5 段 Lottie JSON 走 XHR：er-head-fast 6.77 MB、final-fast 5.30 MB、roses-fast 4.03 MB、elephant 3.80 MB、tp3 1.54 MB，合计约 21.4 MB；另有 68 张 WebP 共 3.3 MB。12 个 `/f/*_1920.mp4` 全部返回 404（requests.json），视频段现在是坏的。
- 页面结构（按截图）：黑屏预载（手机显示 WELCOME TO THE RED ROOM）→ 满屏红字 DAVID LYNCH + 「THE ART LIFE / ERASERHEAD」字距漏斗 → Eraserhead：点描网点人头 + 红字长文 → 水泥墙走廊长镜头（Lottie 逐帧）→ Twin Peaks：人字纹地板、红丝绒幕、带片孔边框的白卡 → 「fire walk with me」→ The Elephant Man：被 9 根竖条切开的颗粒黑白画面 + 红字片名 → 竖条胶片带 → Blue Velvet：蓝天白篱红玫瑰、蓝色抖动网点帧 → 浅灰区「A WINDING CINEMATIC JOURNEY」+ 引言 → 「LAND OF DREAMS」夜路车灯 → 片尾 DAVID LYNCH → 「THE END?」。
- 路由与转场：单页；外链只有 uprock.ru、t.me、taptop.pro。点 THE END? 整页淡出再淡入（见动效）。

## 视觉与配色
| 角色 | 色值 | 用在哪 |
|---|---|---|
| 底色 | #000000 | html 背景、绝大多数章节 |
| 主强调 | #FF0000 | 片名、长文、引言（文字色出现 771 次） |
| 章节色 | #2600FF | Blue Velvet 一章的正文（698 次），换掉红色 |
| 纸白 | #FFFFFF / #EDEDED | 片孔说明卡、「A winding cinematic journey」浅色区 |
| 深灰字 | #212121，及 30% 透明 | 浅色区正文 |
| 胶片缝 | #0D0D0D | 竖条胶片带的缝 |

## 字体与排版
| 角色 | 字体 | 字号 1440 / 390 | 字重 | 行高 / 字距 | 大小写 |
|---|---|---|---|---|---|
| 章节巨字（fire walk with me） | Inter | 285px（19.8vw）/ 看图约 90px | 800 | 0.54 / −0.05em | 全小写 |
| 片名（blue velvet） | Inter | 172px（11.9vw）| 800 | 1.0 / −0.11em | 全大写 |
| 副标题（land / the elephant man） | Inter | 120px（8.3vw）| 700 | 0.9 / −0.06em | 全大写 |
| 引言 | Inter | 74px | 700 | 1.0 / −0.02em | 全大写 |
| THE ART LIFE 漏斗 | Inter | 46px（3.2vw）| 700 | 1.0 / 87→73→58→44→30→15px→0 | 全大写 |
| 正文 | Inter | 24px（1.67vw）/ 看图约 14px | 700 | 0.9 / −0.06em | 每词首字母大写 |
| 小字 | Inter | 18–22px | 700 | 0.9 / −0.06em | 全大写 |

授权：Inter，开源（OFL），Google Fonts 经 WebFont.load 加载 200/400/500/700/800。
- 排版手法：全站一套 Inter，靠字号和字距撑戏。巨字行高压到 0.54，上下行互相咬住；字距全负；正文也用 700 粗，不出现细字。字号全写 vw（6.25vw、3.75vw、2.5vw、1.25vw 最常见），整页随宽度等比缩放。同一行字重复 7 次、字距逐行递减，排成漏斗当片头字幕。文字叠在画面上时用 `mix-blend-mode: exclusion / difference`（.md__text-exclusion、.tp-hs__text-center）。

## 布局
- 版式：通栏场景串联，没有栅格容器；文字块只有 620–790px 几个 max-width。The Elephant Man 一段用 9 根黑缝把画面切成竖条，像胶片带也像百叶窗。
- 断点：479 / 767 / 991 / 1279 / 1439 / 1919（Taptop 默认），1280 以上的规则最多（789 条）。
- 手机：章节顺序不变，竖屏大字压满屏宽；Lottie 换一套手机触发点（见动效）。

## 动效
| 触发 | 效果 | 时长 | 缓动 | 实现 |
|---|---|---|---|---|
| 开场 | 预载层（`.preloader__fade` 500% 宽的黑→透明横向渐变）后 content 加 is-loaded，再移除预载层 | 2000ms / 3000ms | — | inline-script-2 setTimeout |
| 滚动 | Lottie 逐帧：滚动进度线性映射到第 0 帧 → 最后一帧，`goToAndStop(frame, true)`；容器按 viewBox 比例做 cover 裁切 | 跟滚动，scrub 1 | none | GSAP timeline + ScrollTrigger |
| 滚动 | 五段触发区间：Eraserhead `top 120%`→`bottom bottom`；Twin Peaks `top bottom+=200%`→`bottom bottom-=200%`；Elephant `top top`→`bottom bottom`；Blue Velvet `top top-=370%`→`bottom bottom-=200%`；片尾 `top top`→`bottom bottom-=150%` | — | — | inline-script-2 常量 |
| 滚动 | THE ART LIFE 漏斗：字距 1.58em→0、x 3.1%→0（后几行 start 提前 10%、20%） | scrub 0.7 | linear | gsap.to(.tal__text-N) `top bottom`→`bottom bottom` |
| 滚动 | 标题拆字（SplitType chars）逐字变色 | scrub 0.6，`top top`→`top top-=120%` | linear，stagger from start | inline-script-2 |
| 显隐 | 块元素位移 / 透明度过渡 | 1000ms | cubic-bezier(0.59, 0.28, 0.25, 0.99) | CSS transition（4 处） |
| 悬停 | 说明层淡入 | 200–300ms | ease-in-out / cubic-bezier(.4,0,.22,1) | CSS |
| 点击 | THE END?：content 去 is-loaded、所有 .page-fade-anim 加 is-faded，1ms 后恢复，1000ms 后去掉淡出 | 1000ms | — | inline-script-2 |

## 交互与组件
- 导航：没有导航条、没有菜单（summary.nav 为空），整站只能往下滚，唯一按钮是片尾 THE END?。光标默认。
- 加载：桌面前 9 秒基本是黑屏（等 21 MB JSON），没看到进度提示；手机显示一行红字 WELCOME TO THE RED ROOM。
- 声音：无。视频 12 个全 404，页面没有兜底图。
- 减少动态效果：CSS 有 1 条 `prefers-reduced-motion`（建站器默认），JS 里的滚动动画没有对应开关。
- 手机：同一套故事；Lottie 触发点改成 `top top-=100%` / `top top+=300%` 这类，平板也走手机版 Lottie。

## 质感
- 颗粒做法：**没有任何代码层颗粒**（summary.grain：无 feTurbulence、无 grain/noise 类、无全屏叠层；hits.noiseFn 为空）。颗粒、白色尘点、划痕全部烧在素材里：5 段 Lottie JSON 内嵌位图序列（The Elephant Man 段颗粒最粗，带白尘点，见帧 desktop-scroll-38/40）；Eraserhead 人头是点描网点图 head-top.webp（435 KB）；Blue Velvet 段是蓝色抖动网点帧（desktop-scroll-66/68）。
- 参数：无运行时参数；刷新率 = 滚动速度（scrub 1，约 1 秒追上）；静止不滚时颗粒不动。
- 性能：运行时零噪声计算，代价全在下载：约 21.4 MB JSON + 3.3 MB 图；Lottie 用 SVG 渲染位图帧，长段落滚快了会掉帧（推测，没测）。
- 其他：大面积纯黑 + 纯红，没有暗角、没有模糊。

## 可借鉴到巡航永恒
- 学：滚动逐帧放一段黑白颗粒画面（frame = 滚动进度，scrub 1，ease none）→ 首页「一部短片」的冷启动、第一次上路那几镜。素材改成 AVIF / WebP 帧序列画到 canvas，或视频 currentTime 拖动，别用 Lottie 塞位图。
- 学：每章一张巨型片名卡（Inter 800 这一级的粗无衬线，行高 0.54、字距 −0.05em，满宽 19.8vw）→ X48 / FAT BOY / BREAKOUT 三章的片名卡；颜色换成纸白，强调色另选（避开橙）。
- 学：同一行字重复、字距逐行递减排成漏斗，滚动时收拢（1.58em→0，scrub 0.7）→ 片头字卡 ETERNAL CRUISE / 巡航永恒。
- 学：9 根黑缝把画面切成竖条 → 发动机年代史时间轴的分隔，像胶片带。
- 学：每章换一个底片色调（红→蓝）→ 三台车各一种黑白调子（冷灰 / 暖灰 / 高反差），不加彩色。
- 不学：Taptop + jQuery 1.10 堆出来的工程；21 MB JSON 首屏黑 9 秒；视频 404 没兜底；正文 24px 全粗体、负字距，长文难读；没有减少动态效果的开关。

## 没读到
- 12 个 mp4 全部 404，原本的视频内容和位置看不到。
- 滚动追帧的手感（scrub 1 的拖尾、快速滚动掉不掉帧）要真机看；4G 下 21 MB 要等多久没测。
- 手机字号只能看图估；Lottie 每段多少帧、帧尺寸没拆 JSON 去数。
- 网点人头是设计师预先做的图还是运行时处理：推测是预处理（WebP 里已是网点），没见对应代码。
</content>
</invoke>
