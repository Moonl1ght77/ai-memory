# Bugatti Tourbillon 车型页 拆解

> 网址：https://www.bugatti.com/models/tourbillon ｜ 类型：产品页（超跑车型详情） ｜ 拆解：2026-11-04 ｜ 证据：桌面 24 帧、手机 5 帧、CSS/JS 44 个文件（另有内联 3 个）

## 一句话
把一台车当一块机械表来讲：全程黑底，滚动就是拧发条——车身随滚动转一圈、外壳被擦掉露出底盘、“W16”变成“V16”、车身剥开露出动力总成，每一步都由滚动精确控制（视频按滚动逐帧走）；字只有三种，等宽小字像仪表读数一样贴在画面旁边。

## 架构与技术栈
- 框架 / 渲染：SvelteKit（summary.framework、/_app/immutable 分包）+ Tailwind（shadcn 风格变量 --background、--radius .5rem）。没有 canvas、没有 WebGL，全部是 video + img。
- 动效 / 滚动：GSAP 3.14.2 核心（index.CHiOIsZF.js）+ ScrollTrigger（包头标 3.11.0）+ ScrollSmoother（`smooth:1.5, effects:true, smoothTouch:.1, normalizeScroll`）；触屏设备（`"ontouchstart" in window || maxTouchPoints>0`）不启用平滑滚动、也不 pin，改用 CSS sticky。Embla 做画廊轮播；dotLottie-web 0.44.0（jsdelivr，wasm 1.9MB）只用来放「向下滚」小动画 scrolldown2.lottie。没有声音。
- 托管与内容来源：Vercel（server: Vercel、x-vercel-cache: HIT）。图片视频全走 imgix：图片 `auto=format,compress&cs=srgb&sharp=10&w=…` 出 AVIF，先放 `w=100&blur=20` 的 3.7KB 模糊图；循环视频转 AV1 2Mbps 1920 宽，先放 `video-thumbnail=0&fm=jpg&w=500&blur=10` 模糊封面；滚动驱动的两段视频用原始 MP4、不转码（推测为了关键帧密度，未证实）。页面由 Pagebuilder 区块拼出，CMS 不明；Usercentrics 管同意弹窗，Cloudflare Turnstile 管表单。
- 页面结构（sections 顶部 px）：0 首屏 140vh → 1261 滚动视频 600vh（车转一圈 + 4 张说明卡）→ 6661 Eight-Eye Layout、8175 One-Piece Taillight（钉住的图文叠层）→ 10290 Beauty Underneath（外壳擦掉露出底盘）→ 11642 W16→V16 大字 200vh → 13442 3D 打印视频 → 14542 Performance 数据行 → 15335 新发动机视频 → 16435 第二段滚动视频（车身剥开）→ 18552 跑马灯 → 18896 Details 视频网格 → 21870 Gallery → 22840 Specifications 参数表 → 24303 Discover more → 25761 页脚。全长 26732px，单页，无页面转场。

## 视觉与配色
| 角色 | 色值 | 用在哪 |
|---|---|---|
| 底色 | #000000 | body（bg-black），整页 |
| 主文字 | #FFFFFF | 标题、正文、说明卡（textColors ×726） |
| 次文字 | #999999 | 表头、em（em 不斜体、改灰色）、副行 |
| 跑马灯字 | #E1E1E1 | 15vw 大写跑马灯 |
| 卡片底 | #3F3F3F 透明度 .2 / .3 | 说明卡 `from-transparent to-grey-dark/30` 渐变 |
| 描边 | #FFFFFF 透明度 .2 | 说明卡、数据行、按钮 1px 边 |
| 行动色 | #004BFA | 全页唯一彩色按钮「Specification sheet」 |
| 顶栏遮罩 | #000 透明度 .5→0 | 顶栏 `bg-gradient-to-b from-black/50` |

## 字体与排版
| 角色 | 字体 | 字号 1440 / 390 | 字重 | 行高 / 字距 | 大小写 |
|---|---|---|---|---|---|
| 车名字标 | SVG 图片（tourbillon-wordmark-flat.svg） | 约 900px 宽 / 约 250px（看图） | — | — | 手写体 |
| 超大字「W16」 | Bugatti Display | 432px（30vw）/ 约 117px（推算） | 400 | 1em / 0 | 字体只有大写字形（看图） |
| 跑马灯 | Bugatti Display | 216px（15vw）/ 约 58px（推算） | 400 | 1em / 0 | 大写 |
| 数据「2.0 s」 | Bugatti Text Regular | 108px（7.5vw）/ 约 29px（推算） | 400 | 1em / 0 | — |
| 段标题 h3 | Bugatti Display | 60px（≥1280）/ 36px | 400 | 1 / 0 | 大写（字形） |
| 说明卡标题 | Bugatti Display | 36px / 24px | 400 | 40px / 0 | 大写（字形） |
| 引言段 | Bugatti Text Regular | 36px / — | 400 | 40px / 0 | 句首大写 |
| 正文 | Bugatti Text Regular | 20px、16px / 16px | 400 | 28px、24px / 0 | 句首大写 |
| 说明卡正文、表格 | Bugatti Monospace | 14px / 12px | 400（表头 700 #999） | 20px、16px / 0 | 句首大写 |
| UI 标签（MENU、STORE、POUR L'ÉTERNITÉ、按钮） | Bugatti Monospace | 14px / 12px | 400 | 20px / 1.4px（.1em），12px 时 1.2px | 全大写 |
| 等宽大标题 | Bugatti Monospace | 60px | 400 | 60px / 1.5px | 全大写 |

授权：三套都是 Bugatti 专有品牌字体（BUGATTIDisplay / Text / Monospace，各约 20KB woff2），不可用。
- 字号阶梯（Asset.js 源码）：sm 14 → md 16 → lg 16/18/20 → 2lg 16/24 → xl 24/36 → 2xl 36/60（按断点升），再往上直接跳 vw：7.5vw → 10vw → 15vw → 30vw，行高都是 1em。固定 px 管阅读，vw 管“海报字”。
- 排版手法：三套字分工——Display 窄体负责“名字”，Text 负责“叙述”，Monospace 负责“标签、读数、规格”（等宽出现 ×71 处，是整页的仪表盘声音）。等宽 UI 标签全大写 + .1em 字距，等宽正文不加字距。数据行把 12–14px 等宽标签「0–100 km/h」放左上角、108px 数字居中、单位“s”同字号空一格，像表盘刻度。参数表两组并排，左名右值，全等宽，1px 白 20% 分隔线。

## 布局
- 内容最大宽 1536px（`section{max-width:1536px;padding:6rem 0}`，上下 96px），页边距 12 / 24 / 48px（px-3 sm:px-6 md:px-12）。
- 网格：6 栏 gap 48px、3 栏 / 4 栏 gap 36px（Details 网格）、单栏 gap 12 / 36 / 48px；断点 Tailwind 640 / 768 / 1024 / 1280 / 1536 + 1720。
- 版式在三种之间轮换：通栏钉住的全屏影像（1440×900）↔ 带 48px 边距的 16:9 媒体（1344×756）↔ 不对称图文（右侧大图 + 左侧说明卡；Details 用 888×500、660×371、432×243 三种 16:9 拼）。
- 圆角：卡片和媒体 6px，圆形箭头和胶囊按钮 9999px。全屏图有一种“同图模糊放大做背景、原图 6px 圆角居中”的变体（fullscreenSettings bgBlur，帧 desktop-scroll-10）。

## 动效
| 触发 | 效果 | 时长 | 缓动 | 实现 |
|---|---|---|---|---|
| 开场 | 首屏 140vh（手机 110vh）；背景视频放大到 110%，`data-speed .95` 慢半拍视差；字标和副标静止居中；底部「向下滚」Lottie 循环 | 视频循环 | — | ScrollSmoother effects + Tailwind scale-110 |
| 滚动（招牌） | 视频按滚动逐帧播放：容器高 = 视频秒数 × 30vh（第一段 600vh = 5400px），里面 sticky h-screen 钉住；`currentTime` 落进每张说明卡的 [textIn, textOut] 区间才显示 | 由滚动决定 | ease "none"、scrub:true，外加 ScrollSmoother smooth 1.5 | ScrollTrigger timeline `.to(video,{currentTime})` |
| 滚动 | 外壳→底盘：上层图 clip-path polygon 从满幅收向左边线，下层图 scale .5→.8（可选 rotation -3°→3°），进度 28% 时换标题 | 整段 scrub | none | ScrollTrigger |
| 滚动 | 图文叠层：每段标题从视口底（100%）走到中线（50%）期间，下一张图 clip-path inset(100% 0 0 0)→inset(0) 自下而上擦入（推测用于 Eight-Eye / Taillight 段） | scrub | none | ScrollTrigger，≥768px 才启用 |
| 滚动 | W16→V16：W 上移 50% 缩到 .95 淡出，V 从下方 50% 升起（重叠 0.2），整行左移 5%，段落从右侧 2em 淡入，末尾停 0.2 | 200vh | GSAP 默认 | ScrollTrigger timeline |
| 进入视口 | 各区块淡入 | .7s | cubic-bezier(0,0,.2,1) | CSS transition-all duration-700 |
| 循环 | 跑马灯：两份拷贝左移，两端 mask-image 渐隐，可悬停暂停 | 40s 一圈 | linear | CSS @keyframes marquee-left |
| 悬停 | 颜色 / 背景 / 边框变化 | .15s / .3s | cubic-bezier(.4,0,.2,1) | Tailwind |
| 画廊 | 切换说明文字 left 0→24% 带淡入 | .3s（延迟 .3s） | ease-in-out | CSS appear / disappear + Embla |

## 交互与组件
- 导航：顶栏 absolute（不跟随滚动），左「≡ MENU」、中 BUGATTI 字标（SVG）、右「STORE」+ 购物袋；滚下去后没有常驻导航（看图判断）。
- 说明卡（招牌组件）：6px 圆角、1px 白 20% 描边、`from-transparent to-grey-dark/30` 渐变底、内边距 12 / 24px；Display 36px 标题 + 等宽 14/20 正文。桌面放左下或左上，手机放画面顶部、车在下面。
- 数据行：整行 1px 描边大卡，左上等宽小标签，中间 108px 数字。参数表：Performance / Acceleration 两组并排。
- 按钮：唯一实心按钮是 #004BFA 胶囊；其余是 1px 白描边 + 等宽大写 12px（字距 1.2px）。画廊：Embla + 40px 圆形左右箭头。
- 光标默认；没有加载页（先模糊图后高清图顶替）；没有声音。右下常驻招聘浮窗（与设计无关）。
- 手机：不启用 ScrollSmoother、不 pin（CSS sticky 顶替）；`innerWidth < innerHeight` 时滚动视频换竖版源 videoPortrait；等宽正文降到 12/16px，标题降一档。

## 质感
- 唯一的程序化颗粒：顶栏 BUGATTI 字标 SVG 内置 `<filter id="noiseFilter">`，feTurbulence fractalNoise baseFrequency 1、numOctaves 4，再 feComposite operator="in" 只留在字形内；按组件属性开关，本页帧里看不出颗粒（推测没开）。
- 其他质感全靠素材：影棚黑场、低调布光、车漆高光；菜单 / 弹层用 backdrop-blur 24px 或 40px + saturate 150–200%。没有全页颗粒层、没有混合模式。

## 可借鉴到巡航永恒
- 学：视频按滚动逐帧播放（容器高 = 秒数 × 30vh，sticky 钉住，说明卡按 currentTime 区间显隐）→ 发动机冷启动 + 爆炸图：Blender 渲一段“合体→拆开”的视频或图片序列挂在滚动上，引线标注按时间点出现；手机换竖版源。编码要用很短的 GOP（或全关键帧），否则倒滚卡顿。
- 学：W16→V16 字母顶替（旧名上移淡出、新名从下升起、整行左移 5%、说明从右 2em 进，200vh）→ 横向发动机时间轴：Knucklehead → Panhead → Shovelhead → Evolution → Twin Cam → Milwaukee-Eight，一代名字顶掉上一代。
- 学：三套字分工，等宽小字当仪表读数（UI 标签全大写 + .1em 字距；规格、年代、说明卡全用等宽；em 改灰不斜体；固定 px 管阅读、vw 管海报字）→ 车型参数表、年代标注、片尾字幕。开源等宽可选 JetBrains Mono / IBM Plex Mono。
- 学：外壳擦除露出内部（clip-path 向一侧收 + 下层图 .5→.8 放大，28% 换标题）→ 车型详情页“漆面→骨架”：X48 外观照擦掉，露出车架与发动机线稿；也可当改装风格对比的滚动版。
- 学：首屏不做飞入动画：140vh 高、视频放大 110% 慢视差 .95、字标居中静止、下面一行等宽大写副标、顶栏垫 50% 黑渐变 → 巡航永恒片头字卡的庄重感靠“慢”和“暗”，不靠特效。
- 学：媒体先给模糊占位（图 w=100 blur=20 约 3.7KB，视频封面 w=500 blur=10），循环视频统一 AV1 2Mbps 1920 宽 → 黑白长片控制体积的同款做法。
- 不学：首访视频 53MB、脚本 2.8MB；CSS/JS 里没有 prefers-reduced-motion 处理（0 命中），巡航永恒要给“减少动态效果”用户直接放静帧 + 说明卡。专有字体、#004BFA 蓝、招聘浮窗不搬。

## 没读到
- 首屏视频没播：无头 Edge 下 4 张开场帧完全相同，显示的是 blur=10 的模糊封面，所以开场真实节奏（尾灯点亮？镜头推进？）没看到；HeroBlock 里没找到 GSAP 调用，字标判断为静止，未确认。
- AV1 循环视频在帧里多是封面，实际剪辑节奏没看到；只有两段滚动驱动视频（原始 MP4）解码出了画面。
- Performance 数字有没有计数动画：源码没搜到计数逻辑，帧里是静止值，判断为静止。
- ScrollSmoother smooth 1.5 的手感、pin 衔接、倒滚时视频是否卡顿，需要真机。
- 菜单展开态、Store、画廊大图模式（fixed bg-black/90 backdrop-blur-2xl）没打开；Usercentrics 弹窗只是被脚本隐藏，没有点同意；没遇到拦截。
