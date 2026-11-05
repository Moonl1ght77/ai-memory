# darkroom.engineering 拆解

> 网址：https://darkroom.engineering/ ｜ 类型：工作室作品集（Lenis 作者） ｜ 拆解：2026-11-04 ｜ 证据：桌面 51 帧、手机 20 帧（首页、/work、/work/looped、/backrooms 四次扫描）、CSS/JS 46 个外链文件 + 内联脚本；/about、/contact、/developers 只用 curl 读了 HTML

## 一句话
把整站做成一台黑底红字的「暗房仪器」：3 个颜色令牌、等宽小字、1px 细线，内容全排成面板、读数和日志；炫技只放在几处：作品图从红色抖动网点「显影」成原图、整屏重复字的幕布转场、页尾扫描线字标，以及滚到底继续滚会掉进隐藏的文字冒险页。

## 架构与技术栈
- 框架 / 渲染：Next.js 16.4.3 App Router（`window.next.version`）+ React 19.3.0-experimental（为了用 View Transition 的 `addTransitionType`）；样式是 StyleX 原子类（`$$css`、`x1n2onr6` 类名）；静态预渲染（响应头 `x-nextjs-prerender: 1`）。
- 动效 / 滚动 / 3D / 声音库：Lenis 2.0.0-dev.5（`window.lenisVersion`）；Tempus 1.0.0-dev.18（全站只跑一个 rAF 循环，`useTempus` 带 order 排序）；GSAP 3.15.0（ScrollTrigger、TextPlugin，转场时才懒加载）；Three.js r187（`__THREE__="187"`，只在 /work 加载，WebGPURenderer + WebGL 回退，TSL 节点材质）；Theatre.js 只在开发时调参（`theatrejs-studio-root`）。无声音（Howler 只出现在案例页的技术清单文字里）。
- 托管与内容来源：Vercel（`server: Vercel`、`x-vercel-cache: HIT`），CSP `default-src 'self'`，所有资源同域；作品数据随 JS/RSC 下发，没看到 CMS 接口请求。
- 页面结构（首页从上到下）：巨型标题 + 「[ DARKROOM ], NOUN」词典释义 → 3 个精选案例（每个是 1440×900 通栏静音循环视频，案例名叠在左或右）→ 客户评价 → 自家开源工具（Satus / Lenis / Hamo / Tempus 切换 + 像素风设备插画）→ 宣言 + 服务 / 客户 / 技术 / 奖项四列 → Activity Log（日期 / 类别 / 内容三列更新日志）→ 页脚（LET'S TALK、三地时钟 ARG/CET/WET、四列链接、扫描线字标）。
- 信息架构：作品 /work（18 个，ALL[18] 筛选、GRID / LIST 切换、标签框；只有 2 个有案例页 /work/looped、/work/badomens，其余直接给 LIVE SITE）→ 关于 /about → 联系 /contact（项目类型 + 预算单选表单，Base UI 组件）→ /developers（开源项目 + 公开接口）→ /backrooms（彩蛋）。案例页结构：标题视频 → 分主题的「focus …」图文段 → the brief vs. what we shipped → behind the pixels（制作名单 / 技术 / 工具 / 实现四列）→ 下一个案例。
- 转场：只在 `/ /work /about /contact` 四页之间用「幕布」转场（`pages:["/","/work","/about","/contact"]`），案例页普通跳转；系统开了减少动态效果就直接跳转。

## 视觉与配色
| 角色 | 色值 | 用在哪 |
|---|---|---|
| primary 底色 | #000000 | 全站背景（默认主题 `data-theme="simple"`） |
| secondary 主色 | #E71419（oklch 0.5882 0.2334 27.96） | 所有文字、实心按钮、悬停整行反色的底 |
| contrast 对比色 | #390205（burnt-red） | 页眉标签底框、细线、扫描线 |
| hover | `color-mix(in oklab, secondary 83%, transparent)` | 悬停描边 |
| 着色器染色 | #E30613 | /work 抖动材质的 `uColor` |
| 备用主题 | light #FFF/#000/#E5E5E5、dark #000/#FFF/#262626、red（红底黑字，contrast #C20510）、nasa（白底红字） | 底栏 4 个按钮切换（aria-label「Switch to … theme」） |
- 每个主题只有 primary / secondary / contrast 三个令牌 + 一个派生 hover（证据：JS 里的主题对象、CSS `[data-theme=…]`）。无圆角、无阴影（radius / shadows 统计为空）。

## 字体与排版
| 角色 | 字体 | 字号 1440 / 390 | 字重 | 行高 / 字距 | 大小写 |
|---|---|---|---|---|---|
| 首页 H1 | AS Therma Bold Condensed（family `therma`） | 200px / 104px | 只有 Bold Condensed 一个字面 | 0.8（160px）/ -0.05em（-10px） | 大写 |
| H2、作品名 | 同上 | 120px / 74.88px | 700 | 0.8 / -0.05em | 大写 |
| 案例页小标题 | 同上 | 48px / — | 700 | 0.8 / -0.03em | 原样 |
| 正文、导航、按钮 | Replica Mono LL（Lineto，商用付费） | 14px / 12.48px | 200 | 1.2（16.8px）/ -0.01em | 大写 |
| 注释、表头、释义 | Replica Mono LL | 10px / 8.32px | 200 | 1.2 / -0.01em | 大写 |
| 页脚字标、/work 横幅 | AS Module 2 VF（可变字体，自定义轴 `DENS`） | 251px（17.43vw）/ —；横幅 86px | 400 | 1.0 / 0 | 小写 |
- 授权：Replica 是 Lineto 商用字体；AS Therma / AS Module 2 来源不明。字体按 `core`（基本拉丁）+ `ext` 两段子集切分，首页 4 个 woff2 共 37KB。
- 排版手法：只有「窄体展示字 vs 14px 等宽小字」两极，字号档只有 200/120/48/14/10，没有中间档；H1 四个词在 1408px 内两端撑满（看图判断）。
- 全大写等宽 + 日期写 `2025/09/05`、编号 `01.` 放左列 → 读数感；`text-box-trim: both; text-box-edge: cap alphabetic` 把大字行框裁到大写字高，标题底边能贴住 1px 细线。
- 所有尺寸写成 `calc(px * 100 / var(--device-width) * 1vw)`：桌面按 1440 稿、手机按 375 稿等比缩放（800px 分界）。另有 `text-wrap: pretty`、`font-feature-settings: "ss04"`、`"case"`。

## 布局
- 栅格：桌面 8 栏、手机 4 栏（`--columns`）；栏距和页边距桌面 16px@1440、手机 8px@375（`--gap` / `--safe`），`--layout-width: calc(100vw - 2*safe)`，无 max-width，全部通栏。
- 固定分工（看图判断）：左半 1–4 栏放编号、小标签、释义；正文统一从第 5 栏起，按钮贴最右一栏；行与行之间是 1px 通栏细线。
- 页眉高 22px（缩放），四个位置是标签框（左右角是会跳动的字形条，中间是站名和 WORK / ABOUT / CONTACT，当前页实心红底黑字），一条 1px 细线横穿页眉中线；底栏同结构（4 个主题开关 + 中间赞助条）。
- 断点只有 800px 一个（`min-width:800px` / `max-width:799.98px`），另用 `(hover:hover) and (pointer:fine)` 区分鼠标设备。
- /work：作品名和媒体左右交替（偶数行反向），配标签框和一条红色 LIVE SITE 长条按钮。

## 动效
| 触发 | 效果 | 时长 | 缓动 | 实现 |
|---|---|---|---|---|
| 开场 | 无开场动画、无加载页，0.5s 帧已完整（4 个开场帧相同） | — | — | 预渲染 HTML |
| 滚动 | Lenis 平滑：滚轮 lerp 0.125（库默认 0.1）、multiplier 1；触屏设备不挂 Lenis，用原生滚动 | 每帧插值 | lerp | 证据见「交互与组件」 |
| 滚动 | /work 每项进视口：WebGL 平面 scale 0.9→1 并向交替方向平移，DOM 透明度 0→1；一条横条 `clip-path: inset(0 X% 0 0)` 从左往右擦出 | 跟滚动走（start `top 视口高-22px` → end `bottom 视口高-22px`） | 线性 progress | 自写滚动进度 hook |
| 悬停 | 作品图从红色有序抖动网点「显影」成原图；一排按钮在行内跟着鼠标 Y 移动 | 1s（离开时同样 1s 变回网点） | expo.out | GSAP 补间材质 `blendOpacity` 1→0（`appear()` / `disappear()`） |
| 悬停 | 日志行整行反色（红底黑字），日期右移 18px 露出 ■；按钮内箭头出现并右移 18px | 0.15s | cubic-bezier(.23,1,.32,1) | CSS 变量 `--activity-date-x: 1.25vw`、`--button-inner-x`，只在 `(hover:hover) and (pointer:fine)` 下 |
| 按下 | 按钮缩到 .97 / .9 | 0.12s | 同上 | `:active`；.9 那档只在 `prefers-reduced-motion: no-preference` 下 |
| 循环 | 页眉角落字形条逐帧切换；favicon 13 帧动画 | 字形 100ms/帧；favicon 500ms/帧 | 步进 | setInterval 切 SVG 子元素 opacity；离开视口、标签页隐藏或减少动态时停 |
| 转场·离开 | 幕布 `clip-path: inset(100% 0 0 0)→inset(0)` 自下而上盖住；幕布内是一行行重复的目标页名，奇偶行反向横移（每行起始偏移取 [34,14,8,22,10,26,…]%）；旧页 y→-50vh 并淡出；页眉 0.6s 上收 | 1.1s | power3.inOut | GSAP timeline + React `startTransition` / `addTransitionType("curtain")`（View Transition 根动画设为 none），期间 `lenis.lock()` |
| 转场·进入 | 幕布内容整体上移，文字行 y→-200%（0.9s）；新页从 y 100vh 升起；幕布底色 0.6s 淡出；页眉 0.7s 落回 | 1.2s | power3.inOut / expo.out / expo.inOut | 同上；减少动态时整套跳过 |
| 彩蛋 | 停在页底继续向下滚（500ms 内累计 deltaY > 300）→ 800ms → 全屏红卡自下而上擦入（1.2s）→ 2s 后跳 /backrooms（文字冒险游戏） | 1.2s / 2s | — | wheel 监听 + setTimeout；红卡 `clip-path` 关键帧 |
- 全站缓动令牌：整套 Penner 曲线写成 CSS 变量（`--ease-out-quint: cubic-bezier(.23,1,.32,1)` 等）+ `--ease-gleasing: cubic-bezier(.4,0,0,1)`；CSS 过渡全部 0.12–0.35s，长动画全交给 GSAP。

## 交互与组件
- Lenis 实参（证据：chunk `0r0w1d7khx_gp.js` 里 Lenis 构造函数默认值、`02y-1ojauohc8.js` 里 `ReactLenis` 包装与 `LenisGate`；幕布转场在 `3x79abk8gxv4f.js`，抖动材质在 /work 的 `38hp6fhifpmnz.js`）：`wheel {smooth:true, lerp:0.125, multiplier:1}`；`touch {smooth:false, lerp:0.1, multiplier:1, inertia:2}`（iOS `inertia 1.7, lerp 0.05`）；`drag {enabled:false}`；`programmatic {lerp:0.1}`；`autoRaf:true`、`anchors:true`、`overscroll:true`、`stopInertiaOnNavigate:true`、`respectReducedMotion:true`（减少动态时滚轮 lerp=1、程序滚动直接跳到位）；`syncScrollTrigger:true`；`matchMedia("(hover: none) and (pointer: coarse)")` 为真时不挂 Lenis。无 duration（纯 lerp）。
- 自制滚动条：右边缘一根红色细条（`aside aria-label="Page scroll"`），可拖拽（`scrollTo(y, {lerp:0.2})`），方向键 ±100px；转场锁定时隐藏。
- 导航：页眉 3 个文字标签框 + 当前页高亮；手机变成左格子图标 + 中间站名 + 右汉堡按钮（46×29）。光标用系统默认，无自定义光标、无加载页、无声音。
- 主题开关：底栏 4 个方块按钮切 light / dark / red / simple。
- 作品列表：筛选（ALL[18] 上标计数）+ GRID / LIST 切换；悬停时「case study / live site」按钮跟随鼠标。
- 视频：`preload="none"`、muted、loop、playsInline、next/image 海报（w=828），手机换 `-mobile.mp4`（Oreo 720KB → 148KB）。
- 手机端：原生滚动，H1 折成两行，正文 12.48px，案例视频换小文件。

## 质感
- 颗粒 / 噪点：无（没有 feTurbulence、噪声函数、颗粒叠层）。
- 扫描线：页脚字标背后一块 `repeating-linear-gradient(0deg, contrast 0 2px, transparent 2px)`，`background-size: 10px 8px`（手机 10px 5px），即每 8px 一条 2px 线；字标本身用带 `DENS` 轴的可变字体（页眉叠层设 `font-variation-settings: "DENS" 50`）。
- 抖动网点（/work 招牌）：Three.js TSL 材质，先按 `uPixelSize` 像素化 → 取亮度（.299/.587/.114）→ 加有序抖动阈值（抖动矩阵贴图，可混入 `fract(sin(dot(p, vec2(12.9898,78.233)))*43758.5453)` 随机）→ 量化 → 调色板（黑→红两色）。线上参数：`matrix "14"`、`mode 2`、`quantization 4`、`pixelSize 3`、`brightness 0`、`opacity 0.25`。
- 混合模式：CSS 里只有 `mix-blend-mode: difference` / `overlay` 各 1 处、`backdrop-filter: blur(2px|4px)`，截图里看不出用在哪。

## 可借鉴到巡航永恒
- 学：Lenis 照这套配：滚轮 lerp 0.125、multiplier 1、只在 `(pointer: fine)` 挂载，触屏用原生；减少动态时 lerp=1；转场期间 lock；自制细滚动条支持拖拽 + 方向键。→ 用在全站滚动层（稳定版 Lenis 1.x 对应 `lerp: 0.125, wheelMultiplier: 1, syncTouch: false`）。
- 学：「幕布」转场改成电影字卡：1.1s power3.inOut 的 `clip-path` 自下而上擦入，幕布里是目标章节名（如「X48」）一行行重复、奇偶行反向横移，旧页上移 50vh 淡出，减少动态时直接切。→ 用在首页「一部短片」的章节切换和各车型页之间。
- 学：「显影」悬停：照片默认是粗网点 / 抖动的黑白版（pixelSize 3、4 级灰、opacity 0.25），悬停或进视口 1s expo.out 显影成原片；暗房显影和巡航永恒的胶片主题同源。→ 用在「胶片印样」网格和车型列表；抖动换成黑白两色调色板，不用红。
- 学：仪器面板语言：1px 细线横穿、等宽 14px 大写标签框、编号放左半、正文从第 5 栏起、`YYYY/MM/DD` 日志表、`text-box-trim` 让大字贴线。→ 用在发动机爆炸图引线标注、年代史时间轴、车型规格表。
- 学：三令牌主题（primary / secondary / contrast + 83% hover）+ 尺寸按设计稿宽度换算 vw（1440 / 375 两套，800px 分界）。→ 巡航永恒的黑白令牌可设「正片 / 负片」两套主题。
- 学：性能做法：字体切 core / ext 子集（4 个文件共 37KB）、视频 `preload="none"` + 海报 + 手机小文件、Three.js 只在需要的页面加载、GSAP 等到转场时才懒加载。
- 不学：红黑配色和像素游戏风（巡航永恒是黑白电影）；手机 8.32px / 12.48px 小字和整段大写长文（可读性差，中文也没有大小写）；React experimental、Lenis 2 dev、Tempus dev 这类开发版依赖；照搬它们的字体（Replica 商用，AS 系列授权不明）；Ibicash 首页视频 6.2MB 这种未压缩大文件。

## 没读到
- 幕布转场、悬停显影、Lenis 手感都是读源码得出的，没有录屏亲眼看到（扫描脚本不点击、不跳页，只截滚动帧）。
- 页脚字标在页底被拉成横条（首页滚到底那帧），成因没确认：可能是彩蛋触发态，也可能是 Lenis 2 的 overscroll，看图判断。
- 抖动矩阵贴图 `"14"` 具体是哪种矩阵（Bayer / 蓝噪声）没读到；/work 的 canvas 用 WebGPU 还是 WebGL2 后端没确认（扫描时 getContext 两种都返回空）。
- GRID / LIST 切换的动画（代码里出现 GSAP Flip 字样，没找到调用）；手机菜单展开样式没截到；/about、/contact、/developers 只读了标题和表单结构，没截图。
- AS Therma、AS Module 2 的字体厂商和授权方式未知。
