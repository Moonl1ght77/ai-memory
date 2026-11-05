# Serotoninn 拆解

> 网址：https://serotoninn.com/ ｜ 类型：服装品牌电商官网（页面标「(UA)」、代码报错文案是乌克兰语，推测乌克兰女装品牌；口号「Where glam meets grunge」） ｜ 拆解：2026-11-04 ｜ 证据：桌面 0 帧、手机 0 帧（无头浏览器 9 帧全是 403 页）、CSS/JS 11 个文件（curl 下载）+ 首页 HTML 927KB；以下全部是读源码，画面效果属推断

## 一句话
把「glam meets grunge」做成画面：黑白照片上撕开一道口子，只露出半个人的原色，撕纸裂缝压在接缝上；全站所有文字都像卷帘一样从底边往上「拉开」，用同一条缓动 cubic-bezier(.75,0,.25,1)。

## 架构与技术栈
- 框架 / 渲染：WordPress 自建主题 `ref`（`wp-content/themes/ref`），jQuery 3.7.1、Yoast、PixelYourSite 11.4.0；多页站，转场后整页刷新（`window.location.assign`）。前端 Vite 打包成 ES 模块：`loader.js` 802KB 是依赖包，`main.js / footer-anim.js / custom-cursor.js / arrivals-anim.js` 从它 import。服务器 nginx，图片经 Google PageSpeed 改写（`.pagespeed.ce.`）。
- 动效 / 滚动库：GSAP 3.14.2 + ScrollTrigger + SplitText + CustomEase + Flip（证据：loader.js 版本串、`registerPlugin`）；Lenis 1.1.20（`new Lenis({lerp:.1, smooth:true, smoothTouch:false})`，自己 rAF，`window.lenis`）；Swiper；lottie-web（版本串 5.13.0，悬停「马克笔」划线和页脚按钮）；Fancybox 5.0.36。无 3D、无声音。
- 内容：首页 HTML 927KB，因为把全部商品 JSON 内联了两遍（`#all-products-data`、`#search__item` 285KB，给前端搜索 / 收藏用），页头抽屉里还有整套商品卡（搜索、收藏、「You may also like」跑马灯）；共 31 个 video、366 个 img。
- 页面结构（首页从上到下）：hero（计数加载 + 5 张大图轮播，钉住到下一段盖上来）→ arrivals 新品（横向卡片）→ campaign 整屏视频 → categories 无限拖拽横条 → best 爆款（画面左右展开 + 悬停播视频）→ bold 大字 + Lottie → motion（钉住，遮罩窗口放大）→ footer。

## 视觉与配色
| 角色 | 色值 | 用在哪 |
|---|---|---|
| 页面底 | #FFF9F7（暖白） | body 背景 |
| 文字 | #000000 | 正文 |
| 主强调 | #ED3833（`--Red-Main`，CSS 里出现 23 次） | 首屏卡片标题（彩色层）、价格、强调 |
| 反白 | #FFFFFF + `mix-blend-mode: difference`（25 处） | 页头、副标题压在照片上 |
| 次级底 | #EEDFDF（浅粉） | 卡片、抽屉底 |
| 灰 | #DEDEDE / #B9B8B8 | 细线、禁用 |
| 点缀 | #2EC1C5（青） | 少量图标 |
| 转场黑 | #000000 | 转场黑幕 |

## 字体与排版
| 角色 | 字体 | 字号 1440 / 390 | 字重 | 行高 / 字距 | 大小写 |
|---|---|---|---|---|---|
| 品牌名、副标题、计数 | Thunder（窄体，100–900 九个字重，来源和授权不明） | 80px（8rem）/ 手机隐藏 | 800 | 64px（.8） | 大写 |
| 首屏卡片标题 | Thunder | 118px（11.8rem，≥768）/ 约 75px（7.2rem） | 800 | 80% | 大写 |
| bold 段、页脚大字 | Thunder | 80px / bold 约 83px（8rem）、页脚约 67px（6.4rem） | 800 | 64px | 大写 |
| 大字旁的小注 `<i>` | Thunder | 30px（3rem） | 300 | 24px | 大写 |
| 商品名 | Inter（Google Fonts，开源 OFL） | 12px / 约 11.4px | 600 / 500 | 1.0 / -.04rem | 大写 |
| 等宽 | PP Fraktion Mono（Pangram Pangram，商用付费） | 未取到计算值 | 400 / 700 | — | — |
- 根字号按视口算：≥1024 `.6944vw`（1440 宽时 1rem=10px）、768–1023 `1.3021vw`、<768 `2.6667vw`（375 宽 1rem=10px），整页按设计稿等比缩放（推断，未截图核对）。
- 方括号当按钮：「[ be yourself ]」「[ SEE COLLECTION ]」；价格写「340,00_€」；季节标「(SS_“25)」「(UA)」，下划线、引号、括号都当装饰。
- 大字 + 细体小注贴底对齐（`align-items:flex-end`，3rem 300 字重贴 8rem 800 字重）。

## 布局
- `.container` 左右内边距 1.6rem / 3.2rem / 3.5rem（手机 / 平板 / 桌面，桌面即 35px）。断点 768、1024；`(hover:hover)` 13 处只给鼠标设备。
- 首屏高 `var(--inner-vh,100vh)`（修正手机地址栏）；5 张卡绝对定位叠放，图层 16:9、`object-fit: cover`。
- 版式：通栏图 + 大字压角；新品、分类是横向条；best 段 100vh（最高 81rem）。

## 动效
| 触发 | 效果 | 时长 | 缓动 | 实现 |
|---|---|---|---|---|
| 开场 | 首张卡 scaleY 0→1（底边起），延迟 .3s | 1.1s | power3.out | GSAP |
| 开场 | 「Loading」、三个圆点、计数 scaleY 0→1 | .7s，stagger .06 | `heroScaleEase` (.75,0,.25,1) | GSAP |
| 开场 | 预加载静帧按钟面扫开：`mask-image: conic-gradient(transparent 0 calc(var(--p)*1turn), #000 …)`，`--p` 0→1，计数 00%→100% 同步 | 3s | power2.out | CSS 变量 + GSAP onUpdate |
| 开场 | 三个点轮流 1 / .5 / 0 明暗 | 每步 .42s 循环 | none | GSAP |
| 开场 | 计数、Loading 收起（scaleY→0、yPercent→-100）；品牌名 / 副标题 / 按钮 scaleY 0→1；接着播一段 webm，播完淡出 .3s 露出首屏 | .55s / .7s | power2.inOut / (.75,0,.25,1) | sessionStorage `heroIntroDone` 第二次进来跳过计数 |
| 首屏轮播 | 每 5s 换一张：当前卡 scaleY→0（顶边收），下一张 scaleY 0→1（底边起）；标题晚 .8s 跟上 | 1.5s；标题 .7s | `heroImageEase` (.75,0,.25,1) | GSAP；首屏 pin 到 arrivals 顶到顶，`pinSpacing:false`，下一段盖上来 |
| 滚动 | 全站标题 / 正文 / 按钮 scaleY 0→1（底边或顶边起），一次 | .7s | 同一条 (.75,0,.25,1)，起了 6 个名字 | ScrollTrigger「top 95% / 75% / 100%」 |
| 滚动 | 新品图 scale 1.15→1 | 1.5s | power2.out | 「top 75%」 |
| 滚动 | best 画面 clip-path `inset(0 15% 0 15%)`→`inset(0)`，左右展开 | scrub「top bottom→top 20%」 | none | GSAP |
| 滚动 | motion 段钉住到页脚底；遮罩窗口宽 `--maskW` 30%→440%（平板 60→500，手机 90→1000），视频 scale 1.2→1（前 30% 进度） | 跟手 | 线性 | ScrollTrigger onUpdate + CSS mask（343×254 的 SVG 形状） |
| 滚动 | campaign 背景视频 y 10%→-10% | scrub「top 100%→bottom top」 | none | GSAP |
| 滚动 | 分类卡 yPercent [10,25,18,15]→0 | .8s | power2.out | GSAP |
| 滚动 | 页脚：字标 yPercent 300→0、图 100→0；链接逐字母从 a 翻到目标字母 | 1.2s；每字 .18s，stagger .04 | power3.out；线性 | footer-anim.js「top 80%」 |
| 悬停 | 商品卡多图依次 scaleX→0 换图（像翻页 / 快门） | .35s + .3s（4 图版 .8s 叠 .5s） | 自定 `HOVER_EASE` | `[card-hover]`，≤1024 关 |
| 悬停 | 链接 Lottie 马克笔划线（.5s 播完）+ 字符克隆滚动 | .5s | — | `[hover-link]`，只在 `(hover:hover) and (pointer:fine)` |
| 光标 | 区块内药丸光标，两组字每 2s 上下滚换（yPercent ±105）；出现时 scale .4→1.05→.9→1，按下缩到 .8 | 字 .35s stagger .1；弹 .167/.133/.167s | `textEase` (.75,0,.25,1)、`cursorEase` (.2,0,.1,1) | custom-cursor.js |
| 转场 | 点站内链接：黑幕（150vh 高，底边是 1440×328 的 SVG 形状）y -150vh→-50vh→0，然后整页跳；新页字体加载、拆字完成后黑幕 `translateY(105%)` 落下 | .8s + .8s；落下 .8s 延 .1s | `loaderEase` (.75,0,.25,1) | `#loader` + GSAP / CSS transition |

## 交互与组件
- 页头：「menu / Bag.0 / Shop all / Search / Categories / Favorites.0」，加号图标用 difference 混合；页头文字悬停有 Lottie 马克笔。
- 搜索、收藏、购物车都是抽屉（`data-lenis-prevent` 让内部可滚），搜索用内联 JSON 前端现查。
- 分类横条：列表克隆 3 份做无限循环；拖动带惯性（速度平滑 .85/.15，松手 power3.out）；没人碰时自动漂移 66.7px/s；两端的卡被压扁 scaleX→0，像穿过画框。
- 视频懒加载（IntersectionObserver，iOS 单独处理）；订阅弹窗用 sessionStorage 记住。
- 手机端：<768 首屏品牌名和副标题隐藏；光标、卡片悬停、motion 光标都关。
- 「减少动态效果」：CSS 和 5 个 JS 里 `prefers-reduced-motion` 出现 0 次，没有处理。

## 质感
- 颗粒 / 噪点 / 胶片：无（CSS 里没有 grain、noise、feTurbulence）。
- 黑白 + 局部彩色：`.hero-card__img-bw { filter: grayscale(1) }` 做底；`.hero-card__img-color` 叠在上面，`mask-image: url(/img/mask_img.webp)`（看图判断：这张遮罩只留人物左半身，右边是撕边）只露出半个人的原色；`.hero-card__img-line` 是一条竖向撕纸裂缝图，`left: calc(50% + .2rem)` 压在接缝上。卡片标题黑白层白字、彩色层红字。
- 混合模式：difference 25 处、exclusion 1 处（页脚字标）。

## 可借鉴到巡航永恒
- 学：conic-gradient 钟面扫开的预加载（`--p` 0→1，3s power2.out，计数 00%→100%）→ 巡航永恒片头直接做成电影倒计时片头（8-7-6… 扫针 + 十字线），扫完切到冷启动镜头。
- 学：黑白底 + 遮罩露出局部原色 + 撕裂线压缝 → 黑白为主不变，只在「改装文化」或车型对比里用一道胶片接缝露出一小块原色（不用橙色）；或悬停时撕开看原色。
- 学：画面随滚动左右展开（`inset(0 15%)`→0，scrub 到 top 20%）→ 改成宽银幕：上下黑边随滚动合拢，16:9 变 2.39:1，用在车型详情首屏往下的第一镜。
- 学：钉住 + 遮罩窗口从 30% 放到 440%，视频 1.2→1 → 首页「一部短片」里用圆形头灯形遮罩从小到满屏打开公路镜头。
- 学：一条签名缓动 (.75,0,.25,1) 用遍全站，所有文字同一种 scaleY 展开 → 巡航永恒只定 1 条缓动 + 1 种字卡出场。
- 学：页脚逐字母 a→z 翻牌（.18s/字，stagger .04）→ 片尾字幕或「年代史」年份做成翻牌钟。
- 不学：暖白 #FFF9F7 + 红 #ED3833 配色；285KB 商品 JSON 内联进 HTML（首页 927KB）；整页刷新转场；完全不处理减少动态效果；无头浏览器一律 403（会挡掉预览图和抓取工具）。

## 没读到
- 无头浏览器（Edge headless）请求首页被 nginx 返回 403，判断是拦无头浏览器（默认 curl 拿到 200）；按规矩没有改 UA 绕过。所以没有截图、没有计算样式、没有真实开场时长和观感。
- Lottie JSON、首屏视频和大图没看；mask_img.webp 只看了遮罩形状；transition_logo.svg 只看了尺寸（4 个黑色圆角形）。
- 商品页、分类页、关于页、手机端都没看；PP Fraktion Mono 的实际使用位置没取到；Thunder 来源和授权不明。
