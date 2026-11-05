# Immersive Garden 拆解

> 网址：https://immersive-g.com/ （另扫项目页 /projects/girard-perregaux-casquette-1） ｜ 类型：工作室作品集 ｜ 拆解：2026-11-04 ｜ 证据：桌面 57 帧、手机 16 帧（首页 2 次、项目页 2 次）、CSS/JS 51 个文件（去重，另有内联 29 个）

## 一句话
整个站是一个连续的 3D 场景：首页是一面白色石膏浮雕墙，项目影像像挂画一样浮在浮雕之间，衬线大字慢慢显影；进项目页时镜头推近、浮雕爆开、淡到黑。让人记住的是“走进一个展厅”，不是“翻网页”。

## 架构与技术栈
- 框架 / 渲染：Nuxt 3 + Vue 3（证据：summary.framework、`/_payload.json`、asyncData 分包）；全屏 WebGL2 canvas 1440×900 垫在 DOM 下（summary.canvases）；three r151（window.__THREE__）。
- 动效 / 滚动 / 3D / 声音库：GSAP 3.12.5 打包在 entry.js（hits.versions），同包有 CustomEase、SplitText（版本串 3.2.4）；Lenis 1.1.14 打包（`var ioe="1.1.14"`），桌面 `lerp:.05`、手机 `lerp:1`、`syncTouch:true`，外层还有自写滚动管理器（`smoothAmount:.035, deltaMultiplier:.1`），所以原生 scrollY 一直是 0；Howler（window.Howler）；vue3-lottie 做菜单图标；detect-gpu@5.0.60（unpkg）按 GPU 档位选模型（`reliefs_high` / 低档或手机用 `reliefs_low`）；Draco + Basis(KTX2) wasm 解码；文字在 WebGL 里用 MSDF 渲染（/webgl/msdf/PSTimes、TimesNow 数字、Helvetica 三套 json）；后期 CustomUnrealBloomPass。
- 托管与内容来源：Vercel（响应头 server: Vercel、x-vercel-cache: HIT）；媒体在 DigitalOcean Spaces（ig-medias-prod.ams3）。媒体对象带 `formats.small`（手机取 small）、文件名带 10 位哈希后缀，推测 Strapi（未证实）。项目缩略图预转 KTX2（20–110KB/张），预览视频 MP4 0.5–3.6MB。
- 页面结构：首页 = 加载页（logo + 标语 + 进度数字）→ WebGL 标题「Transcend anything…」噪声显影 → 18 个 projectBlock（每块一段 36px 衬线简介 + 1–2 个影像平面）→「Our mission」居中宣言 → 黑色页脚（暗色花卉浮雕 + 44px 邮箱）。
- 项目页结构：顶部居中 Back → 居中 56px 三行标题 + 下划线「Launch website」→ 通栏影像 → 文字块（左 28px 摘要；右 22px 说明 + 灰色服务清单）→ 单张/成对影像左右错落 → 居中灰色过渡句 → pushBlock（100vh）→「See next project」44px → 黑色页脚。右侧中线常驻「See backstage」进幕后子页。
- 路由与转场：/projects/<slug>、/projects/<slug>/backstage/、/projects/、/the-studio/；不整页刷新，3D 场景接住转场（见动效）。

## 视觉与配色
| 角色 | 色值 | 用在哪 |
|---|---|---|
| 底色 | #E8E8E8 | body、加载页底（bodyBg、.introLoader__bg） |
| 主文字 | #030303 | 标题、正文（textColors ×464） |
| 反白 | #E8E8E8 字 / #030303 底 | 页脚、backstage、darkBg 模式 |
| 进度线 | currentColor 不透明度 .2 | 加载页 1px 进度线 |
| 暖灯 | #E6CCA2（主灯）/ #362C1F（泛光）/ #F5A75F（淡出） | WebGL 浮雕的灯光与转场（config.components.light） |
| 次级灰 | #A6A6A6 | 快速浏览模式的点阵光标 |
| 按钮流体 | #7ABFC5（rgb 122,191,197） | 按钮悬停的流体 + 色散效果基色（fluidEffect.baseColor） |

## 字体与排版
| 角色 | 字体 | 字号 1440 / 390 | 字重 | 行高 / 字距 | 大小写 |
|---|---|---|---|---|---|
| 首页 / 项目页 H1 | PSTimes | 56px（3.8889vw）/ 44px | 400 | 1.1 / 0 | 句首大写 |
| 宣言句、See next project | PSTimes | 44px（3.0556vw）/ — | 400 | 1.1 / 0 | 句首大写 |
| 项目简介 | PSTimes | 36px（2.5vw）/ 28px | 400 | 1.2 / 0 | 句首大写 |
| 加载页标语、项目摘要 | PSTimes | 28px（1.9444vw）/ 28px | 400 | 1.2 / 0 | 句首大写 |
| 正文、服务清单 | PSTimes | 22px（1.5278vw）/ 20px | 400 | 1.4（清单 1.7）/ 0 | 句首大写 |
| UI 小字（Menu、Back、See backstage、Off） | HelveticaNeueRegular | 14px（.9722vw）/ 14px | 400 | 1.1 / 0 | 句首大写 |
| 光标标签 | HelveticaNeueRegular | 12px（.8333vw） | 400 | 1.1 / 0 | 句首大写 |
| pushBlock 大按钮 | HelveticaNeueLight | 42px（2.9167vw） | 300 | 1.3 / -.01em | 句首大写 |

授权：PSTimes 来源不明（Times 系衬线，自托管 woff2 50KB，推测商用）；Helvetica Neue 商用（Linotype/Monotype）；WebGL 数字用 Times Now（商用）。
- 排版手法：只有两种声音——衬线负责“说话”，14px 无衬线负责“操作”。全站没有粗体、没有全大写、没有加字距。字号一律写成 px÷1440×100vw，≥1915.2px 锁成固定 px，≤1309 / 1113px 等断点再换回固定 px（等比缩放带上下限）。正文永远窄栏（12 栏里占 3 栏，左右交替），段与段之间 20vh 留白。加载页三样东西同一行：logo（第 2–4 栏）、标语（第 4–10 栏）、进度数字。

## 布局
- 12 栏，栏距 16px（summary.grids ×62），最大宽 2088px；页边距约 135px@1440（看图判断）。
- 断点：1915.2 / 1440 / 1309 / 1183 / 1113 / 1024 / 768 / 560 / 432px，另有横竖屏组合（hits.mediaQ）。
- 首页是“墙 + 挂画”：项目块内间距 11.11vw（160px），项目之间 36.11vw（520px），空白由 WebGL 浮雕填满；手机缩成 48px / 156px。
- 项目页是窄栏长文：区块之间 16.67vw（240px），文字块 3 栏宽、左右交替（align--center-left / center-right），pushBlock 满 100vh、上下各 20vh 内边距。

## 动效
| 触发 | 效果 | 时长 | 缓动 | 实现 |
|---|---|---|---|---|
| 开场 | 延迟 1s 后：logo 由 SVG mask 矩形 x -66%→0 擦出；标语按词淡入（stagger .1）；进度数字 0→100，先给 5–10% 随机假进度 | 2.5s / 1s / .8s | quart.out / sine.inOut | GSAP（IntroLoader 组件） |
| 开场结束 | 加载页整体淡出 | 1.2s | sine.inOut | GSAP |
| 首页标题 | MSDF 标题按噪声场“长出笔画”（thickness -1→4）、淡入、遮罩三层同时走 | 各 6s | cubic-bezier(0,.016,.578,.972) 等三条 | 着色器 + GSAP（home config.title） |
| 滚动 | 虚拟滚动；桌面 Lenis lerp .05（很重很慢），手机 lerp 1（不插值） | 连续 | — | Lenis 1.1.14 |
| 滚动 | 段落进视口：整段 y 50→0；按行淡入（stagger .1）；退回视口下方时复位 | 2s / 1.8s | power2.out / power1.out | AnimatedParagraph（SplitText 拆行） |
| 滚动 | 项目文字与媒体透明度过渡 | 1.9s | ease-in-out | CSS transition（computed ×54） |
| 悬停 | 文字链接透明度 1→.6 | .6s | 进 cubic-bezier(.165,.84,.44,1)；出 cubic-bezier(.77,0,.175,1) | CSS |
| 悬停 | 浮雕被鼠标推开（mouseRadius 1.7、mouseRepulsion 1.13） | 实时 | — | 着色器 |
| 转场 首页→详情 | 相机推近 z→-1；浮雕“爆开” explodeProgress 0→1；淡到黑；灯色淡出；返回时倒放 | 1.3s / 1.3s / 1.2s / 1s | power2.in / cubic-bezier(.519,.068,.931,1.028) / sine.inOut | GSAP + 着色器（hideToDetail） |
| 转场 DOM | 新页淡入 / 旧页淡出 | 1s（延迟 .25s）/ .5s | ease | Vue Transition（.v-enter-active） |
| 快速浏览 | 「See all projects」：UI 相机放大 4 倍、文字缩到 .5 并淡出、滚轮倍率加大 | 1.5s | 自定义 "fast" | GSAP（_goFastAnimation） |

## 交互与组件
- 导航：右上「Menu」+ 22×22 logo 图标（canvas，黑白两套随底色切换），菜单图标动画是 Lottie JSON（blackIn / whiteIn / blackOut / click）。左下常驻「See all projects」，右下常驻声音开关「Off」。
- 光标：自写双层（cursor__follow + cursor__ease 两层不同速度跟随，可挂 12px 文字标签，偏移 35/15px），深底自动反白；滚动提示是 3px 圆点动画。
- 加载页：全屏 #E8E8E8，进度条是 1px 线（scaleX，透明度 .2）；结束时上报加载秒数到统计。
- 声音（Howler）：右下显示「Off」，判断默认静音。三层：① 每页一条环境音乐循环（Home / Focus / About 三条，preload:false，切页 2000ms 交叉淡化）；② 9 个 UI 音效打包成一个 actions.mp3 sprite（如 Hover_Menu-Item 1.38s、Click-allprojectCTA 4s）；③ 转场雷声、10s 大理石裂响循环。监听 visibilitychange 处理后台标签页。
- backstage 幕后子页：全屏视频 + 左侧固定的分章节 1px 时间轴（章节间隔 4px、clip-path 展开、悬停时章节 .15→.25、进度 .5→1，.7s cubic-bezier(.39,.575,.565,1)）+ 底部居中 16px 衬线字幕（读 .srt 文件，SplitText 拆行，距底 120px）。
- 状态记忆：滚动位置写进 localStorage「scrollManager」，从详情返回停在原处。
- 手机：同一套 WebGL，用低档浮雕模型和 small 媒体；Lenis 不插值；段落最大宽 250px、段间 30vh；底栏仍是「See all projects / Off」。

## 质感
- 没有胶片颗粒叠层（feTurbulence 0 个、无 grain 类名、无混合模式）。
- 质感全来自 3D：石膏浮雕 glb（高档 / 低档 4.7MB）+ 法线贴图 + 暖灯 #E6CCA2 + 2048 阴影贴图 + 自定义 Bloom（泛光色 #362C1F）。
- 噪声只用在遮罩和扰动：tMaskNoise / uNoise 做溶解遮罩，uvHoverNoise 做悬停扰动，fastScrollNoise 做快速滚动扭曲；按钮有流体 + 菲涅尔色散（chromaticMask fresnelSharpness 35、opacity .98）。

## 可借鉴到巡航永恒
- 学：一个场景贯穿全站，换页 = 镜头运动（相机推近 1.3s power2.in + 场景淡到黑 1.2s sine.inOut，返回倒放）→ 用在首页「一部短片」到车型详情页：点 X48 那格，镜头推进、黑场，再亮起就是详情页首屏，像剪辑里的黑场转场；列表到详情的共享元素转场也按这个时长做。
- 学：加载页当片头字卡（延迟 1s；字标 mask 擦出 2.5s quart.out；标语按词淡入 stagger .1；进度数字先给 5–10% 假进度）→ 巡航永恒片头：ETERNAL CRUISE 字标擦出，中英标语逐词出现，右侧等宽小字 0→100。
- 学：字幕式段落显影（整段上移 50px 2s power2.out + 按行淡入 1.8s stagger .1，窄栏 3/12 左右交替，段间 20vh）→ 年代史、人与公路的旁白段落，读起来像一句一句的电影旁白。
- 学：backstage 子页 = 全屏视频 + 分章节 1px 时间轴 + 底部 .srt 字幕 → 做「冷启动」或「改装过程」幕后片，直接对应电影方向；字幕文件可同时当无障碍字幕。
- 学：声音分三层且默认静音（页面环境音循环、切页 2s 交叉淡化；UI 音效合成一个 sprite；少量转场音）→ 环境层放怠速，转场放一次轰油门，悬停用短促机械声；右下常驻「声音 关」。
- 学：字号按 1440 稿写 vw，再在大屏 / 小屏锁 px → 宽银幕黑边画幅里字号跟着画面等比走。
- 不学：主包 2.4MB、首访 fetch 26MB + 视频 24MB，软件渲染下加载页要 9–20 秒；CSS/JS 里没有任何 prefers-reduced-motion 处理；标题 DOM 文字 visibility:hidden、画面交给 WebGL（读屏补偿未查）。巡航永恒要保留真实 DOM 文字，WebGL 只当装饰层，并给「减少动态效果」一条静帧路径。
- 不学：白底石膏的亮调材质（和黑白电影的暗调相反），只借“场景贯穿 + 镜头转场”的思路。

## 没读到
- 桌面首页的 WebGL 标题显影只在手机帧里拍到（immersive-g\frames\mobile-scroll-06），桌面帧从加载页直接跳到第一个项目。
- 真实 GPU 下的加载时长、帧率、浮雕跟随鼠标的手感：扫描用无头 Edge + SwiftShader 软件渲染，首页开场空白 3–6 秒、项目页第一次等 9 秒仍卡在加载页（第二次等 22 秒才进），真机应短得多，未测。
- 多数视频平面在帧里是白色矩形（纹理没加载完），播放与悬停效果没看到；声音没有实际听。
- 菜单展开态、about、projects 列表、backstage 页没有扫帧（backstage 只读了 CSS/JS）。
- Strapi 是推测；Lenis 和外层自写滚动管理器的分工没完全读清。
