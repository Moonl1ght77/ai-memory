# Bucks Sauce 单品页拆解

> 网址：https://buckssauce.com/shop/crushed-habanero-garlic ｜ 类型：产品页（电商单品） ｜ 拆解：2026-11-04 ｜ 证据：桌面 19 帧、手机 5 帧（只扫 1 次，主站返回 200，没被限流）、CSS/JS 84 个文件（JS 57、CSS 1、内联 26）

## 一句话
把一瓶酱做成一叠贴纸和卡牌：边缘粗糙的大写标题、带虚线内框的彩色卡、会弹出来还带表情动画的配料图；卖点不写长表格，而是让两张卡「洗牌」互换。全部动效由一套 GSAP 时长 / 缓动 / 错峰数值驱动。

## 架构与技术栈
- 框架 / 渲染：Next.js 16.1.4（App Router、Turbopack；`next={version:"16.1.4"}`）+ React 19 canary；Tailwind v4（`--text-*--line-height` 变量写法）。响应头 `server: Vercel`、`x-nextjs-prerender: 1`。
- 动效 / 滚动 / 3D / 声音：GSAP 3.14.2（ScrollTrigger、SplitText、Draggable、InertiaPlugin、CustomEase，配 useGSAP）；Lenis（`<ReactLenis root autoRaf={false} options={{smoothWheel:true}}>`，默认 lerp .1，挂在 GSAP ticker 上，scrollRestoration 设 manual）；Three r182 + React Three Fiber + drei（Stage、studio HDRI）；dotLottie-web 0.71.0（wasm 1.69 MB）；没有声音。
- 托管与内容来源：Vercel；内容来自 Prismic（images.prismic.io 82 个请求），商品和购物车走 Shopify Storefront（组件 props `shopifyProduct.variants`）；另有 Vimeo 视频、Cloudflare Turnstile（订阅表单）、LogRocket、Meta Pixel、GA。页脚署名 buzzworthystudio.com。
- 页面结构（sections）：公告条 → 固定头 70px → 首屏（商品图轮播 + 大标题 + 购买模块 + 卖点盒，高约 868）→ 创始人视频 + 5 条卖点（约 1003）→ 配料 / 营养双卡 + 弧形大标题 + 3D 配料传送带（约 1081）→ 食物照片双栏（约 727）→ 四口味卡片（约 728）→ 页脚订阅。桌面总高 5408px，手机 8679px。
- 路由与转场：/shop/<口味> 四个单品 + /shop/build-your-own（3 瓶自选）；每页进场时间线统一延迟 100ms（`PAGE_ANIMATION_DELAY`）；每个单品页把主题色设成该口味的颜色（`setCurrentVisitedProductTheme(color)` → `--color-theme`）。路由级转场没确认。

## 视觉与配色
| 角色 | 色值 | 用在哪 |
|---|---|---|
| 底色 | #100B06 | 全站背景（暖黑） |
| 前景 | #F5E4C7 | 文字、描边按钮、圆形箭头底色 |
| 次底 | #272119 / #322C23 | 促销卡底、菜单和公告条 |
| 橙 | #F15726 | 加购按钮、tab 手绘圈、小标题 |
| 金 | #BE8D3F | 默认主题色、菠萝口味卡 |
| 红 | #DA1F27 | 樱桃口味卡 |
| 桃 | #EF8F4A | 桃子口味卡 |
| 前景半透明 | #F5E4C7 @10% / @30% | 卡片底、分隔虚线 |

## 字体与排版
| 角色 | 字体 | 字号 1440 / 390 | 字重 | 行高 / 字距 | 大小写 |
|---|---|---|---|---|---|
| 商品名 H1 | Peperoncino Sans | 100px（text-100 = clamp(4.44rem, 6.94vw, 8.33rem)）/ 70px | 400 | .88–.9 / −.02em | 全大写 |
| 弧形大标题 | Peperoncino Sans | 140px（xl:text-[140px]，字母沿 SVG 曲线 `M50,300 Q500,135 950,300` 排）/ 60px | 400 | — | 全大写 |
| 卡片标题、H2 | Peperoncino Sans | 40px（行高 36px）、36px / 32px（行高 28.8px） | 400 | −.02em | 全大写 |
| 按钮、价格 | Peperoncino Sans | 24px（加购、价格）、20px / 24px | 400 | 1–1.25 / −.02em | 全大写 |
| 大段介绍 | Inter Tight | 40px（行高 44px，−.8px）/ 16px | 400 | — | 句子 |
| 卖点条目、tab | Inter Tight | 20px（行高 30px） | 400–500 | −.02em | 全大写 |
| 正文、口味说明 | Inter Tight | 16px（行高 20–24px） | 400–700 | 0 | 句子 |
| 公告条 | Inter Tight | 14px | 700 | .08em | 全大写 |

授权：Peperoncino Sans 不明（自托管 woff，文件名 `PeperoncinoSans_Regular_fixed`，推测商用）；Inter Tight 开源 OFL（next/font 从 Google Fonts 打包）。
- 排版手法：粗糙展示体只给大写短句，长文全交给 Inter Tight。字号 token 按 1440 设计稿的 px 命名（text-26 … text-300），值 = px ÷ 14.4 vw 再加 clamp 上下限；标题行高压到 .8–.9。价格写在加购按钮右端；一处标题沿弧线排；手机菜单链接放到 60px。

## 布局
- 栅格：12 栏（`--grid-col: calc(100vw/12)`、`--grid-gap: 1.25rem`，`w-col-4` 按栏宽算）。页边距只有 5px→10px（p-container），卡片间距 10px，圆角 10px 为主（69 处）、24px 次之，几乎贴边。
- 版式：首屏桌面是 4 栏标题 + 余下宽度的图片框（源码 `lg:w-col-4` + `flex-1`，桌面首屏没拍到）；视频区左右各半；配料区居中叠卡；口味卡 4 栏（grid 4 cols、gap 10px）；手机全部单栏。
- 断点：Tailwind v4 默认 40 / 48 / 64 / 80 / 96rem + 144rem，另有 `(min-width:64rem) and (max-height:719px)` 照顾矮屏笔记本；hover 只在 `(hover:hover)` 下生效。
- 图片框：首屏商品图的 clip-path 由 JS 算出（圆角 10 的矩形），桌面左上角挖掉 8.5rem × 4.25rem 的缺口，ResizeObserver 随尺寸重算。

## 动效
| 触发 | 效果 | 时长 | 缓动 | 实现 |
|---|---|---|---|---|
| 开场 | 首屏时间线：花纹底 opacity 0→1；标题逐字从 y 60、scaleX .8、scaleY .5 弹到原样（错峰 .028s）；图片框 y 40→0；信息盒 y 100 + 随机 ±30° 旋转回正（错峰 .035s）；配料图 scale 0→1 | .5s；.48s；.8s；1.25s；.5s | power2.out；back.out(1.7)；back.out(1.4)；自定义 bigCopy（冲到 1.1 再回 .978 再停 1）；back.out(1.4) | js-08 首屏组件 + js-18 预设 |
| 滚动 | `[data-gsap-*-on-scroll]` 由 IntersectionObserver（threshold .25）触发同一套预设；段落按行 yPercent 50→0（错峰 .035s）；滑入 y 64→0；放大 scale 0→1 | .7s；.5s；.5s | power3.out；back.out(1.2)；back.out(1.4) | js-18 `gsapAnims` |
| 滚动 | 离开首屏后底部吸附购买条：ScrollTrigger `bottom top+=80px` 时 yPercent 105→0，往回滚收起 | .5s | back.out(1.2) | js-08 |
| 滚动 | 图片框内视差：图片在框里随滚动 y 0 → −2 × 溢出量（scrub），只在 no-preference 下开 | 随滚动 | none | js-06、js-09 |
| 悬停 | 口味卡时间线：瓶子 yPercent −20、scale .849；瓶底投影 scale 1.3325、y −7、opacity .5；标题 scale .6（原点顶部）；按钮从 yPercent 50 错峰 .05 升起；两侧鹿角和前景配料 scale 0→1；离开时倒放回 0 | .4s（鹿角、配料 .8s）；离开 .4s | power2.out；elastic.out(1, .75)；离开 power1.out | js-07 |
| 悬停 | 普通按钮 scale 1.025；颜色 .3s；Tailwind 默认过渡 .15–.3s | .22–.35s | power2.out；cubic-bezier(.4,0,.2,1) | js-07、css-21 |
| 组件转场 | 配料 / 营养双卡洗牌：前卡 x −190、rotate −8°，后卡 x +380、rotate +8°；0.21s 换 z-index；0.35s 起两张归位（新后卡回到 rotate 10°、scale .9、x 120、y 60）；手机改成 xPercent 0 / −100 横滑 | .35s + .6s；手机 .5s | power3.out + elastic.out(1, .75)；power3.out | js-08 |
| 常驻 | 鹿角花纹底 12s 横移 95px 循环；加载点 opacity .45→1、上浮 .3rem；放射线随机 scale .2–1 往返 | 12s；.8s；.9–2.2s | linear；ease-in-out；power2.inOut | css-21、js-18 `burstRays` |

动效数值表（js-18）：时长 hover .25 / fast .3 / medium .4 / base .5 / slow .8 / title .48 / paragraph .7 / bigCopy 1.25；错峰 text .03 / base .05 / title .028 / paragraph .035 / bigCopy .035；缓动 base power2.out、copy power3.out、hover power2.inOut、elastic.out(1, .75)、back.out 1.2 / 1.4 / 1.7 / 2.5 / 4 五档。组件只写 `data-gsap-title`、`data-gsap-slide-in-on-scroll` 这类属性。

## 交互与组件
- 导航：顶部公告条（可关，延迟 .8s 从 y −56px 进）；固定头 70px：左边鹿角线稿 logo，右边购物篮 48×48 + 菜单 72×48 两个奶油色描边圆角按钮；菜单是全屏覆盖层（手机链接 60px）。
- 首屏：商品图轮播 + 左右奶油色圆形箭头（看图约 60px）；配料图是「浮动食物」组件（参数 speed、floatAmplitude、mouseForce，桌面会被鼠标推开，手机 mouseForce 0、浮动幅度 .1）。
- 表情贴纸：Prismic 里的配料图按 `lottie-<表情>-<配料>` 命名，页面就自动叠一个 dotLottie 表情（7 种：heart_eyes、sassy、grinning、spiral_eyes、cool、sweating、fire_breathing），每种配料有一组位置预设（top / left / width %、旋转、镜像），例如 habanero 用 fire_breathing、宽 70%、转 10°、水平镜像（看图：手机帧 02 的辣椒有眼睛和嘴）。
- 购买模块（只记结构）：规格下拉 + 数量步进器 → 整宽加购按钮（价格在按钮右端）→ 口味描述卡 → 搭配卡 → 4 个「划掉的圆」图标 → 3 瓶组合促销卡（虚线内框 + 圆形箭头）；离开首屏后底部吸附「规格 + 加购」条；购物车抽屉空状态用一小一大两行字（30px + 100px）。
- 配料 / 营养：两个 tab（`aria-pressed`），选中项被一个手绘椭圆圈住（宽 150%、颜色跟主题色、选中 scaleY .75、未选 scale 0、300ms）；两张卡叠放，前卡底是鹿角花纹，切换时洗牌（见动效）。
- 3D 配料传送带（只在桌面，`hidden lg:block`，高约 700px，pointer-events:none）：一个 instancedMesh 放 25 个辣椒，间距 2.5、横向漂 .18 单位/秒循环，每个自转 10°/s、相邻交替倾斜 ±6°；鼠标横向速度（钳在 ±200）给附近的辣椒加转速（影响半径 0.18 × 视口宽 + 2.5），鼠标纵向位置让它们俯仰 ±45°（跟随阻尼 6，离开时 2）；正交相机 zoom 90，drei Stage soft + studio HDRI，开阴影，DPR [1, 1.5]，卸载时主动 loseContext。
- 口味卡：hover 播时间线；手机改成卡片滚过屏幕中线自动播放、离开倒放。
- 无障碍：多个组件用 `gsap.matchMedia("(prefers-reduced-motion: no-preference)")` 包住视差和放射线旋转；减少动态时跑马灯速度归零、视频不自动播；tab 有 aria-pressed，按钮有 aria-label。
- 手机：底部吸附购买条；3D 带隐藏；hover 全部改成滚动触发；H1 70px。

## 质感
- 颗粒 / 噪点：无（summary.grain 全空，没有 feTurbulence，没有噪声函数）。
- 鹿角花纹墙纸：`/images/antler-pattern.webp` 95 × 158px 平铺、opacity .1，`antler-scroll` 12s linear 横移 95px 无限循环，底纹一直在慢慢走。
- 虚线：卡片里内缩约 10px 的 1px 虚线框（看图判断）；条目之间 1px dashed、前景色 30%；`data-gsap-word-lines` 给每行字加一条虚线，宽度 0→100% 用 .5s ease-out 画出来。
- 投影：瓶子下面一条模糊渐变椭圆（blur-xl 24px、高 30px、宽 105%），hover 时放大变淡，假装瓶子离地。
- 手绘元素：椭圆圈选、鹿角线稿、划掉的圆形图标、边缘粗糙的展示字体；3D 用 studio HDRI 柔光；一处 `mix-blend-mode: hard-light`（位置没确认）。

## 可借鉴到巡航永恒
- 学：叠卡洗牌 → 单台车详情页把「参数 / 改装 / 年代档案」做成 2–3 张叠放的冲印照片卡：后卡静止 rotate 10°、scale .9、偏移 (120, 60)；切换时两张反向甩开 .35s，0.21s 换层，再 .6s 归位。elastic 换成不回弹的 power3.out，手机改左右滑 .5s。
- 学：手绘圈选 → 当前 tab、当前镜头用一个红色油性笔椭圆圈住（宽 150%、scaleY .75、300ms），就是摄影师在印样上圈片的动作，和「胶片印样」首页结构是一套语言。
- 学：卡片 hover「离地」→ 列表页 3 台车：车身 yPercent −20、scale .85，地面阴影放大 1.33 倍并淡到 .5，车名缩到 .6 让位，按钮错峰 .05 升起，.4s power2.out，离开 .4s 倒放；手机滚到中线自动播。这个姿态也正好当列表→详情共享元素转场的起点。
- 学：动效数值表 + data 属性 → 巡航永恒全站一张表（时长、错峰、缓动分档），组件只挂 `data-reveal-*`，IntersectionObserver threshold .25 触发；把 back.out 全部换成慢出缓动、时长整体放慢一档。
- 学：3D 零件传送带 → 发动机爆炸图旁边放一条活塞、气门、推杆的 instancedMesh 陈列带：慢速横移 + 自转 10°/s，鼠标扫过时附近零件跟着转、上下移动改变俯仰；正交相机，像摆在陈列柜里。
- 学：离开首屏的吸附条 → 单车页底部「车名 · 排量 · 年份 · 下一台」字幕条，`bottom top+=80px` 时 yPercent 105→0。
- 不学：橙配暖黑（#F15726 配 #100B06 正好踩「避开橙黑」红线）；表情 Lottie、back.out 回弹、字符压扁弹出（太卡通，不合黑白电影）；所有动画元素默认 opacity:0 等 JS 放出（这次桌面首屏 6s 全黑）；购物车、价格、Shopify 只记结构。

## 没读到
- 桌面首屏：0.5–6s 只拍到公告条，其余全黑。CSS 让所有 `[data-gsap-*]` 默认 opacity:0，由 JS 时间线（延迟 100ms）放出，这次 6s 内没放出，原因没确认；按「只扫一次」没有重扫，桌面首屏布局只来自源码。
- 创始人视频：player.vimeo.com 返回 401 + Cloudflare 挑战（Vimeo 自己的），没看到视频内容，没绕。
- 订阅表单的 Cloudflare Turnstile 脚本 302 / 401，没碰。
- 3D 传送带：源码读全了，但无头截图里没渲染出辣椒，实际观感和帧率没验证。
- 卡片洗牌、口味卡 hover、tab 圈选只读了源码，没真的触发；弧形标题在手机帧里看到了。
- 路由级转场：React 里有 View Transitions 代码，没确认站点启用。
- Peperoncino Sans 的出处和授权。
