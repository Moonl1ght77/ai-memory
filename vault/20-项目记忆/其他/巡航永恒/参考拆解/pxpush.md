# PX PUSH 拆解

> 网址：https://pxpush.com/ ｜ 类型：工作室官网（按月订阅的外包设计部门，保加利亚索菲亚） ｜ 拆解：2026-11-04 ｜ 证据：桌面 29 帧、手机 7 帧（两次扫描：默认等 9s + 等 22s）、CSS/JS 61 个文件（含内联）

## 一句话
把整站做成一台开着的老 CRT 电视：所有内容都隔着扫描线、暗角、跳动颗粒和一层柔光去看，首屏是三维云海里转动的镀铬「px」硬币。屏幕本身就是品牌，页面只是在这块屏上播放的节目。

## 架构与技术栈
- 框架 / 渲染：Nuxt 3.19.2 + Vue 3.5.21 SSR（证据：响应头 `x-powered-by: Nuxt`；js-07 里 `versions.nuxt "3.19.2"`、`kv="3.5.21"`）。
- 动效 / 滚动 / 3D：GSAP 3.13.0 + ScrollTrigger（js-07 `version:"3.13.0"`）；Lenis 1.3.11（`window.lenisVersion`，`new Lenis({autoRaf:false})` 用默认 lerp .1，挂在 `gsap.ticker`，`lagSmoothing(0)`，触屏设备不开）；three.js r180（`REVISION "180"`，GLTFLoader 读 `floppy_disk/scene.gltf`，OBJLoader 读 `cursor.obj`）；Splitting 式拆字（`data-splitting`、`.word/.char/.line__inner`）。无声音。
- 托管与内容来源：Cloudflare（`server: cloudflare`、`cf-ray`）；内容源（CMS）不明。路由 `/ /about /journal /start /terms /privacy`。
- 页面结构（首页 1440 宽 11,435px）：首屏 900（三维云海 + 跑马灯大标题 + 迷你视频 + 终端风说明）→ About 灰段 1645（长段落逐词浮现）→ 作品 1520（横向卡片条，「HOLD TO SKIM」）→ 蓝色好处 2729（Nº001–005 叠卡）→ 黑色价格 2468（三维软盘 + 套餐表）→ 蓝色 Brand Sprint 1346 → 蓝色页脚 827（字标残影）。
- 转场：Nuxt 页面钩子 + GSAP。离开时 `.wrapper` 透明度 .3s 归零、标题行 yPercent 0→-100（.5s expo.in，stagger .05）；新页逐行进场。

## 视觉与配色
| 角色 | 色值 | 用在哪 |
|---|---|---|
| 页面底 | #1A1A1A（`--bgcolor`） | 首屏后的作品段、默认底 |
| 文字 / 细线 | #BABABA（`--grey`）；线 1px `hsla(0,0%,100%,.7)` | 全站文字、分隔线、按钮描边 |
| 主色块 | #03049C（`--blue`） | 好处段、Brand Sprint、页脚（面积第一） |
| 深底 | #111111（`--black`） | 价格段 |
| 灰段 | #71737D（`--lightgrey`） | About 段 |
| 录制灯 | #00FF00 + 光晕 `rgba(60,255,123,.25)` | 页头「Get started ●」 |
| 首屏天空 | #003C6A→#0EAEBC 渐变；雾色 #0EAEBC；云色调 #DCE7F5；镀铬 #E7EDF6（硬币）/ #E5ECF7（光标） | 三维云海、镀铬标 |
- 变量里还有 `--red #FF001A`、`--cream #BAB5A6`、`--pink #A7849D`、`--lightblue #9FE4F3`，首页没看到大面积使用。

## 字体与排版
| 角色 | 字体 | 字号 1440 / 390 | 字重 | 行高 / 字距 | 大小写 |
|---|---|---|---|---|---|
| 跑马灯大标题 h1 | SemiSqueezed（窄体无衬线，文件名 SemiSqueezedMedium，真名和授权不明） | 172.8px（12vw）/ 78px | 400 | .96 / -1.44px | 原样 |
| 段落大字 p.lead | SemiSqueezed | 40.3px（2.8vw）/ 30px | 400 | 1.0 / 0 | 原样 |
| 好处段标题 | SemiSqueezed | 63.6px / 约 1.5em | 400 | .8 / 0 | 原样 |
| 导航 | SemiSqueezed | 24.5px（1.7vw）/ 手机菜单 42px | 400 | — | 原样 |
| 正文、按钮 | Graphik（Commercial Type，商用付费） | 24.5px（body 1.7vw，≤1300 锁 20px）/ 20px | 400 | — | 原样 |
| 标签、说明 | PP Neue Montreal Mono Book（family 名 `Mono`，Pangram Pangram，商用付费） | 12.2px（50%）、说明 17.1px（70%）/ 10px | 400 | normal / -0.72px | 标签大写 |
- 字号全写 vw（12vw / 2.8vw / 1.7vw），整页跟屏宽等比；≤1300px 正文锁 20px，≤600px 改 px（CSS 里 170 处 `max-width:600px`）。
- 两极对比：172px 窄体 vs 12px 等宽大写标签；标签前放「⬤」，编号写「Nº001」。
- 等宽体模拟终端：「v.02 ------ WELCOME TO PX PUSH ------ //」用连字符画线；正文里关键词「SPEED.」做成可悬停的高亮块（灰底黑字，悬停变蓝底）。
- 段落首行缩进 20vw，缩进处放半透明「→」（`p.lead.lead__indent:before`）。

## 布局
- 通栏 100vw；`.layout` 内边距 1.5vw 3vw（43px 页边），左标签列 + 右内容列 66%（看图判断：内容列从 x≈504 开始）；≤600px 变单栏、内边距 10px 20px。
- 栅格变量 `--columns: 20`（给像素光标和换段色条用）；圆角 `--radius: .5vw`（7.2px），图片 12px。
- 断点：600 / 1000 / 1300。首屏 `--bannerHeight: 170vh`（手机 100lvh）。
- 版式：竖向分段为主；作品段是横向卡片条；好处段是叠卡（每张卡跟滚动下移，叠出 10vh 的标题条，看图判断见 scroll-08 帧）。

## 动效
| 触发 | 效果 | 时长 | 缓动 | 实现 |
|---|---|---|---|---|
| 开场 | 跑马灯上下两条细线 scaleX 0→1 | .8s | expo.out | GSAP 改 `--marquee-line-scale` |
| 开场 | 首屏各行 `.line__inner` yPercent 100→0，0.4s 后开始 | 1s，stagger .05 | expo.out | GSAP（`Tp=.4`） |
| 开场 | 迷你视频 clip-path `rect(100% … round radius)` 从底边升起；视频 scale 1.2→1 | 1s / 2s（0.8s 起） | expo.out | GSAP |
| 开场 | 三维云海 canvas 透明度 0→1 | 1.2s | none | GSAP；看图判断：无头软渲染下 9s 仍黑屏，22s 才出全 |
| 滚动 | 长段落逐词浮现 opacity 0→1 | scrub「top 80%→bottom 80%」，stagger .05 | none | `[effect__textFade]` |
| 滚动 | 标题逐词随机顺序从 z:-100 推到 0（父级 perspective 1000） | stagger .03 from random | power4 | `[effect__titleRandom]` |
| 滚动 | 分隔线 clip-path `inset(0 100vw 0 0)`→0 | scrub「top 90%→top 70%」 | none | `[effect__separatorIn]` |
| 滚动 | 换段：10 条横向色条（手机 15 条）从底边 scaleY 0→1.01 | scrub「top 0%→top -80%」，each .04 from end | power4 | `[effect__overlayIn]` |
| 滚动 | 区块从中间一条竖缝打开 clip-path `inset(0 35vw)`→0（手机 `inset(20vh 15vw 0 15vw)`）；里面的 `.works__slideshow` scale 1.2→1、rotation 3→0、opacity .4→1 | scrub「top 70%→top 0」；里层「top 100%→top 0」 | none | `[effect__clipIn]`（首页 SSR 里没找到用在哪，可能在作品页） |
| 滚动 | 页脚字标残影：3 个克隆，`--trail-progress` 0→1、`--trail-crop` 90%→65% | scrub「top bottom→bottom center」 | none | CSS 变量 + clip-path + `translateY(progress×index×-22%)` |
| 悬停 | 导航逐字乱码再复原（字符池 a–z 和 `!@#$%^&*-_+=;:<>,`） | 每字 .01s，间隔 .02s | — | 自写 TextAnimator |
| 悬停 | 按钮底色 `scaleY(var(--anim))` 从进入方向长出；文字左右 padding 0→1vw | .3s | ease | CSS + JS 写 `--anim`、`--button-origin` |
| 悬停 | 「SPEED.」一词：云海相机速度目标 .2→2（每帧插值 .06）；全屏 `.canvas__cover`（difference 混合）淡入 | 入 1.5s / 出 .6s | none | three.js + GSAP |
| 光标 | 20 列方格（每格 72px）在鼠标下点亮后熄灭，difference 混合 + SVG gooey 滤镜（stdDeviation 3.2） | ttl .2s | — | 自写 Cursor 类，≤1300px 和 Safari 关 |
| 转场 | 离开：`.wrapper` 透明度→0；标题行 yPercent 0→-100 | .3s / .5s stagger .05 | expo.in | Nuxt 页面钩子 |

## 交互与组件
- 导航：固定页头（高 81px），左字标，右「Index / About / Journal ｜ Get started ●」，竖线分隔；绿点是 Arial 字符加三层 text-shadow 光晕，像录像机 REC 灯。≤600px 换「Menu」按钮，菜单是蓝色面板 `clip-path: rect()` 展开 + 黑 80% 遮罩。
- 3D（两块 1440×900 的 webgl2 canvas）：① 镀铬硬币 = Logo3D 组件，SVGLoader 读 `/img/pxpush.svg` 挤出 depth 30 + 倒角，MeshPhysicalMaterial（#E7EDF6、metalness .92、roughness .05、clearcoat 1、envMapIntensity 1.7），初始 rotation.y -.45，带阻尼自转（速度 ×.9 衰减、插值 .16）；看图判断：滚动后缩到页头正中常驻。② 云海：8000 张 64×64 云片合并成一个网格，相机在 8000 单位里循环前进，相机跟鼠标（x×.1、y×.05，每帧插值 .01），相机前挂一个镀铬 3D 光标模型 `cursor.obj`（metalness 1、roughness .1、clearcoat 1）；离开视口 IntersectionObserver 停渲染。价格段是三维软盘（GLTF）。
- 作品段光标换成「HOLD TO SKIM」药丸，按住拖动浏览（按下 scale .95，.35s power2.out）。
- 加载：等字体和首屏媒体，最多 2s（`Promise.race`）就开场，`body.loaded`。
- 手机端：Lenis、像素光标、颗粒都关；扫描线周期改用 vh；菜单改全屏蓝面板。

## 质感
- 颗粒：`.wrapper:after` 平铺 `noise.png`，比视口大一圈（四边各 10rem），`animation: foreground .5s steps(1) infinite` 在 10 个关键帧间跳（translate3d ±1–9rem，相当于 20fps 换一次位置），`mix-blend-mode: overlay`，opacity .3，fixed，z 100，`pointer-events:none`；≤600px `display:none`。
- 扫描线：`.scanlines` 容器 opacity .25、z 22；`:before` 一条 2–3px 黑条（#000000b3，opacity .8），`scanline 8s linear infinite` 从 110vh 走到 -200vh；`:after` 横纹 `linear-gradient(transparent 50%, rgba(0,0,0,.3) 51%)`，周期 .4vw，`1s steps(60)` 抖动。
- 柔光：`.gloom:before` 全屏 `backdrop-filter: blur(10px)`，opacity .15（手机 5px / .2），像显像管的发光晕。
- 暗角：`.vignette:before` 一张 `crt.png` 拉满 100%×100%，opacity .7，scale(1.02)，z 100。
- 着色器：云片自写 ShaderMaterial（雾 smoothstep + 亮度 .9 + 色调 #DCE7F5，透明度乘 `pow(gl_FragCoord.z, 20.0)`），没有噪声函数；颗粒全靠 CSS。

## 可借鉴到巡航永恒
- 学：四层「屏幕」叠加全用 CSS、零 WebGL（颗粒 overlay .3 跳帧 + 暗角 PNG .7 + 全屏 blur 10px@.15 柔光 + 一条移动线）→ 巡航永恒全站胶片层：扫描线换成竖向划痕 / 片门抖动，颗粒用 `steps(1)` 跳位做出 24 格感，手机关颗粒。
- 学：悬停一个词改变三维场景速度（目标 .2↔2，每帧插值 .06，配一个 difference 盖层）→ 首页「一部短片」里悬停「油门 / THROTTLE」让公路镜头加速、转速表跳；声音打开时同步升转。
- 学：页脚字标残影（3 个克隆 + 底部 clip-path 裁切 + 按序号上移，滚动 scrub）→ 片尾字幕的 ETERNAL CRUISE 拖出长曝光尾迹。
- 学：10 条横向色条从底边长出换段（power4，stagger .04 从末尾，scrub）→ 章节换场做成「快门 / 百叶」，黑白之间硬切。
- 学：页头「● Get started」的光晕小灯 → 换成白色「● REC / 放映中」，同时当声音开关的状态灯。
- 不学：克莱因蓝 #03049C 和 CRT / 录像带年代感（巡航永恒是黑白电影，不是 VHS）；像素方格光标（太数码）；8000 张云片的三维天空（无头软渲染 9s 出不来，手机成本高）；「HOLD TO SKIM」操作不直观。

## 没读到
- 真机 GPU 下的开场节奏：无头软渲染里首屏 3D 9s 未出、22s 才出全，真实黑屏多久不知道。
- 悬停乱码、像素光标、按住浏览、「SPEED.」加速都只读了代码，没实测手感；Logo3D 从首屏到页头的停靠是看图判断。
- About / Journal / Start 子页没扫；页面进场只读到逐行进场，没截到转场过程。
- SemiSqueezed 字体真名与授权、内容来源（CMS）不明。
