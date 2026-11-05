# Santioni Spirits 拆解

> 网址：https://santionispirits.com/ ｜ 类型：品牌官网（单页 WebGL 叙事） ｜ 拆解：2026-11-04 ｜ 证据：桌面 17 帧、手机 8 帧（扫 2 次，第 2 次 `--wait=24000` 才等到年龄门；按要求没点 Yes）、CSS/JS 13 个文件 + curl 取着色器包 compiled.vs、UIL 参数表、2 张贴图

## 一句话
把品牌做成一部会动的铜版画短片：所有 3D 场景用「线条贴图 + 三档光影 + 每秒 8 帧跳动」渲染成版画，配旁白、逐词点亮的字幕和随场景换层的环境声；年龄门那一下点击，同时也是打开声音的那一下。

## 架构与技术栈
- 框架 / 渲染：Active Theory 自研 Hydra（inline-script-1：`CREATED_WITH_HYDRAX = "1.1.20"`）。一块全屏 WebGL2 canvas 垫在 DOM 下；1440×900、设备 DPR 1 时画布仍是 2160×1350，即固定按 1.5 倍超采样（summary.canvases）。html/body `overflow:hidden; touch-action:none`，原生滚动不动（帧名全是 y0）。summary.framework 为空：没有 React / Vue。
- 动效 / 滚动 / 3D / 声音：全部 Hydra 自带：TweenManager（含自定义缓动）、Scroll（虚拟滚动）、SplitText（自带，不是 GSAP）、GLUI（PP Nikkei Maru 的 MSDF 图集，WebGL 里排字）、MouseFluid（鼠标流体）、AudioManager（Web Audio + FFT）。外部只有 lottie-web（加载动画，svg 渲染）、Draco / Basis 转码 wasm（52 个 .bin 几何 + .ktx2 贴图）、GA4。没有 GSAP、Lenis、Three（hits.libs；`Theatre` 只是开发期可选编辑器，生产没加载）。
- 托管与内容来源：资源同域 `/assets/`，响应头只有 `x-cache: HIT`；参数资源在 storage.googleapis.com/at-uil-assets、地理接口在 us-central1 cloudfunctions，推测 GCP。零售商列表、隐私 PDF 来自 Sanity（inline JSON）。页脚外链 activetheory.net、plan8.co（推测为制作与声音署名）。首次加载约 18 MB：fetch 210 个 14.9 MB（音频 8.7 MB、几何 2 MB、贴图约 2 MB），app.js 2.09 MB。
- 页面结构：Loader → 年龄门 → 18 幕故事（app.js `sceneConfigs`，高度单位 = 屏）：Wander 2.5 → Profile 1.25 → Approach 2（手机 1.25）→ Near 3（手机 1.25）→ Hand 1.5 → Target 自适应 → Transition 2（手机 1.5，margin-top −0.1）→ Cathedral 4（margin-top −0.5）→ DrinkSelection 2.5 → DrinkPour 4 → AntiGravity 2（手机 1.8）→ PillarCrumble 1.25 → Colosseum 3.25 → Taste / Collection / Products / Retail / Footer 按 DOM 内容自适应。合计约 33 屏（Retail 区块 top 30017px ÷ 900）。每幕在 UIL 里是分层编排（AntiGravity 73 层、DrinkSelection 35、Products 29、Collection 21、Colosseum 18、Hand 17）。
- 路由与转场：单页 `/`，没有页面转场。菜单 experience 回顶、collection 用 scrollTo（easeOutCubic）跳过去，跳的途中压住各幕转场音（`suppressAudioUntilScrollSettles`）。幕与幕之间靠负 margin 重叠、TransitionScene 和 PillarCrumble 的裂石音效过渡。

## 视觉与配色
| 角色 | 色值 | 用在哪 |
|---|---|---|
| 底色 | #1D1D1D | body、Loader、年龄门（inline-style-0） |
| 深字 | #121212 | Taste、Retail、Collection 的文字色，据此推测这几幕是浅底 |
| 主文字 | #FFFFFF | 年龄门大字、按钮框、菜单和 Cookie 底板 |
| 品牌红 | #C82924 | 按钮和链接的 hover / focus、产品幕背景 uBGColor |
| 深红 | #BE261E / #990B06 | Cathedral 背景、Collection 色块（UIL） |
| 版画棕 | #B09C76 / #AE9C74 | 排线着色器的中间调、教堂地面（compiled.vs、UIL） |
| 金 | #DFAF49 | AntiGravity 地面（UIL） |
| 口味高光 | #72C5F6 / #97F3AD / #FBEB7F | 三支瓶子 hover 时的高光（UIL） |

## 字体与排版
| 角色 | 字体 | 字号 1440 / 390 | 字重 | 行高 / 字距 | 大小写 |
|---|---|---|---|---|---|
| 年龄门问句 heading2 | Charles Rosie Regular | 159px（calc(8.49673vw + 36.86px)，≥1920 封顶 200px）/ 70px | 400 | 1（`text-box-trim: trim-both; text-box-edge: cap alphabetic`）/ .005em | 全大写 |
| heading1 | Charles Rosie Regular | 217px（≥768 固定）/ 70px | 400 | 1.2 / .005em | 全大写 |
| heading3、零售区标题 | Charles Rosie Regular | 82px（≥768）；零售区 76px（容器查询 `max(30px, min(16cqw, 82px))`）/ 40px | 400 | 1.2（零售区 .85）/ .005em | 全大写 |
| 按钮、标签、菜单 | PP Nikkei Maru Ultrabold | 15px / 12px（calc(.7937vw + 8.9px)） | 800 | 1.2 / .005em | 全大写 |
| 正文 | GT Era Text Light | 15px / 15px | 300 | 1.5 / .005em | 常规 |
| 故事里的字幕 | PP Nikkei Maru Regular（MSDF 图集 png + json） | WebGL 内 | 400 | — | 常规 |

授权：Charles Rosie 来源不明（自托管 woff2 / woff / otf，31 KB）；PP Nikkei Maru 商用付费（Pangram Pangram）；GT Era 商用付费（Grilli Type）。
- 排版手法：一个复古展示体只做全大写大字，配一个 800 粗的小号无衬线做按钮，大小比约 10:1。年龄门把一句话拆成上下两行贴住屏幕上下边，按钮夹在正中，中间放一枚新艺术风格的竖向花饰（SVG）。流体字号用 `calc(vw + px)` 分段写，到断点锁成固定 px，不用 clamp()。`text-wrap: balance`（Safari 除外）。

## 布局
- 版式：通栏舞台。每幕宽 100vw、高 N 屏，`--height`、`--margin-top` 由 JS 按 Stage.height 写进去；`.Story` 整体 position:fixed，DOM 层的 y 等于滚动值，和 3D 相机共用一个滚动数。
- 内容区：ProductsUI max-width 1600px，左右 padding calc(9.03vw − 37.3px)，1920 时 136px；RetailUI max-width 950px、组间 gap 100px；Taste 三列各占 23vw，中间段落两端对齐；Footer padding 45→65px、gap 100→120px。
- 断点：320 / 390 / 400 / 768 / 959 / 1024 / 1279 / 1280 / 1920，hover 只在 `(hover:hover) and (pointer:fine)` 下生效。
- 手机：同一套幕，长幕压短（见页面结构）；滚动改成拖拽、lerp .9；横屏时盖一层 RotatePrompt 提示竖过来。

## 动效
| 触发 | 效果 | 时长 | 缓动 | 实现 |
|---|---|---|---|---|
| 开场 | Logo 淡入 + y 2%→0；Lottie 圣像纹章线稿 0.5 倍速，至少放 3000ms 才放行 | 1000ms | easeInOutSine | LoaderView（app.js） |
| 开场 | 进度条：SVG 路径每秒重算 8 次（8 个噪声点），stroke-dashoffset = 长度 ×（1 − 进度），像手画的线在抖 | 8fps | — | `startRender(updateBarPaths, 8)` |
| 开场 | 年龄门：两行字从 y ±110% 推到中间叠在一起（看图：帧 mobile-scroll-04 两行重叠、半透明）→ 停 1200ms → 分别推到屏幕上下边；同时按钮从交叉位置滑回、花饰 scale 0→1；最后开鼠标倾斜 | 1000 + 1000 + 2000ms（delay 400） | easeInOutCubic；分开那步 easeInOutQuart | AgeGate；`.top/.bottom{clip-path:inset(0)}` 当遮罩 |
| 转场 | 点 Yes：上下两行各自滑出，整层淡出；第一层 drone 音量 0→1 | 1000ms（淡出 delay 300）/ 1200ms | easeInOutCubic / easeOutSine | AgeGate、`Story/tweenInDrones` |
| 滚动 | 虚拟滚动插值：桌面 lerp .08、手机 .9；滚动速度带相机俯仰 rotation.x = lerp(0.002 × 速度, 当前, .05)，钳在 ±0.2 rad | 每帧 | 线性插值 | Story loop |
| 悬停 | 手绘框按钮：边框位置被噪声贴图扰动、8fps 跳；hover 边框加粗 +0.03、抖幅 0.01→0.02；DOM 同时变 #C82924 | 600ms / 100ms | easeOutCubic / ease-in-out | GLUIButtonShader、`.AgeGate .action` |
| 悬停 | 页脚链接两段下划线交替 scaleX；菜单字 opacity .5→1 | 300ms（+100ms）/ 400ms | 默认 / ease-out | inline-style-0 |
| 交互 | 选酒：hover 的那支悬浮 + 自转，其余落回；倒酒：按住 pourProgress 0→1，松手回落，进度到 0.24 起倒酒声 | 800 / 600ms；3000 / 1000ms | easeOutCubic；linear | DrinkSelectionScene、DrinkPourScene |
| 交互 | 产品轮播：无限循环、可甩（friction .3、interpolate .3）；文案贴图 uTranslateIn 推入 | 1200ms；2000ms | easeOutCubic；linear | ProductsScene `Interaction.Slider` |
| 全站统计 | 最多的是 1000ms easeInOutCubic（39 处），UI 用 300–600ms easeOutCubic；自定义 speedUpEase (.52,.02,.02,1)、collectionEase (.30,.09,0,1.05)、textBoxEase (.14,1,.34,1)、productsEase (.22,1,.36,1) | — | — | app.js tween 统计 |

## 交互与组件
- 导航：右上角固定小白板（top 20→25px、right 20→24px、高 40px），底板是 3px 描边的手绘框 SVG；两个链接 12→15px、opacity .5，hover 到 1。底板开场延迟 4000ms 后 scaleX 0→1（1100ms easeOutCubic）。
- 声音开关：右下 4 根竖条，各接一个 FFT 频段（20–600 / 600–1k / 1k–2k / 1.5k–22k Hz，灵敏度 .75 / 1 / 1 / 2.75），24fps 刷新，`mix-blend-mode: difference`，`aria-pressed`；静音淡出 50ms，开声淡入 200ms。
- 声音设计：5 层 drone 按幕内滚动进度交叉淡化（approach、cathedral 0→1，antiGravity −.5→.5，taste −.25→.5）；环境 jazz、wind 两轨；18 段旁白；一次性音效轮换取用（文本框 6 个、进出脚步 7 / 6 个、轮播 3、瓶子 hover 3），避免同一声反复；PillarCrumble 入场裂石声（增益 .75）。ogg 优先，mp3 兜底。
- 旁白字幕：文本框过视口 60% 且用户停手 ≥100ms 才排队播放；播放时用 AudioContext.currentTime 驱动 MSDF 文字的 uKaraokeTime，逐词点亮（点亮强度 400ms 进、600ms 出，easeOutCubic）。
- 拖拽控件：120px 白圆、3px 黑边、`cursor: grab`；HandUI 另有 1px 半透明连线 + 16px 黑点的「牵引线」。
- 年龄门按钮：底下的 DOM 按钮 96×48、3px 白框负责点击和读屏，≥768px 用 `:before` 撑出 3 倍宽的方形热区；看到的画面是叠在上面的 WebGL 按钮。拒绝时整页换成另一段提示。
- Cookie 条：右下 370px 白卡，同款手绘框底板；`?skip` 参数可跳过 Loader（开发用）。
- 无障碍：WebGL 文字都有 sr-only DOM 镜像（XText `addSrOnly`、GLA11y），focus-visible 用红色；没有 prefers-reduced-motion 处理（源码 0 处）。
- 手机：拖拽滚动；产品左右箭头改成 48px 白圆；菜单花饰 <768 隐藏。

## 质感
- 全局纸纹（CompositeShader）：蓝噪声贴图 bluenoise0.png 按宽高比平铺 5 次，跟着滚动走（uv.y += uScrollY × 1.5），取值噪声 × 2 + 0.1，multiply 混合 10%。静止时不闪，滚动时像纸在走。
- 版画排线（BaseShader 等 10 余个着色器）：lines.jpg（1024² 波浪竖线）按物体坐标绕轴 (1, 0, 2.5) 转 0.5 rad 贴上，每步平移 0.15；光照用 aastep 分三档（阈值 0.1 / 0.9 / −0.91），排线扰动明暗交界（系数 0.3 / 0.075 / 0.2）；亮部用噪声把线打散成点；只用黑、#B09C76、白三色；背面纯黑。
- 每秒 8 帧：`steppedTime = floor(time × 8)`，排线、按钮边框、进度条、年龄门烟雾都按 8fps 跳，像手绘动画一拍三。
- 年龄门烟雾：鼠标流体被蓝噪声和 8fps 噪声切碎，只在 sin(流体 × 4) > 0.9 处提亮 +0.02，很淡。
- 模糊：kawase blur，蓝噪声抖动，uBlurAmount .15。
- 版画插画：Loader 的圣像纹章线稿（Lottie）、年龄门花饰、手写体 Logo（看图判断）。
- 没有：SVG feTurbulence、CSS 颗粒层、暗角、棕褐滤镜（summary.grain 全空）。

## 可借鉴到巡航永恒
- 学：8fps 步进时间 → 全站胶片颗粒、片门抖动、字幕抖动都按 floor(t × 24) / 24 或 / 12 更新，不跑 60fps；放映机感更强，GPU 也省。
- 学：年龄门兼开声 → 片头字卡放「有声观看 / 静音观看」两个按钮（默认静音不变），选有声那一下顺便解锁 AudioContext，引擎底噪 1200ms easeOutSine 淡入。两行片名先在中间叠合 1000ms、停 1200ms、再推到宽银幕黑边上下 1000ms easeInOutQuart，正好是开幕。
- 学：幕高配置表 → 首页「一部短片」每个镜头一行 {场景, 高度(屏), 手机高度, 负 margin}，长镜 3–4 屏和喘气镜 1.25 屏交替；DOM 字幕和 WebGL 共用一个滚动值（桌面 lerp .08），滚动速度给相机 ±0.2 rad 俯仰，像手持跟拍。
- 学：按住倒酒 → 发动机冷启动做成「按住点火」：按住 3000ms linear 转速走满、松手 1000ms 回落、进度 0.24 起怠速声。
- 学：旁白逐词点亮 → 年代史、改装文化章节的字幕按音频时间逐词点亮；字幕框过视口 60% 且用户停手 100ms 才播；用菜单跳章节时压住途经章节的声音。
- 学：随滚动走的颗粒 → 黑白页面整层叠 multiply 10% 的蓝噪声，跟滚动走、静止不闪，作为最轻一档颗粒。
- 不学：铜版画排线着色器（插画路线，巡航永恒要黑白摄影感，图纸方向已被否）；宗教题材和红色主调；HydraX 私有框架（换 Three.js 或 OGL + Lenis）；首次 18 MB 的加载量、不处理减少动态效果。

## 没读到
- 年龄门之后的 18 幕没有画面（按要求不点 Yes）：故事场景、产品陈列、倒酒、滚动节奏全部来自源码和 UIL 参数，手感没验证；浅色段是由 #121212 文字色推测的。
- 声音没听，只读了文件清单和配置；旁白时间轴 all_timestamps.json 没读（避免搬文案）。
- 无头 swiftshader 下桌面 16s 以上还在 Loader，24s 才拍到年龄门，手机约 12s；真机速度没测。
- 着色器只细读了 CompositeShader、BaseShader、GLUIButtonShader，其余 140 多个（Cathedral、Colosseum、Liquid、Wander 等）没逐个看。
- Charles Rosie 的出处和授权没查到；托管商是推测。
- 手机只拍到 Loader 和年龄门。
