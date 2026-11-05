# CULTISH™ 5 拆解

> 网址：https://5.cultish.com/ ｜ 类型：品牌周年纪念站（开普敦街头服饰品牌 Cultish 五周年，档案照里印着 33.9249° S, 18.4241° E；400 张黑白照片的 WebGL 漫游；Teamgeek 制作，Awwwards Honorable Mention 2026-03-29） ｜ 拆解：2026-11-05 ｜ 证据：桌面 19 帧、手机 5 帧（都停在 STEP INSIDE 入口门，脚本不点击，门后没看到）、CSS/JS 8 个文件（主包 4.0 MB + scene 分包 20 KB）+ curl 抽样 12 张档案照片验色

## 一句话
整站是一卷翻拍出来的黑白街拍底片：先给一张歪着放的颗粒黑白照片和手写字「These are our cities / These are our people」，点「Step inside」后进入 3D 空间，在 5 年 400 张黑白照片里飞；全屏一层 80% 不透明度的跳动噪点把所有东西压成同一种胶片底。

## 架构与技术栈
- 框架 / 渲染：React（Vite 打包，index-*.js / scene-*.js 懒加载分包）+ React Three Fiber，three r182；两块 webgl2 canvas：1440×900 全屏场景、576×546 一块（推测画入口字，summary.canvases）。Tailwind v4（`--spacing:.25rem` 等变量）。
- 动效 / 滚动 / 3D / 声音：Motion（framer-motion，Awwwards 技术栏写 Motion；源码有其 prefers-reduced-motion 侦测）；Lenis（`duration:1.6, lerp:.08, smoothWheel:true, wheelMultiplier:.425, touchMultiplier:.45`，它的 raf 再分发给场景订阅者）；drei KeyboardControls；zustand 管加载进度。声音：`/music/blxckie.mp3` 6.5 MB（文件名推测是同名南非说唱歌手的曲子），声音开关图 blxckie-on / off.png，入口提示「sound on」。
- 托管与内容来源：Cloudflare（响应头 cf-ray）；照片同域静态 `/年份/序号.jpg`：2025 年 146 张、2021 年 103、2022 年 75、2020 年 52、2024 年 24，共 400 张 37.5 MB，桌面端开场就全部请求（手机 152 张 13.8 MB）。
- 页面结构：加载条 → 入口门（歪放黑白照 + 手写两行字 + 「sound on」）→ 「STEP INSIDE」故障字（点击进入）→ 3D 照片漫游，按年份分组，年份标签是 `/assets/years/{年}.png` 手写图，切换时 opacity 0→1→0 → 菜单（index / respect / note-to-self / domain / credits / copyright，全是手写 PNG 字）。门后结构来自源码，没截到。
- 路由：单页，无路由。

## 视觉与配色
| 角色 | 色值 | 用在哪 |
|---|---|---|
| 底色 | #000000 | body、加载层（`--color-background`、`--color-loader-bg`） |
| 文字 / 手写字 | #FFFFFF | 所有 UI 字（UI 字本身多是白色手写 PNG） |
| 加载条 | #65523C，底槽同色 10% | 唯一的彩色：一点咖啡棕（`--color-loader-bar-fill`） |
| 照片 | 0 饱和度灰阶 | 抽样 12 张 HSV 饱和度均值 = 0，全是黑白 |
| 门页背景 | 看图约 #0A0A0A–#141414 | 黑底叠噪点后的实际观感，四角更暗（看图判断） |

## 字体与排版
| 角色 | 字体 | 字号 1440 / 390 | 字重 | 行高 / 字距 | 大小写 |
|---|---|---|---|---|---|
| 入口标语 / STEP INSIDE / 年份 / 菜单 | 手写字 PNG（非字体） | 看图：标语约 30px 高、STEP INSIDE 约 45px / 手机约 20px | — | — | 手写大写 |
| UI 小字（按钮、提示） | ui-monospace（系统等宽） | 12px / 12px（行高 18px） | 400 | 1.5 / 0 | 原样 |
| 备用 | Asul 400/700、DM Sans 可变（Google Fonts，开源 OFL） | 门页未出现（summary.loadedFonts 为空） | — | — | — |

- 排版手法：几乎不排字。所有有态度的字都是手写图片（马克笔质感、笔画抖），等宽系统字只做功能提示；照片是主角，字是贴在照片上的胶带标签。

## 布局
- 版式：全屏单幕，scrollHeight = 900（不滚页面，3D 场景接管 wheel、拖拽、触摸、键盘）。入口照片约 320×490px，居中、顺时针歪约 2°（看图判断）。
- 断点：Tailwind 默认 40 / 48 / 64 / 80 / 96rem，外加 `(hover:hover)`。
- 手机：同一扇门，照片缩到约 100×150（看图）；场景里单指拖动 = 前后移动（scrollAccum 钳在 ±15），双指 = 平移。

## 动效
| 触发 | 效果 | 时长 | 缓动 | 实现 |
|---|---|---|---|---|
| 常驻 | 全屏噪点层平移跳动（见质感） | 8s 一轮，每 0.08s 跳一格 | steps(10, end) | CSS `body:after` + `@keyframes animateGrain` |
| 开场 | 入口元素淡入上移 | 500ms | cubic-bezier(0,0,.2,1) | CSS keyframes fadeInUp / fadeIn（Tailwind --ease-out） |
| 悬停入口字 | STEP INSIDE 故障：按 50 条横带随机横移（强度 ×0.2）、15 段纵向错位 0.08、RGB 分离 0.008×aberration、400 线扫描线 ±5%、随机丢块 | 着色器每帧，time×20 换一次 | — | 自写片元着色器（uniform glitchIntensity / corruption / aberration / clipProgress），只在字的蒙版内生效 |
| 进入 | clipProgress 0→1 从左到右擦出字 | 看不到时长 | — | 同上着色器 `if (uv.x > clipProgress) discard` |
| 场景 | 速度插值：拖动/滚轮写 targetVel，velocity 追 targetVel；静止时随鼠标漂移（系数 8×clamp(z/50, .3, 2)，追随率 .12，移动中 .2） | 每帧 | 线性插值 | scene 分包 useFrame |
| 年份切换 | 年份手写图 opacity 0→1→0 | 看不到时长 | — | Motion |
| 其他 | 浏览器标签页图标逐帧动画，5 帧轮播 | 每 200ms 一帧 | — | inline-script-0 setInterval |

## 交互与组件
- 入口门：一张照片 + 一句手写标语 + 「sound on」提示，点「STEP INSIDE」才进，进门这一下也是开声音的那一下（浏览器自动播放规则）。
- 光标：自定义像素箭头 / 手指 PNG（`--cursor-default`、`--cursor-interactive`）。
- 场景操作：鼠标拖拽、滚轮、触摸（单指、双指）、键盘（前后左右上下 6 个方向键位）。
- 加载：zustand 记 THREE.DefaultLoadingManager 进度，咖啡棕细条。
- 性能护栏：每 10 秒查一次 JS 堆（某个开关为真时 5 秒，推测是手机分支），占用 >90% 就把速度清零；每 30 秒清一次纹理缓存（同一分支 45 秒）（scene 分包 `Low memory detected`、`Texture cleanup`）。
- 减少动态效果：CSS 噪点动画没有 `prefers-reduced-motion` 关闭规则（media 列表里没有）。

## 质感
- 颗粒做法：**JS 一次性生成噪点贴图 + CSS 叠层平移**。开场 `bq()` 在 128×128 的 canvas 上逐像素填随机灰度（R=G=B=random×256，alpha=30/255≈12%），`toDataURL('image/png')` 存进 CSS 变量 `--hero-noise-image`，尺寸 `--hero-noise-size:128px 128px`。
- 叠层参数：`body:after`，position fixed，`inset:-100%`、宽高 300%，`opacity:.8`，z-index 3000，`pointer-events:none`，平铺；混合模式 normal（不用 overlay），等效每像素约 9.4% 的灰白点。
- 刷新：`animation: 8s steps(10,end) infinite animateGrain`，关键帧在 (0,0)、(−5%,−10%)、(−15%,−20%) 之间来回；10 段×10 步 = 每 80ms 跳一格（约 12.5 帧/秒），每步位移约 20px，噪点在屏上等于每 80ms 换一张。
- 性能：噪点只生成一次（128px PNG，约几十 KB），之后只动 transform，走合成层，主线程零开销；代价是一张 3 倍屏宽的平铺层常驻顶层。
- 叠加：照片本身就是高反差、粗颗粒的黑白街拍（抽样 12 张全是灰阶），代码噪点再盖一层，两层颗粒叠出「翻拍的底片」感；STEP INSIDE 的 RGB 分离是全站唯一的彩色。

## 可借鉴到巡航永恒
- 学：噪点做法整套照搬：开场生成 128px 灰度噪点 PNG（alpha 约 30/255），fixed 伪元素 300% 平铺，opacity .8，`steps(10)` 8s 跳动 → 全站颗粒层；「轻 / 中 / 重」三档只改 opacity（如 .35 / .6 / .8）和 alpha，一行变量切。
- 学：照片先「做旧」再上墙：素材统一转灰阶、拉高反差，代码颗粒只是第二层 → 「放映日记」或「胶片印样」里每一格的车照处理规范。
- 学：入口门 = 一张歪放的照片 + 一句手写标语 + 「声音开」提示，点一下进场同时开声音 → 发动机冷启动之前的那一下点击（默认静音，点了才响）。
- 学：手写 PNG 字当胶带标签（年份、菜单）→ 年代史时间轴的年份、改装文化章节的批注。
- 学：内存护栏（堆占用 >90% 停速、定时清纹理）→ 爆炸图、3D 发动机页的保底。
- 不学：400 张照片 37.5 MB 开场全量请求，应按视口懒加载；噪点没给「减少动态效果」关停；字全是图片，读屏和翻译都读不到。

## 没读到
- 入口门之后的 3D 漫游、菜单、年份切换都没截到：扫描脚本只滚不点，没有绕过入口；门后描述全部来自源码（scene 分包、主包字符串），属推测。
- 声音、拖拽和飞行的手感、照片在 3D 里怎么排布（隧道 / 网格 / 散布）不知道。
- 四角压暗是图片自带还是场景里的暗角，没在源码里找到对应代码。
- 手机端是否也加载全部 400 张：扫到 152 张时还停在门口，后续不明。
</content>
</invoke>
