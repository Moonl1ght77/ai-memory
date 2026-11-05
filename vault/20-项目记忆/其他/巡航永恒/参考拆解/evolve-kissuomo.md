# Made in Evolve · Kissuomo 案例页 拆解

> 网址：https://madeinevolve.com/projects/kissuomo （顺带首页 https://madeinevolve.com/ ） ｜ 类型：项目案例页（意大利 Modena 电商工作室作品集） ｜ 拆解：2026-11-04 ｜ 证据：桌面 38 帧（案例 28 + 首页 10）、手机 10 帧、CSS/JS 233 个文件（84 + 149）；另用 curl 读了 /projects/co-in 的 HTML 做对照

## 一句话
案例页是一卷放不完的片子：每章「先放镜头、最后上字卡」，最后一屏就是下一个案例的封面（同一张图），滚到底自动接着放下一部，按 CMS 顺序无限循环。

## 架构与技术栈
- 框架 / 渲染：Next.js 16.2.11（Turbopack，响应头 `x-powered-by: Next.js`、`x-nextjs-prerender`）+ React，数据走 RSC（`self.__next_f`）；类名全是 Webflow 导出的（`w-dyn-list`、`w-node-*`、`webflow-icons`、`html.w-mod-js`），推测是 Webflow 做设计和 CMS、再迁到 Next。自建 nginx/Ubuntu，媒体走 `cdn.madeinevolve.com`。
- 动效 / 滚动：GSAP 3.15.0 + ScrollTrigger + SplitText + Flip + CustomEase（js-14 `version:"3.15.0"`）；Lenis 1.3.25（`new Lenis({lerp:.1, wheelMultiplier:1.25})`，挂 `gsap.ticker`，`history.scrollRestoration="manual"`）；Bunny 视频播放器（`data-bunny-player-init`）；组件写法和 Osmo（osmo.supply）一致（`data-parallax="trigger"`、`data-split`、`data-sequence-wrap`，缓动直接命名 `osmo`）。无 3D、无声音。
- 转场：自写 React `usePageTransition`（`body.barba-leaving`、`data-barba` 只是沿用 Barba 的名字）。
- 页面结构（案例页从上到下，1440 宽全长约 15,500px）：
  1. 封面 100vh：整幅主图（视差 yPercent 0→30，scrub「top top→bottom top」）+ 顶部压暗渐变；第一行三个分类标签 + 一条细线；正中「KISSUOMO / 2012©」两行大写；左边一句话定位（约 110 字符）；右边 (a.)–(k.) 11 项服务索引（打字机等宽体，编号和服务名两端对齐）；底部正中「SCROLL TO EXPLORE」。一屏交代：谁、哪年、做了什么。
  2. 导语 604px：一段 42px 大字导语（约 320 字符）+「OVERVIEW」小标签 + 两段 14px 小字。
  3. 四章 Design / Technology / Performance / System Integration，同一模板「图先字后」：通栏大图（100vh）→ 两张方图并排（1:1，间距 1em，视频和图混排）→（部分章）左长文右小图 → 第二张大图（视差 -10→10）→ 章节字卡：左「( a. )」+ 56px 章节名，右「INTRODUCTION」标签 + 两段正文。字卡在章末，像镜头放完才上字幕。
  4. System 章插一张 200vh 高的长图（框内 120% 高，视差 -18→0，scrub 2，要滚很久才过完）+ 三个 56px KPI 数字（「+4k / x9.7 / +12%」，各配 (a.)(b.)(c.) 小标和一行说明）。
  5. 口号带 80vh：一句口号绕成圆形文字 + 6 条 SVG 线循环描线。
  6. 团队约 450px：职位（等宽大写）/ 姓名两列，11 人。
  7. 下一个案例 250vh：内层 100vh 吸顶；下一个项目封面铺满（就是下一页的首屏图，证据：`co%26IN__cover.avif` 在本页预告和 /projects/co-in 封面同名），顶部压暗；「( Next Case Study )」+ 下一个名字（与封面标题同一个 `.scroll_title` 样式）；底部 2px 进度线随滚动填满；到底自动跳下一页。列表 `tr-cmsnext-loop="true"`，最后一个接回第一个。
- 图片节奏：大（100vh 通栏）→ 小（两张方图）→ 字 → 大（视差）→ 字卡；每章 4–6 张，全页约 20 张图 / 视频（桌面图片 67 个请求 12.4MB，视频 2 个 22.8MB）。
- 首页怎么进项目页：首屏是「最新项目」轮播（3 张图，每张停 1.3s，再用 `clip-path: polygon` 从右往左擦 1.5s 换下一张，带「CASE STUDY」按钮）；往下是作品索引表（Partner / Platform / Case Studies·VIEW ALL / Service 四列，16 个项目一行一个，行末「EXPLORE」），悬停出预览图（见动效），点击走黑幕转场进案例封面。

## 视觉与配色
| 角色 | 色值 | 用在哪 |
|---|---|---|
| 页面底 | #111111（`--mie-black`） | 全站底色、压暗渐变起点 |
| 面板 | #131313（`--dark-grey`） | Cookie 面板等深色块 |
| 文字 | #FFFFFF | 全部文字 |
| 强调 | #C3FF00（`--accent` / electric-green） | 下一个案例进度线、加载指示灯、Cookie 同意按钮 |
| 栅格线 | rgba(255,255,255,.06)（`#ffffff0f`） | 全局 48 条竖线、进度线底轨 |
| 压暗渐变 | #111 → #11111130（81%）→ 透明（91%） | 封面顶部；下一个案例为 #111 9% → 透明 76% |
- 变量里还有 `--tech-blue #003BB0`、`--neon-purple #9747FF`、`--mie-red #DA4A4A`，案例页没看到使用。

## 字体与排版
| 角色 | 字体 | 字号 1440 / 390 | 字重 | 行高 / 字距 | 大小写 |
|---|---|---|---|---|---|
| 案例名 + 年份、下一个案例名 | neue-haas-grotesk-text（Adobe Fonts，商用订阅） | 84px（clamp(3em,6vw,6em)）/ 46.8px | 500 | .8 / -3.36px（-.04em） | 大写 |
| 章节名 `.h-h3` | 同上 | 56px（clamp(2em,4vw,4em)）/ 约 28px（推算） | 500 | .8 / -2.24px | 原样 |
| 导语 | 同上 | 42px / 未取到 | 500 | 1.0 / -1.68px | 原样 |
| KPI 数字 | 同上 | 56px | 500 | 1.0 / -2.24px | 原样 |
| 正文 | 同上 | 14px / 12.25px | 500 | 16.8px / -.56px | 原样 |
| 页头小字 | 同上 | 11.9px | 500 | 13.09px / -.476px | 原样 |
| 编号、标签、服务索引 | Bull 5 Typewriter（打字机等宽，授权不明） | 11.9px | 400 | 15.47px / 0 | 大写 |
| 首页大标题 | neue-haas-grotesk-text | 187px | 500 | .8 / -7.49px | 大写 |
- 全站只用一个无衬线（全是 500 字重）+ 一个打字机等宽体；大字一律 -.04em 字距、.8 行高，两行标题几乎贴住。
- 编号用「( a. )」括号小写字母代替 01/02；年份后加「©」当装饰。
- 正文小（14px）、图巨大：文字只占 48 栏里的一窄条，大量留黑。

## 布局
- 栅格：桌面 48 列（gap 0 或 1em），手机 22 列。全局固定一层 48 条竖线（`.grid`：每列 `border-right: 1px #ffffff0f`，`mix-blend-mode: difference`，z 20，不挡点击），压在所有照片上；手机换 `.grid__mobile`。
- 页边距：大图左右约 30px（看图判断）；间距变量 base `clamp(.75rem,1.5vw,1.25rem)`、medium `clamp(3rem,6vw,6rem)`、large `clamp(4rem,8vw,8rem)`、huge `clamp(6rem,10vw,10rem)`。
- 断点：479 / 767 / 991 / 1920。版式：通栏大图 + 两栏方图 + 左标题右正文的字卡；结尾 250vh 吸顶。

## 动效
| 触发 | 效果 | 时长 | 缓动 | 实现 |
|---|---|---|---|---|
| 开场（从别页跳来） | 黑幕 yPercent 0→-100 离场，新内容 y 15vh→0；首页另有 48 条竖线 scaleY 0→1（随机延迟 ≤.45s）、大标题 blur(16px)→0 + 逐词 yPercent 120→0 | .8s；逐词 1s stagger .08 | osmo = cubic-bezier(.625,.05,0,1)；power3.out | React usePageTransition |
| 开场（首访） | 加载页：计数 000→100、进度条 scaleX、logo clip-path 展开、指示灯 #C3FF00 闪（.35s yoyo）；3s 时 70 格百叶 scaleY→0（随机延迟 ≤.5s） | 3s；百叶 .6s | loader (.65,.01,.05,.99)；lamellaOut (.35,0,.1,1) | sessionStorage `mie_intro_seen` 第二次跳过 |
| 滚动 | 封面主图视差 0→30；章内大图 -10→10；长图 -18→0（scrub 2） | scrub | none | `data-parallax` |
| 滚动 | 标题按行从遮罩里升起 yPercent 110→0 + 透明度 0→1 | .8s，stagger .05，一次 | expo.out | SplitText lines mask，「top 90%」 |
| 滚动 | 小标签 `.eyebrow` 逐字随机闪现 | 每字 .05s，总 .4s from random | — | SplitText chars |
| 滚动 | 下一个案例进度线 width 0→100% | scrub 1「top bottom→bottom bottom」 | none | ScrollTrigger |
| 滚动到底 | 下一个案例区底边离视口底 ≤100px、且页面已载入 >1.2s → 自动跳下一页 | — | — | scroll 监听 |
| 循环 | 口号带 6 条 SVG 线描线（offset 6000/600→-700） | 1.5s，间隔 .2s，repeat -1 | power2.inOut | CSS 变量，离屏暂停 |
| 悬停（首页） | 作品行预览图从中心打开 `inset(50%)`→`inset(0%)`，透明度 .22s，最多叠 20 张；图框跟鼠标偏移 ±16/±12px；4 列高亮框滑到当前行 | .8s；跟随 .7s；高亮 .45s | power2.inOut；power2.out；expo.out | GSAP quickTo + Flip.fit，≥992px |
| 转场 | 点链接或滚到底：黑幕 yPercent 100→0 盖上，.3s 时淡入目的地名字（从网址生成，如「Kissuomo」），旧内容 y -15vh；1s 后换路由；新页停 .3s 后黑幕上移离场 | 1s / .4s / .8s | 默认 / osmo | 自写，6s 兜底超时 |

## 交互与组件
- 导航：左上像素点 logo（几个白方块），中间一句工作室定位，「▍Live Website」直达客户网站，EN / IT，白底黑字「Menu」块。
- 下一个案例：整块是链接，可点；滚到底自动跳。
- 首页 showreel 做成取景器界面：时间码「00:02:30」、「4k 60fps」「Res: 1920x1080」、「Play Showreel ▶」、章节号「04. AI CONTENT CREATION」。
- 首页页脚：300vh 的滚动逐帧序列（canvas 2D，46 张 webp，二分顺序预加载，DPR≤2，cover 方式绘制，缺帧用最近一帧；减少动态效果只放第 1 帧），画面是戴 3D 眼镜的黑白影院观众（看图判断）。
- 手机端：封面左侧定位句隐藏，标题 46.8px，22 列栅格；作品悬停预览只在 ≥992px。
- 减少动态效果：`prefers-reduced-motion` 时跳过加载页、转场和逐帧序列，标题动画时长改 .01s。

## 质感
- 颗粒 / 噪点 / 胶片：无。
- 唯一的「质感层」是全局 48 条细竖线（6% 白 + difference 混合），像设计稿网格压在照片上。
- 压暗渐变（封面和下一个案例封面顶部）；菜单、Cookie 面板用 backdrop-filter blur（5–30px）。

## 可借鉴到巡航永恒
- 学：结尾就是下一部的片头。250vh 吸顶 + 下一页同一张封面 + 进度线 scrub 1 + 到底自动跳 + CMS 循环 → 3 台车首尾相接 X48 → 肥仔 → 突破者 → X48；进度线换成放映进度条，保留「下一台」按钮，自动跳给开关。
- 学：封面一屏交代全部：名字 + 年份©（84px，行高 .8）、左一句话、右 (a.)–(k.) 打字机索引 → 车型首屏：车名 + 年份，右侧打字机体列 11 项规格（发动机、排量、扭矩、车重…）。
- 学：章节「图先字后」：通栏大图 → 两张方图 → 大图视差 → 章末字卡「( a. ) 发动机」→ 车型页和故事页（发动机演变、年代史）都按这个节拍，字卡做成黑底白字的电影字幕卡。
- 学：转场黑幕中间写目的地名字（.4s 淡入，1s 后跳）→「下一卷：FAT BOY 肥仔」字卡转场。
- 学：滚动逐帧序列（46 帧 / 300vh / 二分预加载 / DPR≤2 / 减少动态只放首帧）→ 冷启动发动机、爆炸图前半段先用逐帧序列做，比 WebGL 省。
- 学：作品索引表 + 悬停从中心打开预览（`inset(50%)`→0，.8s）→「胶片印样」首页的格子悬停放大。
- 不学：荧光绿 #C3FF00；全局 48 条竖线（巡航永恒用宽银幕黑边和片孔就够）；到底强制自动跳、不给停（键盘和读屏用户会被带走）；没点 Cookie 同意就加载 GTM / Facebook / Clarity（requests.json 里有这些域名）。

## 没读到
- Cookie 横幅（按规矩没点同意）一直挡住右下角约 1/4 画面。
- 无头截图里部分章节图没画出来（y5238、y6988、y10488 一片黑，图片请求都是 200），图片节奏以 HTML 结构和第二轮（CO&IN 页）截图为准。
- `data-scale` 属性的处理代码没找到；首访加载页（70 格百叶）没截到，参数只来自代码。
- 首页作品悬停、转场黑幕的真实手感没实测；/projects 列表页没扫；Bull 5 Typewriter 来源和授权不明。
